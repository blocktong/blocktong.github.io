---
title: "암호화폐의 스캘핑 전략"
description: ""
coverImage: "/assets/img/2024-05-05-ScalpingStrategyforCryptocurrency_0.png"
date: 2024-05-05 15:06
ogImage: 
  url: /assets/img/2024-05-05-ScalpingStrategyforCryptocurrency_0.png
tag: Tech
originalTitle: "Scalping Strategy for Cryptocurrency"
link: "https://medium.com/coinmonks/scalping-strategy-for-cryptocurrency-76207939dff1"
---


## EODHD API를 사용하여 Bitcoin 데이터에 스캘핑 전략 적용하기

![Scalping Bitcoin](/assets/img/2024-05-05-ScalpingStrategyforCryptocurrency_0.png)

## 스캘핑 전략이란?

이 개념은 듣기만큼 복잡하지 않습니다. 시장에 있다고 상상해보세요. 한 가격에 물건을 사고 빨리 약간 높은 가격에 다른 사람에게 판매할 기회를 발견합니다. 그것이 바로 거래에서의 스캘핑이 하는 일입니다. 시장의 작은 가격 움직임을 이용해 빠르고 소규모 수익을 창출하는 것입니다.



이제 암호화폐 거래에 적용하는 것을 생각해보겠습니다. 특히 비트코인의 경우, 초고속 디지털 시장에서 이를 진행한다고 상상해보세요. 비트코인과 같은 암호화폐는 가격이 매우 빠르게 변동할 수 있으며 짧은 시간 내에도 꽤 크게 변할 수 있습니다. 이는 스캘핑에 적합합니다. 적은 양의 비트코인을 구매한 다음, 가격이 약간 상승하자마자 소량 이익을 내고 팔아야 합니다. 그리고 다시 그렇게 반복하며 하루에 여러 번 거래를 합니다.

이를 제대로 수행하려면 짧은 간격으로 거래해야 합니다. 전형적인 스캘핑 접근 방식의 경우, 트레이더들은 주로 1분, 5분 또는 15분과 같은 짧은 시간프레임을 사용하는데, 이는 스캘핑이 매우 짧은 시간 내에 시장 움직임에서 이익을 얻으려는 것이므로입니다. 1분 시간프레임은 하루 거래 전체를 통해 다양한 소규모 가격 움직임을 포착할 수 있는 잠재력으로 매우 인기가 있습니다. 그러나 시간프레임의 선택은 트레이더의 특정 전략, 리스크 허용능력 및 거래되는 시장에 따라 다양할 수 있습니다. 이번 시연에서는 EODHD APIs에서 제공하는 1분 데이터를 사용하겠습니다.

## 비트코인 스캘핑의 장점

- 빠른 수익: 빠르고 민첩하다면 하루 내내 소규모 이익을 상당히 쌓을 수 있습니다. 개발한 암호화폐 거래 봇 PyCryptoBot은 이를 자동화하는 데 완벽합니다. 웹소켓을 사용할 수도 있습니다.
- 즐거움: 꽤 즐겁습니다! 언제나 기회를 찾아 신속하게 결정을 내려야 합니다.
- 레버리지 사용: 일부 트레이더들은 레버리지를 사용하여 거래력을 증폭시키고 소규모 가격 변동으로부터 이익을 증가시킬 수 있지만, 이는 일반적으로 암호화폐 거래소에는 적용되지 않습니다. 하지만 IG와 같은 플랫폼에서는 가능하지만 전문 계정이 필요하며 상당한 리스크가 있으므로 주의해야 합니다.



## 비트코인 스캘핑의 단점

- 고위험: 보상 가능성이 높을수록, 특히 레버리지를 사용하는 경우 손실도 증폭될 수 있습니다.
- 수수료 부담: 매번 거래를 할 때마다 수수료가 발생합니다. 부주의하면 수익을 갉아먹거나 이긴 전략조차 패배로 변할 수 있습니다. 수수료는 전략 논리에 반영되어야 합니다.
- 스트레스와 시간 소모: 여유롭지 않습니다. 항상 준비돼 있어야 하며, 상당히 집중력을 필요로 하는 거래 방식입니다.

수수료에 대해 얘기해보죠. 이것은 계산에 꼭 고려해야 할 중요한 부분입니다. 거래를 할 때마다 거래소가 약간의 수수료를 차감합니다. 스캘핑이라면 자주 거래를 하기 때문에 작은 수수료가 많이 발생합니다. 주의를 기울이지 않으면 수익을 올리기 위해 움직여야 할 가격이 충분하지 않을 수도 있습니다. 이러면 움직이지 않고 제자리에 서 있기 위해 열심히 움직이고 있는 느낌이 들 수도 있습니다. 혹은 더 나빠진 경우, 돈을 잃을 수도 있습니다. 네, 수수료에 대해 자주 언급하지만 중요성을 강조하기에 그만이 없습니다.

요약하자면, 비트코인 스캘핑은 심리적으로 힘든 것으로 여겨집니다. 변동성이 높아 흥미로울 뿐만 아니라 이윤을 얻을 수 있지만, 손실의 영향, 특히 거래 전략에 미치는 수수료의 영향을 잘 파악해야 합니다. 실제 돈이 걸린 아주 빠른 속도의 비디오 게임을 하는 것과 비슷합니다. 잘 짜여진 전략이 필요하며, 뚜렷한 통제력이 필요하며, 냉정함을 유지해야 합니다.



## 전략 개요

- 시간 단위: 1분 데이터.
- 매수 신호: 빠른 SMA(5기간)가 느린 SMA(12기간)를 상향 돌파하면 상승 모멘텀이 나타납니다. 우리는 신호 캔들의 종가에 매수합니다.
- 매도 신호: 빠른 SMA(5기간)가 느린 SMA(12기간)를 하향 돌파하면 하락 모멘텀이 나타납니다. 우리는 신호 캔들의 종가에 매도합니다.
- 거래 수수료: 각 거래에서 차감되는 왕복(매수 및 매도) 거래 수수료를 가정합니다. Coinbase Advanced Trade의 0.6% 테이커 수수료를 가정합니다.
- 재투자: 수수료를 고려한 이익을 모두 재투자합니다.
- 실행: 전략은 신호에 이어 종가에서 실행됩니다.
- 초기 투자: 백테스트는 정의된 초기 투자 금액으로 시작됩니다.

## 데이터 검색 중...

EODHD APIs에서 1분 단위 BTC-USD 데이터를 검색하겠습니다.



```python
import pandas as pd
from eodhd import APIClient
import config as cfg

api = APIClient(cfg.API_KEY)

def get_ohlc_data():
    df = api.get_historical_data("BTC-USD.CC", "1m", results=120)
    return df

if __name__ == "__main__":
    df = get_ohlc_data()

    df["close"] = pd.to_numeric(df["close"], errors="coerce")
    df.dropna(subset=["close"], inplace=True)
    df["close"].fillna(value=df["close"].mean(), inplace=True)

    print(df)
```

![Chart](/assets/img/2024-05-05-ScalpingStrategyforCryptocurrency_1.png)

## 기본 전략 적용

이것은 기본적인 스캘핑 전략의 예입니다. 세부 조정을 권장드리며, 이후에 이를 백테스트하는 방법을 보여드릴 예정이니 결과에 미치는 개선 사항을 평가할 수 있을 것입니다.
  



```js
fast_sma_period = 5
slow_sma_period = 12
df["fast_sma"] = df["close"].rolling(window=fast_sma_period).mean()
df["slow_sma"] = df["close"].rolling(window=slow_sma_period).mean()

df["signal"] = 0  # default
df.loc[df["fast_sma"] > df["slow_sma"], "signal"] = 1  # buy signal
df.loc[df["fast_sma"] < df["slow_sma"], "signal"] = -1  # sell signal

print(df)
```

![Image for Strategy](/assets/img/2024-05-05-ScalpingStrategyforCryptocurrency_2.png)

## 전략 백테스팅

```js
fee_percent = 0.006  # 0.6%

initial_investment = 1000.0
cash = initial_investment
position = 0  # bitcoin balance

for i in range(1, len(df)):
    if df.iloc[i]["signal"] == 1 and df.iloc[i - 1]["signal"] == -1:
        # buy, assuming all cash is used
        buy_amount = cash * (1 - fee_percent)  # deduct fee
        position = buy_amount / df.iloc[i]["close"]
        cash = 0

    elif df.iloc[i]["signal"] == -1 and df.iloc[i - 1]["signal"] == 1:
        # sell, converting position to cash
        cash = position * df.iloc[i]["close"] * (1 - fee_percent)  # deduct fee
        position = 0

final_value = cash if position == 0 else position * df.iloc[-1]["close"]
net_result = final_value - initial_investment

print(f"최종 가치: {final_value:.2f}")
print(f"순수익: {net_result:.2f} ({net_result / initial_investment * 100:.2f}%)")
```



![Scalping Strategy for Cryptocurrency](/assets/img/2024-05-05-ScalpingStrategyforCryptocurrency_3.png)

요 정보는 120분 동안 데이터를 사용할 때 전략이 손해를 낼 것을 보여줍니다. 시작 투자금인 £1000은 120분 후에 £914.38로 감소할 것입니다. 순 결과로는 -£85.62 (-8.56%)가 됩니다. 이것은 크게 놀랍지 않습니다. Coinbase의 수수료가 매우 높기 때문에 전략에 수수료가 포함되어 있습니다.

수수료를 제외하면 긍정적인 결과가 나옵니다.

```js
# fee_percent = 0.006  # 0.6%
fee_percent = 0.0  # 0.0%
```



![Scalping Strategy for Cryptocurrency](/assets/img/2024-05-05-ScalpingStrategyforCryptocurrency_4.png)

0.08% in 2 hours.

## Trailing Stop Loss and Take-Profit

A potential way to improve on this while maintaining the inclusion of fees is to implement a trailing stop loss and/or a take-profit mechanism. The take-profit is often 3x the stop loss.



저는 이것을 실험해 보았고 긍정적인 결과를 얻었지만 수수료를 0.6%에서 0.1%로 낮춰야 했습니다. 비트코인과 같이 변동성이 큰 자산에서 이보다 높은 수수료로는 1분 간격으로는 일관성이 없어 보입니다.

또한 암호화폐와 함께 작업할 때 더 반응성이 좋은 EMA(Exponential Moving Average)로 SMA(Simple Moving Average) 교차선을 교체했습니다.

내 코드는 이렇게 생겼어요:

```js
fast_ma_period = 12
slow_ma_period = 26
# df["fast_ma"] = df["close"].rolling(window=fast_ma_period).mean()
# df["slow_ma"] = df["close"].rolling(window=slow_ma_period).mean()
df["fast_ma"] = df["close"].ewm(span=fast_ma_period, adjust=False).mean()
df["slow_ma"] = df["close"].ewm(span=slow_ma_period, adjust=False).mean()

df["signal"] = 0  # 기본값 설정
df.loc[df["fast_ma"] > df["slow_ma"], "signal"] = 1  # 매수 신호
df.loc[df["fast_ma"] < df["slow_ma"], "signal"] = -1  # 매도 신호

fee_percent = 0.001  # 0.1%

initial_investment = 1000.0
cash = initial_investment
position = 0
high_since_buy = 0
buy_price = 0

trailing_stop_loss = 0.002  # 예: 0.002 × 100 = 0.2%
take_profit = trailing_stop_loss * 3  # 3배 트레일링 스탑 손실
stop_loss = 0.02  # 예: 0.02 × 100 = 2%

for i in range(1, len(df)):
    current_price = df.iloc[i]["close"]
    if position > 0:
        high_since_buy = max(high_since_buy, current_price)
        if current_price <= high_since_buy * (1 - trailing_stop_loss):
            cash = position * current_price * (1 - fee_percent)
            position = 0
            high_since_buy = 0
            print(f"{current_price}에서 트레일링 스탑 손실 활성화, 현금 {cash}으로 변경")
        elif current_price >= buy_price * (1 + take_profit):
            cash = position * current_price * (1 - fee_percent)
            position = 0
            high_since_buy = 0
            print(f"{current_price}에서 이익 실현, 현금 {cash}으로 변경")
        elif current_price <= buy_price * (1 - stop_loss):
            cash = position * current_price * (1 - fee_percent)
            position = 0
            high_since_buy = 0
            print(f"{current_price}에서 손절, 현금 {cash}으로 변경")

    if df.iloc[i]["signal"] == 1 and df.iloc[i - 1]["signal"] != 1:
        if cash > 0:
            buy_amount = cash * (1 - fee_percent)
            position = buy_amount / current_price
            cash = 0
            high_since_buy = current_price
            buy_price = current_price
            print(f"{current_price}에 매수, 포지션 {position}으로 변경")
    elif df.iloc[i]["signal"] == -1 and df.iloc[i - 1]["signal"] != -1:
        if position > 0:
            cash = position * current_price * (1 - fee_percent)
            position = 0
            high_since_buy = 0
            print(f"{current_price}에 매도, 현금 {cash}으로 변경")

final_value = cash if position == 0 else position * df.iloc[-1]["close"]
net_result = final_value - initial_investment

print(f"최종 가치: {final_value:.2f}")
print(f"순 결과: {net_result:.2f} ({net_result / initial_investment * 100:.2f}%)")
```



이 결과가 나왔어요...

```js
64034.67에 매수, 현재 포지션은 0.015600923687121368
63988.53에 매도, 현재 현금은 997.2818932076951
64011.68에 매수, 현재 포지션은 0.015564106602333939
64431.99에서 익절, 현재 현금은 1001.8235345995538
64399.0에 매수, 현재 포지션은 0.015540951118261995
64606.62에서 트레일링 스탑 로스, 현재 현금은 1003.0442750127916
최종 가치: 1003.04
순 결과: 3.04 (0.30%)
```

## 자동화: 봇 또는 웹소켓

이상적으로 효과적으로 사용하려면 이 작업을 자동화하는 것이 좋습니다. 짧은 시간 내에 많은 분석과 업데이트를 다뤄야 하기 때문에 수동으로는 실수할 가능성이 매우 높습니다.



웹소켓을 통해 이 작업을 수행하는 방법을 알아보았어요. EODHD API는 암호화폐 웹소켓 API를 제공하며, 여기에서 해당 문서를 찾을 수 있어요.

시작할 수 있는 몇 가지 코드를 제공해보려고 해요…

```javascript
import datetime
import websocket
import json
import time
import threading
import signal
import config as cfg

data = {}
last_printed_minute = None

def on_message(ws, message):
    global last_printed_minute
    message_json = json.loads(message)

    if "t" in message_json:
        date_time = datetime.datetime.utcfromtimestamp(message_json["t"] / 1000.0)
        floored_datetime = date_time.replace(second=0, microsecond=0)
        iso_format = floored_datetime.isoformat()

        if iso_format not in data:
            data[iso_format] = {"sum": float(message_json["p"]), "count": 1}
        else:
            data[iso_format]["sum"] += float(message_json["p"])
            data[iso_format]["count"] += 1

        if last_printed_minute != iso_format:
            if last_printed_minute is not None:
                avg_price = (
                    data[last_printed_minute]["sum"]
                    / data[last_printed_minute]["count"]
                )
                print(f"{last_printed_minute}: Average price: {avg_price}")
            last_printed_minute = iso_format

def on_error(ws, error):
    print("Error: " + str(error))

def on_close(ws, close_status_code, close_msg):
    print("WebSocket closed")

def on_open(ws):
    def run(*args):
        payload = {
            "action": "subscribe",
            "symbols": "BTC-USD",
        }
        ws.send(json.dumps(payload))

        # keep alive
        while True:
            time.sleep(50)
            ws.send(json.dumps({"action": "keep_alive"}))

    thread = threading.Thread(target=run)
    thread.start()

def on_signal(signal, frame):
    print("Signal received, closing WebSocket...")
    ws.close()

if __name__ == "__main__":
    websocket.enableTrace(False)
    ws = websocket.WebSocketApp(
        f"wss://ws.eodhistoricaldata.com/ws/crypto?api_token={cfg.API_KEY}",
        on_open=on_open,
        on_message=on_message,
        on_error=on_error,
        on_close=on_close,
    )

    signal.signal(signal.SIGINT, on_signal)  # handle Ctrl-C
    signal.signal(signal.SIGTERM, on_signal)  # handle kill commands

    ws.run_forever()
```

이 코드는 웹소켓에 연결하고 BTC-USD 메시지를 구독할 거에요. 그런 다음 한 분 내 모든 데이터를 집계하고 해당 분의 평균 가격을 계산하며, ISO 형식의 분 타임스탬프에 연결할 거에요. 웹소켓 오픈을 유지하는 케이프 앨리브도 있으며, 스크립트가 종료되면 웹소켓을 정상적으로 중지할 거에요.



다음과 같이 출력이 나타납니다…

```js
2024-04-14T20:26:00: 평균 가격: 64106.09294307696
2024-04-14T20:27:00: 평균 가격: 64108.821365536765
2024-04-14T20:28:00: 평균 가격: 64137.0758961011
2024-04-14T20:29:00: 평균 가격: 64134.83291021914
2024-04-14T20:30:00: 평균 가격: 64162.79143044936
2024-04-14T20:31:00: 평균 가격: 64212.15334335508
```

제 스캘핑 코드에서는 EMA12 및 EMA26을 사용했습니다. 이는 EMA26을 계산하기 위해 웹소켓을 26분간 실행해야 하며, 이것이 매수/매도 신호를 계산하는 데 필요합니다.

여기서부터는 동일한 과정입니다. 웹소켓이 다음 분을 반환할 때마다 이동 평균을 다시 계산하고 결과에 대해 어떻게 처리할지 결정하면 됩니다.




## 결론

스캘핑 전략은 잠재력이 있습니다. 반드시 봇이나 웹소켓으로 프로세스를 자동화해야 합니다. 이 작업을 수동으로 하면 머리가 순식간에 회색이 될 것입니다. 또한 매우 낮은 수수료를 제공하거나 다른 요금 구조를 가진 거래소를 찾아야 합니다. Binance는 이 측면에서 꽤 좋은데, 그들의 수수료가 훨씬 낮습니다.

이 기사가 흥미로웠고 유용했기를 바랍니다. 계속해서 최신 정보를 받아보고 싶다면, 저를 팔로우하고 이메일 알림에 가입해주시기 바랍니다.

- Michael Whittle



- 만약 이 글이 마음에 드셨다면, 제 Medium 페이지를 팔로우해주세요.
- 더 많은 흥미로운 기사를 보고 싶다면, 제 출판물을 팔로우해주세요.
- 협업에 관심이 있으시다면, LinkedIn에서 연결해보세요.
- 제 글을 비롯한 Medium 작가들을 지원하고 싶다면, 여기에서 가입해주세요.
- 이 글에 박수를 보내주세요 :) ← 감사합니다!