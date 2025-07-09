# S-p500-Financial-analysis

# Purpose: Analyze and visualize S&P 500 historical data

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

# Load cleaned historical prices
df = pd.read_csv('../data/historical_prices.csv')
df['Date'] = pd.to_datetime(df['Date'])

# Preview
print(df.head())

# Pivot for adjusted close price time series
pivot_df = df.pivot_table(index='Date', columns='Ticker', values='Adj Close')

# Plot price trends
pivot_df.plot(figsize=(14, 7), title='Adjusted Close Prices of Top 10 S&P 500 Companies (2010–2025)')
plt.ylabel('Price ($)')
plt.xlabel('Date')
plt.grid(True)
plt.tight_layout()
plt.show()

# Calculate daily returns
returns_df = pivot_df.pct_change().dropna()

# Plot cumulative returns
cumulative_returns = (1 + returns_df).cumprod()
cumulative_returns.plot(figsize=(14, 7), title='Cumulative Returns of Top 10 Companies')
plt.ylabel('Growth ($)')
plt.xlabel('Date')
plt.grid(True)
plt.tight_layout()
plt.show()

# Correlation heatmap
plt.figure(figsize=(10, 8))
sns.heatmap(returns_df.corr(), annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Correlation of Daily Returns (Top 10 Companies)')
plt.tight_layout()
plt.show()

# Volatility over time
volatility = returns_df.rolling(window=30).std()
volatility.plot(figsize=(14, 6), title='30-Day Rolling Volatility')
plt.ylabel('Volatility')
plt.xlabel('Date')
plt.grid(True)
plt.tight_layout()
plt.show()

print("✅ Exploratory analysis complete.")
