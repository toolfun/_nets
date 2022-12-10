### Test config of the CURRENCY PAIRS, after added BNB.
BTC, ETH, DOT paired with USDT. More exchanges
#
#### Unsupported exchanges with my price-feeder
`bitget`
#### Provider does not support multiple pairs
`gate`
#
### Currency Pairs in config.toml
```bash
[[currency_pairs]]
base = "ATOM"
providers = [
  "binance",
  "kraken",
  "osmosis",
]
quote = "USD"

[[currency_pairs]]
base = "BTC"
providers = [
  "huobi",
  "okx",
  "osmosis",
]
quote = "USDT"

[[currency_pairs]]
base = "ETH"
providers = [
  "okx",
  "gate",
  "huobi"
]
quote = "USDT"

[[currency_pairs]]
base = "DOT"
providers = [
  "okx",
  "huobi",
  "coinbase"
]
quote = "USDT"

[[currency_pairs]]
base = "BNB"
providers = [
  "binance",
  "huobi",
  "mexc"
]
quote = "USDT"

[[currency_pairs]]
base = "USDT"
providers = [
  "coinbase",
  "kraken",
  "osmosis",
]
quote = "USD"
```
