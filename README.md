# Ledger Stats

You can use this tool to fetch all the account balance information or a specific XRP/XAG ledger number.

This tool will connect to a _rippled_-server using websockets. The default server it will connect to is `g1.xrpgen.com`. You can manually specify a different _rippled_-server. 

## Syntax

### Fetch ledger account data into .json file

Run the code to fetch all account balances using:

```
npm run fetch
```

If you don't append any arguments, the latest (closed) ledger will be fetched from the public `s1.ripple.com` server.

If you want to fetch a specific ledger, append the ledger index:

```
npm run fetch 38596307
```

If you want to query a specific _rippled_ node:
```
npm run fetch 32570 g2.xrpgen.com
```

(This tool will assume secure websockets (`wss://`) if you only enter a hostname. Prepend `ws://` to connect to an insecure host)

Instead of a ledger number (or omitting one) you can also enter one of the ledger info keywords:


```
npm run fetch "closed" g2.xrpgen.com
```

**The tool will save the balance information for the ledger in `./data/ledgerno.json`, eg. `./data/38596307.json`.

### JSON output format

The JSON files stored in the `./data/` folder will contain all the accounts (`balances[x].a`) found in the given ledger index with their balances (`balances[x].b`):

```
{
  "stats": {
    "hash":"4109C6F2045FC7EFF4CDE8F9905D19C28820D86304080FF886B299F0206E42B5",
    "ledger_index":32570,
    "close_time_human":"2013-Jan-01 03:21:10"
    ,"total_coins":99999999999.99632
  },
  "balances": [
    {"a":"rBKPS4oLSaV2KVVuHH8EpQqMGgGefGFQs7","b":370},
    {"a":"rLs1MzkFWCxTbuAHgjeTZK4fcCDDnf2KRv","b":10000},
    {"a":"rpGaCyHRYbgKhErgFih3RdjJqXDsYBouz3","b":10000000},
    {"a":"rUnFEsHjxqTswbivzL2DNHBb34rhAgZZZK","b":10000},
    [ ... more data ]
  ]
}
```


### Process ledger data from .json file (to stats)

Run:

```
npm run stats 32570
```

... where `32570` is the ledger index you want to process. You should -of course- have fetched this ledger first.

The stats will be displayed in the terminal and exported to `./data/ledgerno.stats.json` [in this format](https://ajx2m1t.dlvr.cloud/pasted_1.png):

```
{
  meta: { ... },
  top100Balance: 123.567,
  accountPercentageBalance: [ 
    {
      percentage: 1,
      numberAccounts: 2,
      balanceEqGt: 3
    },
    { ... }
  ],
  accountNumberBalanceRange: [
    {
      numberAccounts: 1,
      balanceFrom: 2,
      balanceTo: 3,
      balanceSum: 4
    },
    { ... }
  ]
}
```

## Batch processing

To download a ledger, calculate the stats, increment (backwards) and do the same all over again, you can use:

```
npm run batch {startledger} {rippledserver} {increment}
```

eg.

```
npm run batch 38630615 g2.xrpgen.com 100000
```

... to start at ledger `38630615` and then increment backin history by 100,000 ledgers.

The batch process will cleanup the downloaded ledger data (to save storage space). The `json` files containing the calculation results will be stored in `./data/`.
