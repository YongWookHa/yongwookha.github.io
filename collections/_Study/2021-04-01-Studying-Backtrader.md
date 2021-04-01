---
layout: post
title: 알고리즘 트레이딩 준비하기 - 백테스팅
subtitle: feat. Backtrader
tags: [LIFE, DEVELOP]
cover-img: /assets/img/trading_graph.jpg
comments: true
---

Backtrader는 Quant Algorithm Trading에서 매우 중요한 Backtesting을 도와주는 오픈소스 라이브러리입니다. 직접 짠 매수매도 전략을 과거의 데이터에 시뮬레이션해보고 전략의 유효성을 실험하는데 이용합니다. 이 작업을 효율적으로 수행하기 위해 Backtrader에 적용된 몇가지 개념에 대해 정리합니다. 꽤나 정교하게 구성된 큰 라이브러리이기 때문에 이번 포스트에서는 사용 방법과 관련된 핵심적인 일부에 대해서만 다룹니다.

대부분의 자료는 https://backtrader.com/의 공식 문서에서 참조했습니다.

## Cerebro

```python
import backtrader as bt
cerebro = bt.Cerebro()
```

Backtrader의 주춧돌 역할을 하는 메인 클래스

1.  모든 입력을 모아주는 센터입니다. 가능한 입력들은 다음과 같습니다.
    - DataFeeds: `과거 주가 데이터`
    - Strategies: `매수매도 전략`
    - Writer: `문서화`, Analyzer: `분석`, Observer: `감시`
    - Broker: `거래소`
    - Notificatio: `알림`
2.  과거 주가 데이터, 혹은 실시간 데이터에 대해 Backtesting을 수행합니다.
3.  결과를 반환합니다.
4.  그래프 그리기 등의 외부 라이브러리에 대한 접근을 제공합니다.

## Data Feeds (GenericCSVData)

Cerebro에 데이터를 입력하는 방법은 여러가지가 있습니다. 대표적으로는 Yahoo Finance에서 온라인으로 데이터를 받아오는 방법이 있고, Pandas 라이브러리 등을 통해서도 데이터를 입력받을 수 있습니다. 저는 csv 형식으로 데이터를 1차 가공한 후에 이용하기에, 이에 적합한 `GenericCSVData`를 이용합니다. CSV 데이터에 포함된 `datetime`, `time`, `high`, `row`, `open`, `close`, `volume`과 같은 데이터의 위치와 양식을 명세합니다.

```python
import backtrader as bt

cerebro = bt.Cerebro()
data = bt.feeds.GenericCSVData(
            dataname="Bitcoin_24h.csv",

            fromdate=datetime.datetime(2017, 7, 1),
            todate=datetime.datetime(2020, 11, 20),

            nullvalue=0.0,

            dtformat=('%Y-%m-%d'),
            tmformat=('%I-%p'),

            datetime=1,
            time=2,
            high=5,
            low=6,
            open=4,
            close=7,
            volume=9,
            openinterest=-1,

                        headers = True,
                        separator = ','
        )

cerebro.adddata(data)
```

## Strategy

Cerebro에 매수 매도 전략을 전달합니다. `strategy`는 다음과 같은 생애주기가 있습니다.

1.  Initialize (`__init__`) : 전략에서 이용하는 지표들(`indicators`) 정의
2.  Birth (`start`) : World(`cerobro`)에게 시작을 알림.
3.  Childhood (`prenext`) : Window Size가 아직 차지 않은 초기 단계.
4.  Adulthood (`next`) : Window Size만큼의 데이터를 축적하여 본격적인 예측을 할 수 있는 단계.
5.  Reproduction : Parameter Optimizing 등을 위해 전략을 복사할 때 이용.
6.  Death (`stop`) : 종료

아래는 15개 시간단위에 대한 이동평균선에 기반한 매수매도 전략 탬플릿입니다.

```python
class MyStrategy(bt.Strategy):

    def __init__(self):
        self.sma = btind.SimpleMovingAverage(period=15)

    def next(self):
        if self.sma > self.data.close:
            # Do something
            pass

        elif self.sma < self.data.close:
            # Do something else
            pass
```

보통의 경우, `Strategy`는 위와 같이 `__init__`, `next`로 구성됩니다. `next`에 상세 전략을 코딩하고, 필요하다면 헬퍼 함수를 추가합니다. `start`, `prenext`, `next`, `stop`은 overide 하지 않습니다.  

우리에게 깔끔한 코드를 작성하는 것보단 효과적인 전략을 설계하는 일이 더 중요하게 느껴질 수 있습니다. 하지만 하나의 전략만으로 완벽한 트레이딩을 할 수는 없습니다. 다양한 전략을 빠르게 실험하기 위해 Strategy 클래스에서 다음 데이터가 들어왔을 때 프로그램이 어떤 동작을 하는지 정의하는 `next`를 능숙하게 코딩하는 것이 필요합니다. 

따라서 우리는 `next`에 이용되는 요소들에 대해 진지하게 알아보는 것이 좋습니다. 깔끔한 코드는 지속적인 관리를 용이하게 해줄겁니다. 

`next`에서 정해진 조건에 도달했을 때 주문을 하는 방법은 `buy`, `sell`, `close`, `cancel` 메소드를 이용합니다. `Buy`와 `Sell` 메소드는 `Order`를 리턴합니다.

- data (default: `None`) : 어떤 데이터에서 생성되었는가? (World에 여러 데이터가 있는 경우)
- size : 주문량
- price : 가격
- exectype : 조건부 발동 (`Order.Market`: 시장가, `Order.Limit`: 다음틱 등)
- plimit, tradeid, ..

내가 만든 strategy를 실제 트레이딩에 적용하여 알고리즘 매매를 하는 경우, 자리에 앉아서 프로그램의 동작을 하루종일 예의주시하려는 목적은 아닐 겁니다. 따라서 프로그램이 주문을 하거나, 장이 열리고 끝나거나, 잔고가 특정 지점에 도달하는 등의 이벤트가 발생할 때, 이를 알릴 수 있는 방법을 제공합니다. 이를 `Order Notification`이라고 합니다.

`Order Notification`은 `notify_order`, `notify_trade`, `notify_cashvalue` 등의 메소드를 이용합니다. 메소드를 overide하여 로그를 남기거나 _Slack_ 혹은 _Telegram_ 과 같은 메신저를 연결해 모바일로 푸쉬 알림을 발생시킬 수도 있습니다. 각 notification은 `next`가 한번 실행될 때 마다 함께 실행됩니다.



