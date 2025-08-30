from datetime import datetime
import yfinance as yf
import time

class MyTradingStrategy:
    def __init__(self,name):
        self.__name=name

    def generate_signal(self,price_data):
        print("This method is intended to be over ridden")
        return "Hold"
        
    @property
    def name(self):
        return self.__name
        
class MySmaTradingStrategy(MyTradingStrategy):
    def __init__(self,swindow,lwindow):
        self.__swindow = swindow
        self.__lwindow  = lwindow
        super().__init__("My SMA Trading Strategy")
        
    def generate_signal(self,price_data):
        if len(price_data[-self.__lwindow:])<self.__lwindow:
            return "Hold"
        short_avg=sum(price_data[-self.__swindow:])/self.__swindow
        long_avg=sum(price_data[-self.__lwindow:])/self.__lwindow
        
        if short_avg>long_avg:
            return"Buy"
        elif short_avg< long_avg:
            return "sell"
        else:
            return "Hold"
        
        
    @property
    def swindow(self):
        return self.__swindow
        
    @property
    def lwindow(self):
        return self.__lwindow
        
class MyTrade:
    def __init__(self,strategy_name, signal, amount ):
        self.__strategy_name=strategy_name
        self.__signal=signal
        self.__amount=amount
        self.__timestamp= datetime.now()
        
    def execute(self):
      print(f"Executed {self.__signal} trade with the strategy {self.__strategy_name} with amount {self.__amount} at the time {self.__timestamp}")
        
    @property
    def strategy_name(self):
        return self.__strategy_name
        
    @property
    def signal(self):
        return self.__signal
        
    @property
    def amount(self):
        return self.__amount
        
    @property
    def timestamp(self):
        return self.__timestamp
        
#Mock Trading API

class MockTradingAPI:
    def __init__(self,balance):
        self.__balance=balance
    
    def place_order(self,trade,price):
        if trade.signal=="Buy" and self.__balance>=trade.amount * price:
            self.__balance -= trade.amount*price
            print(f"Placed a buy trade at the {price}, Remaining Balance :{self.__balance}")
        elif trade.signal == "Sell":    
            self._balance += trade.amount * price
            print("Placedd a buy trade at the {price},Remaining Balance :{self.__balance}")
        else:
            print(f"Insufficient balance or Invalid Signal")
            
        @property
        def balance(self):
            return self.__balance
            
class MyTradingSystem:
    def __init__(self,api,strategy,symbol):
        self.__api = api
        self.__strateg = strategy
        self.__symbol = symbol
        self.__price_data =[]
        
    def fetch_price_data(self):
         data = yf.download(tickers = self._symbol, period= '1d', interval = '1m')
         if not data.empty:
             price = data['Close'].iloc[-1]
             self.__price_data.apped(price)
             if len (self.__price_data) >  self.__strategy. lwindow:
                 self.__strategy.lwindow.pop(0)
                 print(f"Fetched new price data: {price}")
             else:
                 print("No data fetched")
            
    def run(self):
        self.fetch_price_data()
        signal = self.__strategy.generate_signal(self.__price_data)
        print(f"Generated Sinal: {signal}")
        if signal in ["Sell","Buy"]:
            trade = MyTrade(self.__strategy,signal,1)
            trade.execute()
            self.__api.place_order(trade,self.__price_data[-1]) 
       
    @property
    def api(self):
        return self.__api
    
    @property    
    def strategy(self):
        return self.strategy
        
    @property
    def symbol(self):
        return self._symbol
        
    @property
    def balance(self):
        return self.__price_data
        
symbol = 'AAPL'
api = MockTradingAPI(balance=10000)
strategy =MySmaTradingStrategy(3,5)
system=MyTradingSystem(api,strategy,symbol)

for _ in range(3):
    system.run()
    print(f"Remaining balance: {api.balance}")
    time.sleep(2)
        
#trade1= MySmaTradingStrategy(3,5)
#strategy_name = "SMA"  # This assigns a string, not a variable
#signal = trade1.generate_signal([1,2,3,4,5,4,3,2,11])
#trade2 = MyTrade(strategy_name, signal, 10000)
#ObjMockAPI = MockTradingAPI(10000)
#ObjMockAPI.place_order(trade2, 1000)

            

#MyBaseObj=MyTradingStrategy("Awesome Stratergy")
#print(MyBaseObj.generate_signal(12))
#print(MyBaseObj.name())

#trade=MyTradingStrategy("Awesome Strategy")
#trade.generate_signal(1,2,3,4,5)

#ObjStrategy = MySmaTradingStrategy(3, 5)

#signal = ObjStrategy.generate_signal([1,2,3,4,5,6,7,8,9,10])

#print("Signal:", signal)                
#print("Long Window:", ObjStrategy.lwindow)
#print("Short Window:", ObjStrategy.swindow)
#print("Strategy Name:", ObjStrategy.name)
#ObjStrategy = MySmaTradingStrategy(3, 5)
#strategy_name=ObjStrategy.name
#signal=ObjStrategy.generate_signal([1,2,3,4,5])
#ObjMyTrade=MyTrade(strategy_name,signal,10000)
#ObjMyTrade.execute()

#print(ObjMyTrade.signal)
#print(ObjMyTrade.strategy_name)
#print(ObjMyTrade.amount)
#print(ObjMyTrade.timestamp)
