---
layout: post
title: 유용한 Linux snippets
tags: [DEVELOP, LINUX]
cover-img: /assets/img/linux.jpg
comments: true
---

처음 시작이 Windows였기 때문인지, 저는 OS로는 Windows가 좋습니다. 특히 Windows 10은 MS가 마지막 버전이라고 공표했듯, 완성형이라 생각합니다. 요즘에는 클라우드를 이용하는 일이 많다보니, 자연스럽게 데이터를 다루거나 모델 실험에도 리눅스를 자주 이용하게 됩니다. 그래도 개발과정은 vscode-insider를 통해 윈도우 환경과 똑같이 세팅하기 때문에, 컴퓨터를 전공했음에도 사실 리눅스 OS가 완전히 익숙하지는 않습니다. 머릿속에는 정말 기본적인 것들만 있다보니 조금만 뭔가를 하려고 하면 검색부터 해야할 때가 많습니다. 매번 검색해서 '지난번에 썼던 커맨드였는데.. 뭐였지?' 했었던 경우들을 이 포스트에 정리하려고 합니다.

## 🔧 Unzip  
현재 디렉토리 내의 모든 zip 파일들을 각각 파일명으로 압축풀기  
![](https://www.dropbox.com/s/5gfgb1754wfqbi2/%EA%B0%81%EA%B0%81_%ED%8C%8C%EC%9D%BC%EB%AA%85_%ED%8F%B4%EB%8D%94%EC%97%90_%ED%92%80%EA%B8%B0.jpg?raw=1)    
```bash  
for file in `ls *.zip`; do unzip "${file}" -d "${file:0:-4}"; done
```

`bz2`로 고효율 압축하기 (멀티코어)
```
# 압축
tar --use-compress-prog=pbzip2 -cvf <압축 파일 이름> <압축할 파일 또는 폴더>
```

```
# 압축 해제
tar -I lbzip2 -xf <압축 파일>
```

## 💾 Disk Usage  
```bash  
du -sh .  # 현재 폴더의 사용 용량
du -h -d 1  # (--max-depth 1) 폴더별
df -h  # 파티션별 확인  
```

## 🎫 Num of Files  
```bash  
ls . | wc -l  # 현재 폴더에 있는 파일과 폴더의 개수
```  

## 🔏 Permission 
```bash  
- - - = 0
r - - = 4
r - x = 5
r w - = 6
r w x = 7

chmod OwnerGroupOthers FileOrDir

chmod 700 SomeFile  # 소유자만 읽기 쓰기 실행 가능
chmod 744 SomeFile  # readonly
chmod 766 SomeFile  # read and write only

chmod -R 700 SomeDir  # Recursive 적용  
```

## 🙋‍♂️ User  
```bash
sudo groupadd RnD  # 그룹 추가
sudo adduser 계정이름 # 계정 추가
sudo adduser 계정이름 그룹이름   

```

## 💻 Device Information  
```bash
cat /proc/cpuinfo  # CPU 확인
lspci | grep -i VGA  # GPU 확인
```

## 📥 Download  
```bash
# web
curl -O <URL>

# FTP
curl -u <FTP_USERNAME>:<FTP_PASSWORD> <ftp://<HOST>:<PORT>/<FILE_DIR>
```  

> - curl <url> : url의 소스코드 출력  
> - curl -o <filename> <url> : <url>의 소스코드를 <filename> 이름으로 로컬에 저장
> - curl -O <url> : <url>의 소스코드를 원래 이름으로 로컬에 저장

## 📤 Upload
```bash
curl -T <LOCAL_FILE> -u <FTP_USERNAME>:<FTP_PASSWORD> <ftp://<HOST>:<PORT>/<FILE_DIR>
```  

## ⚡ API TEST  
```bash
curl -F "image=@image.png" http://127.0.0.1:5000/api/v1/
```

## 📡 SFTP

```bash
sftp -P <port> <host-ip>
# cd <dest-directory>
put <local-file>
```