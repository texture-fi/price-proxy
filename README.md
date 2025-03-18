# Price Proxy

Solana on-chain program to collect price info from various preconfigured sources (SuperLendy, Pyth, Switchboard, custom off-chain oracle) and put it in to standard PriceProxy feed accounts. 

## CLI Examples

### Create off-chain price-feed

```
price-proxy create-price-feed \
    --symbol SOL \
    --quote-symbol USD \ # USD is default
    --source offchain \ # or `off-chain`, or `OffChain`, or `Offchain`, or `offChain`
    --source-address $(solana address)
```

Output:
```
{
  "created_price_feed": "DQfDFM8uPhYL4Abgj6dFUhtyFaGfFo7oB88iwesUbR3G",
  "signature": "2ahPNRUXmuaffo3kbRkkKYwue146oL2iTTXL5piPF2P2FaTWHExnrqoNKjEUXfBDCAk7RJs18PzGEgRfP3tVTxzt"
}
```

### Print price-feed

```
price-proxy price-feed GkJtXVs4cyXhY81w48YEmruDcPkhv1AXC51VFQ9rQoVE
```

Output
```
{
  "key": "AtV6ixH4K9LrnY1H1m8bmNwnBBVd93cr7oqGfgmHaFp1",
  "price_feed": {
    "discriminator": "PRICEEED",
    "version": 1,
    "source": "OffChain",
    "source_address": "AupcoSi6mQi2PjgSDTh3gBZ2FiKnhL55Xo1XYSg3meJM",
    "symbol": "SOL",
    "quote_symbol": "USD",
    "update_authority": "AupcoSi6mQi2PjgSDTh3gBZ2FiKnhL55Xo1XYSg3meJM",
    "update_timestamp": "1970-01-01T00:00:00Z",
    "update_slot": 0,
    "price": "0.000000000000000000"
  },
  "slot": 2
}
```

### Print multiple price-feeds

All
```
price-proxy price-feeds
```

Multiple by key
```
price-proxy price-feeds \
    --key Aqx2G4XpZruz4MWKtCaCRMVzKhUNWiRrPpEKzKJ2Ymy6 \
    --key E6tLQn4QjGqWUPAwsqjdH9e5fbbLW6LNXYq5kw7YpnUP \
    --key ...
```

Output
```
{
  "price_feeds": [
    [
      "Aqx2G4XpZruz4MWKtCaCRMVzKhUNWiRrPpEKzKJ2Ymy6",
      {
        "discriminator": "PRICEEED",
        "version": 1,
        "source": "OffChain",
        ...
      }
    ],
    [
      "E6tLQn4QjGqWUPAwsqjdH9e5fbbLW6LNXYq5kw7YpnUP",
      {
        "discriminator": "PRICEEED",
        "version": 1,
        "source": "OffChain",
        ...
      }
    ],
    ...
  ],
  "slot": 3
}
```

### Write price into off-chain price-feed

```
price-proxy write-price GkJtXVs4cyXhY81w48YEmruDcPkhv1AXC51VFQ9rQoVE 1.04003
```

Output
```
{
  "signature": "gLB51yuyepCiJHi4h3EA7hQz6wyuchr2NXpdi1FNFPbRHHKZsLZynpcgks7LXTZFimGSfZm7CmQEGDBz2GzrkWE"
}
```

### Force update timestamp (for development purposes only)

* Once
  ```
  price-proxy force-update-timestamp \
      --key 8ttGephEdQB57XuMA2YZnrehSosQRgYodGbG5nb1HVmE \
      --key 9dhYbsNdFwdA6XH7apFBJecAJvuBsRVxQZTHmspB65jK \
      --key ...
  ```

  Output
  ```
  [
    {
      "price_feed": "9dhYbsNdFwdA6XH7apFBJecAJvuBsRVxQZTHmspB65jK",
      "signature": "3mgmqaK83gnPUqqQecTLaVAvAGJx3G5UMUQXuZoosULhMjcuSxNzok518KeZW8pFpGxRRq8rPdkofbaRx1igK8fE"
    },
    {
      "price_feed": "8ttGephEdQB57XuMA2YZnrehSosQRgYodGbG5nb1HVmE",
      "error": "price feed source must be 'OffChain' (current 'Pyth')"
    }
  ]
  ```

* Auto
  ```
  price-proxy force-update-timestamp --period 1s \
      --key DQfDFM8uPhYL4Abgj6dFUhtyFaGfFo7oB88iwesUbR3G
      --key ...
  ```

  Logs only.
