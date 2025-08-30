# 📈 Algo Trading Bot – SMA Strategy

A **Algorithmic Trading Bot** built in Python that uses a **Simple Moving Average (SMA) crossover strategy** to generate trading signals (Buy / Sell / Hold).  


## 🚀 Features
- ✅ Object-Oriented Python design (`MyTradingStrategy` base class + `MySmaTradingStrategy`)
- ✅ Configurable **short window** and **long window**
- ✅ Generates signals:  
  - 📊 **Buy** when short SMA > long SMA  
  - 📉 **Sell** when short SMA < long SMA  
  - ⏸ **Hold** otherwise
- ✅ Easy to extend for more advanced strategies

---

## 🛠️ How It Works
1. **Define Base Class** – `MyTradingStrategy` for generic trading strategy structure.
2. **Extend with SMA Strategy** – `MySmaTradingStrategy` for moving average crossovers.
3. **Feed Price Data** – Provide a list of prices as input.
4. **Generate Signal** – Returns `Buy`, `Sell`, or `Hold`.

---

## 🧑‍💻 Example Usage

```python
# Create strategy with short_window=3, long_window=5
strategy = MySmaTradingStrategy(3, 5)

# Run on sample price data
prices = [1,2,3,4,5,6,7,8,9,10]
signal = strategy.generate_signal(prices)

print("Trading Signal:", signal)
print("Short Window:", strategy.swindow)
print("Long Window:", strategy.lwindow)
print("Strategy Name:", strategy.name())
