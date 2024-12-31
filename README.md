import yfinance as yf

class StockPortfolio:
    def __init__(self):
        self.stocks = {}

    def add_stock(self, ticker, shares):
        if ticker in self.stocks:
            self.stocks[ticker] += shares
        else:
            self.stocks[ticker] = shares
        print(f"Added {shares} shares of {ticker}")

    def remove_stock(self, ticker, shares):
        if ticker in self.stocks and self.stocks[ticker] >= shares:
            self.stocks[ticker] -= shares
            if self.stocks[ticker] == 0:
                del self.stocks[ticker]
            print(f"Removed {shares} shares of {ticker}")
        else:
            print(f"Not enough shares of {ticker} to remove")

    def get_portfolio_value(self):
        total_value = 0.0
        for ticker, shares in self.stocks.items():
            stock = yf.Ticker(ticker)
            price = stock.history(period='1d')['Close'][0]
            total_value += price * shares
        return total_value

    def show_portfolio(self):
        for ticker, shares in self.stocks.items():
            stock = yf.Ticker(ticker)
            price = stock.history(period='1d')['Close'][0]
            print(f"{ticker}: {shares} shares @ {price:.2f} each")
        print(f"Total portfolio value: {self.get_portfolio_value():.2f}")

def main():
    portfolio = StockPortfolio()
    
    while True:
        print("\nStock Portfolio Tracker")
        print("1. Add Stock")
        print("2. Remove Stock")
        print("3. Show Portfolio")
        print("4. Exit")
        
        choice = input("Enter your choice: ")
        
        if choice == '1':
            ticker = input("Enter the ticker symbol: ").upper()
            shares = int(input("Enter the number of shares: "))
            portfolio.add_stock(ticker, shares)
        elif choice == '2':
            ticker = input("Enter the ticker symbol: ").upper()
            shares = int(input("Enter the number of shares to remove: "))
            portfolio.remove_stock(ticker, shares)
        elif choice == '3':
            portfolio.show_portfolio()
        elif choice == '4':
            print("Goodbye!")
            break
        else:
            print("Invalid choice, please try again.")

if __name__ == "__main__":
    main()
