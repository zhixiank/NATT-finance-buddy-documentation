# Technical Indicators and Matrices : Common Ways to Analyze Financial Data

There are various techniques used to analyze financial data, and the choice of method often depends on the type of data available and the objectives of the analysis. I reserach and implemented some of the most common methods for analyzing financial data and their usages:

## Types of Financial Analysis

### 1. Simple Moving Average (SMA)
   - Reference: [Investopedia - Simple Moving Average](https://www.investopedia.com/terms/s/sma.asp)
   
   The Simple Moving Average (SMA) is one of the most widely used indicators for identifying trends in stock prices. SMA is calculated by taking the average of a set number of past data points, usually the closing prices, over a specified time period.
   
   Formula:  
   `SMA = (Sum of Prices over N periods) / N`
   
   Example:  
   For three days of stock prices:
   - Day 1: $100
   - Day 2: $105
   - Day 3: $110
   
   The 3-day SMA would be:
   - SMA = (100 + 105 + 110) / 3 = 331 / 3 = 105
   
   Usage:  
   SMA is particularly useful for identifying the direction of a trend over a short, medium, or long term. However, in volatile markets, SMA may not be as accurate because it doesn't account for sudden market shifts.

   Time Periods:  
   - Short-term: Typically 5 to 20 days  
   - Medium-term: Typically 20 to 50 days  
   - Long-term: Typically 100 to 200 days
   
   Limitations:  
   SMA may not be as useful in highly volatile markets as it tends to smooth out rapid price changes.

   ```python 
   @staticmethod
    def get_sma(stock_ticker_symbol: str, interval: str = "1d", time_periods: List[int] = [8]) -> Optional[Dict[int, Any]]:
        """
        Given a stock's ticker symbol, calculates the SMA for the given time periods using yfinance.
    
        Returns a dictionary with the SMA values for each time period, or None if an error occurs.
        """
        try:
            # Fetch historical data using yfinance
            stock = yf.Ticker(stock_ticker_symbol)
            stock_data = stock.history(period="1y", interval=interval)  # Last year of data

            if stock_data.empty:
                return None

            results = {}
            for time_period in time_periods:
                # Calculate SMA
                sma = stock_data['Close'].rolling(window=time_period).mean()

                # Get the latest SMA value
                latest_sma = sma.iloc[-1]
                results[time_period] = round(latest_sma, 4)

            return results

        except Exception as e:
            print(f"Error fetching or calculating SMA: {e}")
            return None
    ```


### 2. Relative Strength Index (RSI)
   - Reference: [Investopedia - Relative Strength Index](https://www.investopedia.com/terms/r/rsi.asp)
   
   The Relative Strength Index (RSI) is a momentum oscillator that measures the speed and change of price movements. It helps inverstors and traders assess whether an asset is overbought or oversold.
   
   Formula:  
   RSI = 100 – ( 100 / (1 + RS) )  
   where RS is the Relative Strength, calculated as:
   
   `RS = Average Gain / Average Loss`
   
   Calculation Steps:
   - Identify gains and losses for each day.
   - Calculate the average gain and average loss over a specific period (usually 14 days).
   - Calculate Relative Strength (RS) and then the RSI.
   
   Interpretation:  
   - RSI above 70 suggests the asset might be overbought, indicating a potential price decrease.
   - RSI below 30 suggests the asset might be oversold, indicating a potential price increase.
   
   Data Needed:  
   To calculate RSI, you need the closing prices of the asset over a specific period.

   Usage:  
   RSI is widely used to identify overbought or oversold conditions in stocks or other assets, helping investors decide when to enter or exit positions.

   ```python
   @staticmethod
    def get_rsi(stock_ticker_symbol: str, interval: str = "1d", time_periods: List[int] = [14]) -> Optional[Dict[int, Any]]:
        """
        Given a stock's ticker symbol, calculates the RSI for the given time periods using yfinance.
    
        Returns a dictionary with the RSI values for each time period, or None if an error occurs.
        """
        try:
            # Fetch historical data using yfinance
            stock = yf.Ticker(stock_ticker_symbol)
            stock_data = stock.history(period="1y", interval=interval)  # Last year of data

            if stock_data.empty:
                return None

            # Calculate daily price change
            delta = stock_data['Close'].diff()

            results = {}
            for time_period in time_periods:
                # Calculate gain and loss
                gain = (delta.where(delta > 0, 0)).rolling(window=time_period).mean()
                loss = (-delta.where(delta < 0, 0)).rolling(window=time_period).mean()

                # Calculate RS and RSI
                rs = gain / loss
                rsi = 100 - (100 / (1 + rs))

                # Get the latest RSI value
                latest_rsi = rsi.iloc[-1]
                results[time_period] = round(latest_rsi, 4)

            return results

        except Exception as e:
            print(f"Error fetching or calculating RSI: {e}")
            return None
    ```


### 3.Candlestick Pattern Analysis
- Reference: [Investopedia - Candlestick Patterns](https://www.investopedia.com/articles/active-trading/092315/5-most-powerful-candlestick-patterns.asp)
   
   Candlestick Patterns are visual representations of price movements in the stock market. They help inverstors and traders predict potential future price movements based on historical data. A candlestick pattern typically includes:
   - Open price 
   - Close price 
   - High price
   - Low price
   
   Popular candlestick patterns include:
   - Hammer: A pattern where the price goes down during the day but recovers, forming a small body at the top and a long lower wick.
   - Shooting Star: The opposite of the hammer, where the price rises during the day but closes lower, with a small body at the bottom and a long upper wick.
   
   Usage:  
   Candlestick patterns are commonly used by traders to predict short-term price movements and market sentiment. While the data retrieved with yfinance libary is sufficient for this type of analysis.




