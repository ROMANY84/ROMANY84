- import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import yfinance as yf

# تحميل البيانات التاريخية
ticker = 'AAPL'
data = yf.download(ticker, start='2020-01-01', end='2023-01-01')

# حساب المتوسط المتحرك البسيط SMA 14
data['SMA_14'] = data['Close'].rolling(window=14).mean()

# حساب مؤشر القوة النسبية RSI 14
delta = data['Close'].diff()
gain = (delta.where(delta > 0, 0)).rolling(window=14).mean()
loss = (-delta.where(delta < 0, 0)).rolling(window=14).mean()
rs = gain / loss
data['RSI_14'] = 100 - (100 / (1 + rs))

# قواعد الاستراتيجية
data['Signal'] = 0
data.loc[(data['Close'] > data['SMA_14']) & (data['RSI_14'] < 30), 'Signal'] = 1
data.loc[(data['Close'] < data['SMA_14']) & (data['RSI_14'] > 70), 'Signal'] = -1

# حساب العوائد
data['Daily_Return'] = data['Close'].pct_change()
data['Strategy_Return'] = data['Signal'].shift(1) * data['Daily_Return']

# حساب العائد الكلي
cumulative_returns = (data['Strategy_Return'] + 1).cumprod()

# رسم النتائج
plt.figure(figsize=(14, 7))
plt.plot(data['Close'], label='Close Price')
plt.plot(data['SMA_14'], label='SMA 14')
plt.legend()
plt.show()

plt.figure(figsize=(14, 7))
plt.plot(cumulative_returns, label='Cumulative Strategy Returns')
plt.legend()
plt.show()

# عرض النتائج
print(f"Strategy Cumulative Return: {cumulative_returns[-1]}")👋 Hi, I’m @ROMANY84
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
ROMANY84/ROMANY84 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
