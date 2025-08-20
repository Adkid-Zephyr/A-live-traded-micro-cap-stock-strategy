# A-live-traded-micro-cap-stock-strategy
Base on an powerful active factor among CHINESE TOP 2000 Universe.

The code presented here contains only the core of the small-cap factor, with a very simple logic that is, however, highly effective in practice. 

The Micro-cap factor is a long-term effective factor in A-shares and often brings excess returns. In live-traded strategies, it can be combined with other strategies. The stock selection, risk control, and trading modules are still in need of improvement. For the convenience of backtesting, the code uses the JoinQuant platform API. Please register an account on your own, copy and paste the code for backtesting. The strategy data can be found in the Excel file. As for building the backtesting framework, the author is still in the process of learning and developing it.





# Strategy Logic Explanation

## 1. Strategy Objective
The strategy aims to regularly rebalance by purchasing small-cap stocks (with a market value between 200 million and 3 billion) that are not ST stocks and holding a certain number of stocks (set by **Stock Number Limit**, which is 3 in this case).

## 2. Rebalancing Frequency
The strategy rebalances every 5 trading days (set by **Rebalancing Frequency**).

## 3. Stock Selection Logic
- **Market Value Screening**: Select stocks from the CSI 300 index components with a market value between 200 million and 3 billion.
- **Filtering Suspended and ST Stocks**: Remove suspended stocks and ST stocks from the selected list.
- **Quantity Limit**: The final number of selected stocks does not exceed **Stock Number Limit** (i.e., 3 stocks).

## 4. Trading Logic
- **Selling Logic**: During each rebalancing, clear all current positions (i.e., sell all held stocks, targeting a position value of 0).
- **Buying Logic**: If the current number of held stocks is less than **Stock Number Limit** (i.e., 3), allocate the remaining cash evenly among the selected stocks for purchase. During each rebalancing, a maximum of **Stock Number Limit** stocks are purchased.

## 5. Funds Allocation
If there is remaining cash during rebalancing and the number of stocks to be purchased is **Number of Stocks to Buy**, then the cash allocated to each stock is **Cash per Stock** = Remaining Cash / Number of Stocks to Buy.

## 6. Timer
A **Trading Day Timer** is used to count trading days, incrementing by 1 each trading day, and resetting to 1 when the rebalancing frequency is reached.

## 7. Helper Functions
- **Filter Suspended Stocks**: Use the **Filter Suspended Stocks Function** to remove suspended stocks by obtaining current data.
- **Filter ST Stocks**: Use the **Filter ST Stocks Function** to remove ST stocks by obtaining current data.

## 8. Execution Mechanism
The strategy is set to run the **Trading Function** every trading day using the **Daily Execution Function**.

## 9. Transaction Cost Settings
Stock transaction costs are set to a commission of 0.03% for buying, and 0.03% commission plus 0.1% stamp duty for selling, with a minimum commission of 5 yuan per transaction.

## 10. Rebalancing Process Summary
1. **Check if Rebalancing is Needed**: Determine whether to rebalance based on **Trading Day Timer** and **Rebalancing Frequency**.
2. **Clear Positions**: If rebalancing is required, sell all held stocks.
3. **Select Stocks**: Choose target stocks based on market value range, filtering suspended and ST stocks.
4. **Purchase Stocks**: Allocate remaining cash evenly among the selected stocks and buy the stocks.
5. **Update Timer**: If rebalancing is completed, reset **Trading Day Timer** to 1; otherwise, increment **Trading Day Timer** by 1.

## 11. Strategy Features
- **Preference for Small-Cap Stocks**: The strategy focuses on small-cap stocks, which may have higher growth potential but also come with higher risks.
- **Regular Rebalancing**: By regularly rebalancing, the strategy attempts to capture short-term fluctuations in small-cap stocks.
- **Risk Control**: Potential risks are reduced by filtering out suspended and ST stocks.

## 12. Potential Risks
- **Small-Cap Stock Risk**: Small-cap stocks typically have lower liquidity and are more susceptible to market sentiment.
- **Rebalancing Frequency Risk**: Frequent rebalancing may increase transaction costs, affecting the overall strategy performance.
- **Market Volatility Risk**: During significant market fluctuations, the strategy may face considerable drawdown risks.

## 13. Optimization Directions
- **Add Selection Criteria**: Additional selection criteria, such as financial indicators and technical indicators, can be added to further optimize the stock selection logic.
- **Adjust Rebalancing Frequency**: Adjust the rebalancing frequency based on market conditions to reduce transaction costs.
- **Risk Management**: Introduce stop-loss mechanisms to control the maximum loss per stock.

---

### Explanation of Replaced Variables and Functions in the Code
- `g.stocknum` → **Stock Number Limit**
- `g.refresh_rate` → **Rebalancing Frequency**
- `g.days` → **Trading Day Timer**
- `context.portfolio.positions` → **Current Positions**
- `context.portfolio.cash` → **Remaining Cash**
- `order_target_value(stock, 0)` → **Sell Stock**
- `order_value(stock, Cash)` → **Buy Stock**
- `get_fundamentals(q)` → **Get Fundamental Data**
- `filter_paused_stock(buylist)` → **Filter Suspended Stocks Function**
- `filter_st_stock(buylist)` → **Filter ST Stocks Function**
- `run_daily(trade, 'every_bar')` → **Daily Execution Function**
- `trade` → **Trading Function**


# Translation in CHINESE

# 策略逻辑说明

## 1. **策略目标**
该策略的目标是通过定期调仓，买入市值在20亿到30亿之间且非ST的股票，并持有一定数量的股票（由**股票数量限制**设定，这里为3只）。

## 2. **调仓频率**
策略的调仓频率为每5个交易日调仓一次（由**调仓频率**设定）。

## 3. **选股逻辑**
- **市值筛选**：选股时，从沪深300成分股中筛选市值在20亿到30亿之间的股票。
- **过滤停牌和ST股**：从筛选出的股票中剔除停牌股票和ST股票。
- **数量限制**：最终选出的股票数量不超过**股票数量限制**（即3只）。

## 4. **交易逻辑**
- **卖出逻辑**：
  - 每次调仓时，清空所有持仓股票（即卖出所有持仓股票，目标持仓价值为0）。
- **买入逻辑**：
  - 如果当前持仓股票数量小于**股票数量限制**（即3只），则将剩余资金平均分配到选出的股票中进行买入。
  - 每次调仓时，最多买入**股票数量限制**只股票。

## 5. **资金分配**
如果调仓时有剩余资金，且需要买入的股票数量为**需买入股票数量**，则每只股票分配的资金为**每只股票分配资金 = 剩余资金 / 需买入股票数量**。

## 6. **计时器**
使用**交易日计时器**作为交易日计时器，每过一个交易日加1，达到调仓频率时重置为1。

## 7. **辅助函数**
- **过滤停牌股票**：使用**过滤停牌股票函数**，通过获取当前数据，剔除停牌的股票。
- **过滤ST股**：使用**过滤ST股函数**，通过获取当前数据，剔除ST股票。

## 8. **运行机制**
策略通过**每日运行函数**设置为每个交易日运行**交易函数**。

## 9. **手续费设置**
股票交易手续费设置为买入时佣金万分之三，卖出时佣金万分之三加千分之一印花税，每笔交易佣金最低扣5元。

## 10. **调仓流程总结**
1. **检查是否需要调仓**：根据**交易日计时器**和**调仓频率**判断是否达到调仓频率。
2. **清空持仓**：如果需要调仓，卖出所有持仓股票。
3. **选股**：根据市值范围、过滤停牌和ST股，选出目标股票。
4. **买入股票**：将剩余资金平均分配到选出的股票中，买入股票。
5. **更新计时器**：如果调仓完成，重置**交易日计时器**为1；否则，**交易日计时器**加1。

## 11. **策略特点**
- **小市值偏好**：策略专注于小市值股票，这类股票可能具有较高的成长潜力，但同时也伴随着较高的风险。
- **定期调仓**：通过定期调仓，策略试图捕捉小市值股票的短期波动机会。
- **风险控制**：通过过滤停牌和ST股，降低潜在风险。

## 12. **潜在风险**
- **小市值股票风险**：小市值股票通常流动性较差，且容易受到市场情绪的影响。
- **调仓频率风险**：频繁调仓可能导致交易成本增加，影响策略的整体收益。
- **市场波动风险**：在市场大幅波动时，策略可能面临较大的回撤风险。

## 13. **优化方向**
- **增加筛选条件**：可以增加其他筛选条件，如财务指标、技术指标等，以进一步优化选股逻辑。
- **调整调仓频率**：根据市场情况，调整调仓频率，以降低交易成本。
- **风险管理**：引入止损机制，控制单只股票的最大亏损比例。

---

### 代码中的变量和函数替换说明
- `g.stocknum` → **股票数量限制**
- `g.refresh_rate` → **调仓频率**
- `g.days` → **交易日计时器**
- `context.portfolio.positions` → **当前持仓**
- `context.portfolio.cash` → **剩余资金**
- `order_target_value(stock, 0)` → **卖出股票**
- `order_value(stock, Cash)` → **买入股票**
- `get_fundamentals(q)` → **获取基本面数据**
- `filter_paused_stock(buylist)` → **过滤停牌股票函数**
- `filter_st_stock(buylist)` → **过滤ST股函数**
- `run_daily(trade, 'every_bar')` → **每日运行函数**
- `trade` → **交易函数**
