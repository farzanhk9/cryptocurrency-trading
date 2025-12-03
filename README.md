import ccxt
import time

# Set your API credentials (you can get these from your exchange account)
api_key = 'YOUR_API_KEY'
api_secret = 'YOUR_API_SECRET'

# Initialize the exchange (Binance in this example)
exchange = ccxt.binance({
    'apiKey': api_key,
    'secret': api_secret,
})

# Define trading pair and amount
symbol = 'BTC/USDT'  # Example: trading Bitcoin with USDT (Tether)
amount = 0.001  # Amount of BTC to buy/sell

# Function to get the current price of the asset
def get_current_price():
    ticker = exchange.fetch_ticker(symbol)
    return ticker['last']

# Function to place a market buy order
def place_buy_order():
    print(f"Placing buy order for {amount} {symbol}")
    order = exchange.create_market_buy_order(symbol, amount)
    print(f"Buy order placed: {order}")

# Function to place a market sell order
def place_sell_order():
    print(f"Placing sell order for {amount} {symbol}")
    order = exchange.create_market_sell_order(symbol, amount)
    print(f"Sell order placed: {order}")

# Example strategy: Buy when price is lower than $30,000, Sell when price is higher than $40,000
def trade_strategy():
    while True:
        try:
            current_price = get_current_price()
            print(f"Current price of {symbol}: ${current_price}")

            # Buy condition (price lower than $30,000)
            if current_price < 30000:
                place_buy_order()

            # Sell condition (price higher than $40,000)
            elif current_price > 40000:
                place_sell_order()

            time.sleep(60)  # Wait 1 minute before checking again
        except Exception as e:
            print(f"Error: {e}")
            time.sleep(60)  # Wait and retry if error occurs

# Start the trading strategy
trade_strategy()


