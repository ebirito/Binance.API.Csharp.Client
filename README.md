# Binance API Csharp Client
[![NuGet](https://img.shields.io/badge/nuget-v1.0.1-blue.svg)](https://www.nuget.org/packages/Binance.API.Csharp.Client)
### C#.NET client for Binance Exchange API.

## Features
- Complete implementation of the Binance API.
- API results parsed to concrete objects for better ease of usage.
- Test project included with all posible API calls.

## Installation

**Nuget Package Manager**
```
Install-Package Binance.API.Csharp.Client
```
**.NET CLI**
```
dotnet add package Binance.API.Csharp.Client
```
## Getting Started
Create an instance of the **APIClient** which receive the following parameters:

* **ApiKey** - *Key used to authenticate within the API.*
* **ApiSecret** - *API secret used to signed some API calls.*
* **ApiUrl (Optional)** - *Base URL of the API.*
```c#
    var apiClient = new ApiClient("@Your-API-Key", "@Your-API-Secret");
```

Create an instance of the **BinanceClient** which will receive the previously created APIClient
 
```c#
    var binanceClient = new BinanceClient(apiClient);
```

## General Methods
### Test connectivity
Test connectivity to the Rest API.
#### Example
```c#
    var test = binanceClient.TestConnectivity().Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<dynamic> TestConnectivity()
```
</details>

### Check server time
Test connectivity to the Rest API and get the current server time.
#### Example
```c#
    var serverTime = binanceClient.GetServerTime().Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<ServerInfo> GetServerTime()
```
</details>

## Market Data Methods
### Get order book
Get order book for a particular symbol.
#### Example
```c#
    var orderBook = binanceClient.GetOrderBook("ethbtc").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<OrderBook> GetOrderBook(string symbol, int limit = 100)
```
</details>

### Get compressed/aggregate trades list
Get compressed, aggregate trades. Trades that fill at the time, from the same order, with the same price will have the quantity aggregated.
#### Example
```c#
    var aggregateTrades = binanceClient.GetAggregateTrades("ethbtc").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<IEnumerable<AggregateTrade>> GetAggregateTrades(string symbol, int limit = 500)
```
</details>

### Get kline/candlesticks
Kline/candlestick bars for a symbol. Klines are uniquely identified by their open time.
#### Example
```c#
    var candlestick = binanceClient.GetCandleSticks("ethbtc", TimeInterval.Minutes_15).Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<IEnumerable<Candlestick>> GetCandleSticks(string symbol, TimeInterval interval, int limit = 500)
```
</details>

### Get 24hr ticker price change statistics
24 hour price change statistics.
#### Example
```c#
    var priceChangeInfo = binanceClient.GetPriceChange24H("ethbtc").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<PriceChangeInfo> GetPriceChange24H(string symbol)
```
</details>

### Get symbols price ticker
Latest price for all symbols.
#### Example
```c#
    var tickerPrices = binanceClient.GetAllPrices().Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<IEnumerable<SymbolPrice>> GetAllPrices()
```
</details>

### Get symbols order book ticker
Best price/qty on the order book for all symbols.
#### Example
```c#
    var orderBookTickers = binanceClient.GetOrderBookTicker().Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<IEnumerable<OrderBookTicker>> GetOrderBookTicker()
```
</details>

## Account Methods
### Post new order
Send in a new order
####Examples
Post new buy order (LIMIT)
```c#
    var newOrder = binanceClient.PostNewOrder("ethbtc", 1m, 0.04m, OrderType.LIMIT, OrderSide.BUY).Result;
```
Post new sell order (LIMIT)
```c#
    var newOrder = binanceClient.PostNewOrder("ethbtc", 1m, 0.04m, OrderType.LIMIT, OrderSide.SELL).Result;
```
Post new buy order (MARKET)
```c#
    var newOrder = binanceClient.PostNewOrder("ethbtc", 0.1m, 0m, OrderType.MARKET, OrderSide.BUY).Result;
```
Post new sell order (MARKET)
```c#
    var newOrder = binanceClient.PostNewOrder("ethbtc", 0.1m, 0m, OrderType.MARKET, OrderSide.SELL).Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<NewOrder> PostNewOrder(string symbol, decimal quantity, decimal price, OrderType orderType, OrderSide side, TimeInForce timeInForce = TimeInForce.GTC, long recvWindow = 6000000)
```
</details>

### Post test order 
Test new order creation and signature/recvWindow long. Creates and validates a new order but does not send it into the matching engine.
#### Example
```c#
    var testOrder = binanceClient.PostNewOrderTest("ethbtc", 1m, 0.1m, OrderType.LIMIT, OrderSide.BUY).Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<dynamic> PostNewOrderTest(string symbol, decimal quantity, decimal price, OrderType orderType, OrderSide side, TimeInForce timeInForce = TimeInForce.GTC, long recvWindow = 6000000)
```
</details>

### Get order
Check an order's status.
#### Example
```c#
    var order = binanceClient.GetOrder("ethbtc", 8982811).Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<Order> GetOrder(string symbol, long? orderId = null, string origClientOrderId = null, long recvWindow = 6000000)
```
</details>

 ### Cancel order 
Cancel an active order.
#### Example
```c#
    var canceledOrder = binanceClient.CancelOrder("ethbtc", 9137796).Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<CanceledOrder> CancelOrder(string symbol, long? orderId = null, string origClientOrderId = null, long recvWindow = 6000000)
```
</details>

### Get current open orders
Get all open orders on a symbol.
#### Example
```c#
    var openOrders = binanceClient.GetCurrentOpenOrders("ethbtc").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<IEnumerable<Order>> GetCurrentOpenOrders(string symbol, long recvWindow = 6000000)
```
</details>

### Get all orders
Get all account orders; active, canceled, or filled.
#### Example
```c#
    var allOrders = binanceClient.GetAllOrders("ethbtc").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<IEnumerable<Order>> GetAllOrders(string symbol, long? orderId = null, int limit = 500, long recvWindow = 6000000)
```
</details>

### Get account info
Get current account information.
#### Example
```c#
    var accountInfo = binanceClient.GetAccountInfo().Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<AccountInfo> GetAccountInfo(long recvWindow = 6000000)
```
</details>

### Get account trade list
Get trades for a specific account and symbol.
#### Example
```c#
    var tradeList = binanceClient.GetTradeList("ethbtc").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<IEnumerable<Trade>> GetTradeList(string symbol, long recvWindow = 6000000)
```
</details>

## User Stream Methods
### Start user stream
Start a new user data stream.
#### Example
```c#
    var listenKey = binanceClient.StartUserStream().Result.ListenKey;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<UserStreamInfo> StartUserStream()
```
</details>

### Keep alive user data stream
PING a user data stream to prevent a time out.
#### Example
```c#
    var ping = binanceClient.KeepAliveUserStream("@ListenKey").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<dynamic> KeepAliveUserStream(string listenKey)
```
</details>

### Close user data stream
Close out a user data stream.
#### Example
```c#
    var resut = binanceClient.CloseUserStream("@ListenKey").Result;
```
<details>
 <summary>Method Signature</summary>

```c#
    public async Task<dynamic> CloseUserStream(string listenKey)
```
</details>


## License

Binance.API.Csharp.Client is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
