---
layout: post
title: Image Attention 정리
subtitle: feat. Pytorch
tags: [MACHINE_LEARNING, IMAGE]
cover-img: /assets/img/image-attention-heatmap.jpg
comments: true
---

이미지 분석에서 Attention의 부산물인 Score를 이용하면 네트워크가 집중(_Attention_)하고 있는 영역을 시각적으로 표현 가능합니다. 이를 참고하면 네트워크의 동작을 조금 더 직관적으로 이해할 수 있습니다. Attention은 `Soft Attention`과 `Hard Attention`으로 나뉘는데, 이번 포스트에서는 거의 대부분의 Image Attention에서 이용하는 Soft Attention에 대해 코드와 함께 간략하게 정리해보려고 합니다.

### Soft Attention
- **Dot**
Dot 방식은 `decoderPrevHidden`과 `encoderOutputs`를 바로 dot 연산하는 방법으로, Linear등을 쓰지 않는 가장 Plain한 방법입니다. 하지만 Encoder의 입력이 이미지인 경우, `H'*W'==dec_hidden_size`를 만족시켜줘야하기 때문에 사용하기 어렵습니다.
$score_{alignment} = H_{encoder} \cdot H_{decoder}$
```
class Attention(nn.Module):
	def __init__(self)
		super(Attention, self).__init__()
	
	def forward(self, decoderPrevHidden, encoderOutputs):
		'''
		decoderPrevHidden : (B, dec_hidden_size)
		encoderOutputs : (B, H'*W', enc_hidden_size)
		'''
		assert decoderPrevHidden.size(1) == encoderOutputs.size(1)
		out =  torch.bmm(decoderPrevHidden.unsqueeze(1), encoderOutputs).squeeze(-1)
		return out
	
```

---
- **General**
Dot 방식에서 Shape 일치 문제를 손쉽게 해결할 수 있는 방법입니다. 주의해야할 점은, `self.d2h`와 `self.e2h` 가 `bias=False` 로 선언되어야한다는 점입니다. bias로 인해 `decoderPrevHidden` 혹은 `encoderOutputs`가 과도하게 훼손되는 것을 방지하기 위함입니다.
$score_{alignment} =W( H_{encoder} \cdot H_{decoder})$
```
class Attention(nn.Module):
	def __init__(self, dec_hidden_size, encoderOutputSize, attn_hidden_size)
		'''
		encoderOutputSize : (B, H'*W', enc_hidden_size)
		'''
		super(Attention, self).__init__()
		self.d2h = nn.Linear(dec_hidden_size, attn_hidden_size, bias=False)
		self.e2h = nn.Linear(encoderOutputSize[1], attn_hidden_size, bias=False)
	
	def forward(self, decoderPrevHidden, encoderOutputs):
		'''
		decoderPrevHidden : (B, dec_hidden_size)
		encoderOutputs : (B, H'*W', enc_hidden_size)
		'''
		dec2attn = self.d2h(decoderPrevHidden).unsqueeze(1)  # (B, 1, attn_hidden_size)
		enc2attn = self.e2h(encoderOutputs.permute(0, 2, 1))  # (B, enc_hidden_size, attn_hidden_size)
		enc2attn = enc2attn.permute(0, 2, 1)  # (B, attn_hidden_size, enc_hidden_size)
		
		out = torch.bmm(enc2attn, dec2attn).squeeze(-1)  # (B, enc_hidden_size)
		return out
```  
---
- **Concat**
Concat 방식이 이미지 Encoder에 대한 Attention을 수행할 때 가장 적합한 방식입니다. `decoderPrevHidden`과 `encoderOutputs` 입력값에 변형을 주지 않고, 두 입력을 concatenate하여 바로 Fully Connected Layer에 입력시킵니다. 이 Fully Connected Layer는  Multi Layer Perceptron(MLP)을 이용할 수도 있습니다. 결과로는 현재 타임 슬라이스 `t`에서의 `context`와 이미지의 그리드에 대한 `Attention score`를 얻을 수 있습니다.
$score_{alignment} = W \cdot tanh(W_{combined}(H_{encoder} + H_{decoder}))$
```
class Attention(nn.Module):
	def __init__(self, dec_hidden_size, encoderOutputSize)
		'''
		encoderOutputSize : (B, H'*W', enc_hidden_size)
		'''
		super(Attention, self).__init__()
		fc_inp_size = encoderOutputSize[1]*encoderOutputSize[2]+dec_hidden_size
		self.fc = nn.Linear(fc_inp_size, encoderOutputSize[1])
	
	def forward(self, decoderPrevHidden, encoderOutputs):
		'''
		decoderPrevHidden : (B, dec_hidden_size)
		encoderOutputs : (B, H'*W', enc_hidden_size)
		'''
		enc_flatten = encoderOutputs.view(encoderOutputs.size(0) -1)  # (B, H'*W'*enc_hidden_size)
		attn_inp = torch.concat([decoderPrevHidden, enc_flatten)], dim=1)  # (B, fc_inp_size)
		score = F.softmax(torch.tanh(self.fc(attn_inp)), dim=1) # (B, H'*W')
		
		context =  torch.bmm(score, encoderOutputs).squeeze(-1)  # (B, enc_hidden_size)
		return context, score
```