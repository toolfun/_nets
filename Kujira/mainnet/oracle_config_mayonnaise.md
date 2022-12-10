#### mayonnaise | Kohola.io oracle config currency pairs

```
[[currency_pairs]]
base = "USDT"
providers = [
  "binance",
  "kraken",
  "coinbase",
]
quote = "USD"

[[currency_pairs]]
base = "BNB"
providers = [
  "binance",
]
quote = "USD"

[[currency_pairs]]
base = "BNB"
providers = [
  "huobi",
  "mexc",
]
quote = "USDT"

[[currency_pairs]]
base = "ATOM"
providers = [
  "binance",
  "kraken",
  "coinbase",
  "osmosis",
]
quote = "USD"

[[currency_pairs]]
base = "BTC"
providers = [
  "huobi",
]
quote = "USDT"

[[currency_pairs]]
base = "BTC"
providers = [
  "binance",
  "coinbase",
]
quote = "USD"
[[currency_pairs]]
base = "ETH"
providers = [
  "binance",
  "kraken",
  "coinbase",
]
quote = "USD"

[[currency_pairs]]
base = "DOT"
providers = [
  "binance",
  "kraken",
  "coinbase",
]
quote = "USD"

[[provider_endpoints]]
name = "binance"
rest = "https://api.binance.us"
websocket = "stream.binance.us:9443"
```
