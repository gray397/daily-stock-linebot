stock_notify.py
import requests
import yfinance as yf
import datetime
import os

token = os.getenv("LINE_NOTIFY_TOKEN")

stocks = {
    "聯電": "2303.TW",
    "台積電": "2330.TW",
    "鴻海": "2317.TW"
}

def send_to_line(message):
    url = "https://notify-api.line.me/api/notify"
    headers = {"Authorization": f"Bearer {token}"}
    data = {"message": message}
    requests.post(url, headers=headers, data=data)

report = f"📊 台股每日報告 ({datetime.date.today()})\n"

for name, code in stocks.items():
    try:
        stock = yf.Ticker(code)
        df = stock.history(period="2d")
        today = df.iloc[-1]
        change = today["Close"] - today["Open"]
        pct = change / today["Open"] * 100
        report += f"\n【{name}】收：{today['Close']:.2f} ({'▲' if change >= 0 else '▼'}{abs(pct):.2f}%)\n"
        report += f"開：{today['Open']:.2f}｜高：{today['High']:.2f}｜低：{today['Low']:.2f}\n"
    except:
        report += f"\n❌ 無法取得 {name} 資料\n"

send_to_line(report)
