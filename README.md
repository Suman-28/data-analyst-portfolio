# stock_analysis_visuals.py

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Load the data (replace with your actual CSV file if using real data)
df = pd.read_csv('apple_stock_prices.csv', parse_dates=['date'])

# 2. Sort data by date
df.sort_values('date', inplace=True)

# 3. Add new columns for analysis
df['daily_change'] = df['close_price'] - df['open_price']
df['intraday_volatility'] = df['high_price'] - df['low_price']
df['rolling_avg_7d'] = df['close_price'].rolling(window=7).mean()
df['cumulative_return_pct'] = ((df['close_price'] / df['close_price'].iloc[0]) - 1) * 100

# 4. Set seaborn style
sns.set(style="whitegrid", palette="muted")

# 5. Visualization 1: Close Price Over Time
plt.figure(figsize=(12, 6))
plt.plot(df['date'], df['close_price'], label='Close Price', linewidth=2)
plt.plot(df['date'], df['rolling_avg_7d'], label='7-Day Rolling Avg', linestyle='--')
plt.title('Apple Stock: Close Price Trend')
plt.xlabel('Date')
plt.ylabel('Price (USD)')
plt.legend()
plt.tight_layout()
plt.show()

# 6. Visualization 2: Daily Price Change (Last 30 Days)
plt.figure(figsize=(12, 4))
sns.barplot(x='date', y='daily_change', data=df[-30:], palette='coolwarm')
plt.xticks(rotation=45)
plt.title('Daily Price Change (Last 30 Days)')
plt.tight_layout()
plt.show()

# 7. Visualization 3: Volume Trends
plt.figure(figsize=(12, 4))
sns.lineplot(data=df, x='date', y='volume', color='purple')
plt.title('Daily Trading Volume')
plt.tight_layout()
plt.show()

# 8. Visualization 4: Intraday Volatility
plt.figure(figsize=(12, 4))
sns.lineplot(x='date', y='intraday_volatility', data=df, color='orange')
plt.title('Intraday Price Volatility')
plt.tight_layout()
plt.show()

# 9. Visualization 5: Cumulative Return
plt.figure(figsize=(12, 4))
sns.lineplot(x='date', y='cumulative_return_pct', data=df, color='green')
plt.title('Cumulative Return % Over Time')
plt.ylabel('Return (%)')
plt.tight_layout()
plt.show()

# End of Script
print("Analysis Complete ✅")
