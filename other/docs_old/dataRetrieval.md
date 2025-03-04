# Financial Data Retrieval

In the initial stages of development, I explored various APIs for obtaining stock market data and technical analysis indicators such as the Simple Moving Average (SMA) and Relative Strength Index (RSI). Below is a summary of the APIs considered:

## 1. Polygon API

   Polygon offers real-time and historical market data for stocks, forex, and cryptocurrencies. It provides robust APIs that cover a wide range of market data, but I found it limited by the number of free calls allowed for users. This made it less suitable for continuous, large-scale requests within the application.

## 2. Finnhub API

   Finnhub provides stock market data, financial news, and sentiment analysis. It covers a broad spectrum of market trends and news, and is especially useful for staying up-to-date with the latest financial news. However, it too suffers from limited API call quotas for free users.

## 3. Alpha Vantage API

   Alpha Vantage offers stock prices, forex rates, and cryptocurrency data, with a strong focus on technical analysis and data visualization. I experimented with a GitHub repository that aimed to visualize stock data with ticker inputs, but the implementation was unsuccessful. Nevertheless, this API remains a good candidate for future integration, especially for users who require more technical indicators.

## 4. NewsAPI

   NewsAPI allows retrieval of the latest financial news from various sources. It’s particularly helpful for integrating real-time financial news into the application. However, the API does not provide in-depth market analysis or technical data, making it a supplementary tool for keeping users informed.

## Challenges with Free API Limits

   One of the main challenges encountered while using these APIs is the limitation on the number of API calls for free-tier users. This significantly slows down the progress of development and impacts the real-time updating of financial data in the application.

## Yahoo Finance (yfinance) vs Google Finance (googlefin)

### Coverage and Scope & Integration

- Yahoo Finance (yfinance): Provides stock prices, historical data, indices, P/E ratios, earnings, cryptocurrency, and forex data. It is available via the `yfinance` library, an open-source Python tool.
- Google Finance (googlefin): Offers stock prices, recent historical data, and a limited number of technical indicators. It is accessed through the `GOOGLEFINANCE` function in Google Sheets.

### API Stability and Rate Limits

- Yahoo Finance (yfinance): Though unofficial, `yfinance` is relatively stable. The rates are not officially monitored, but too many requests may lead to temporary blocks.
- Google Finance (googlefin): More stable when accessed via Google Sheets, but heavy usage may trigger rate limiting. Third-party APIs exist for Google Finance but offer limited access and support.

### Data Accuracy

- Yahoo Finance (yfinance): Provides real-time data and is well-suited for analytical purposes.
- Google Finance (googlefin): Offers basic data but lacks the depth and real-time capabilities that Yahoo Finance offers.

## Conclusion

Based on the comparison, Yahoo Finance (yfinance) is the most preferable option due to its comprehensive coverage of financial data and superior accuracy compared to Google Finance. After conducting thorough research, I decided to use the `yfinance` library for retrieving financial data in this project.

## References

- [yfinance Documentation](https://python-yahoofinance.readthedocs.io/en/latest/api.html#)
- [yfinance GitHub Repository](https://github.com/ranaroussi/yfinance)
