# Crypto HFT System

Multi-exchange cryptocurrency high-frequency trading system.

## Features

- **10 Exchange Support**: Binance, Bybit, OKX, Kraken, Coinbase, KuCoin, Gate.io, Bitfinex, Deribit, HTX
- **3 Trading Modes**: Paper Trading, Live Trading, Backtesting
- **5 HFT Strategies**: Latency Arbitrage, Triangular Arbitrage, Statistical Arbitrage, Momentum Ignition, Order Flow Imbalance
- **5 Market Making Strategies**: Basic MM, Adaptive MM, Inventory MM, Grid MM, Avellaneda-Stoikov
- **Ultra-Low Latency**: Lock-free queues, memory pools, RDTSC timing

## Requirements

- C++20 compiler (GCC 11+, Clang 14+)
- CMake 3.20+
- Boost 1.75+
- OpenSSL 1.1+
- libcurl
- yaml-cpp

### Optional
- Google Test (for testing)
- Google Benchmark (for benchmarking)

## Quick Start

```bash
# Clone and build
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make -j$(nproc)

# Run paper trading
./crypto_hft --mode paper --config ../config/system.yaml

# Run backtesting
./crypto_hft --mode backtest --data ../data/

# Run live trading (requires API keys)
./crypto_hft --mode live --config ../config/system.yaml
```

## Configuration

All configuration is YAML-based in the `config/` directory:

- `exchanges.yaml` - Exchange API keys, endpoints, rate limits
- `strategies.yaml` - Strategy parameters and risk limits
- `risk.yaml` - Global risk management settings
- `system.yaml` - System-wide configuration

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Trading Engine                            │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │   Strategy  │  │   Strategy  │  │        Strategy         │  │
│  │ (HFT x 5)   │  │  (MM x 5)   │  │      Management         │  │
│  └──────┬──────┘  └──────┬──────┘  └────────────┬────────────┘  │
│         │                │                       │               │
│         └────────────────┼───────────────────────┘               │
│                          ▼                                       │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                 Order Management System                    │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐   │  │
│  │  │   Order     │  │  Position   │  │   Execution     │   │  │
│  │  │   Manager   │  │  Manager    │  │   Engine        │   │  │
│  │  └─────────────┘  └─────────────┘  └─────────────────┘   │  │
│  └───────────────────────────────────────────────────────────┘  │
│                          │                                       │
│  ┌───────────────────────┼───────────────────────────────────┐  │
│  │               Risk Management                              │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐   │  │
│  │  │   Risk      │  │   PnL       │  │   Circuit       │   │  │
│  │  │   Manager   │  │   Tracker   │  │   Breaker       │   │  │
│  │  └─────────────┘  └─────────────┘  └─────────────────┘   │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                          │
    ┌─────────────────────┼─────────────────────┐
    ▼                     ▼                     ▼
┌────────┐          ┌────────┐           ┌────────┐
│Binance │          │ Bybit  │    ...    │  HTX   │
└────────┘          └────────┘           └────────┘
```

## Strategies

### HFT Strategies

| Strategy | Description | Latency Target |
|----------|-------------|----------------|
| Latency Arbitrage | Exploits price differences across exchanges | <100μs |
| Triangular Arbitrage | Currency triangle inefficiencies | <200μs |
| Statistical Arbitrage | Pairs trading with cointegration | <500μs |
| Momentum Ignition | Order flow momentum detection | <100μs |
| Order Flow Imbalance | Microstructure-based signals | <100μs |

### Market Making Strategies

| Strategy | Description | Use Case |
|----------|-------------|----------|
| Basic MM | Simple spread-based quoting | Low volatility |
| Adaptive MM | Volatility-adjusted spreads | Variable markets |
| Inventory MM | Inventory-aware skewing | Risk management |
| Grid MM | Multi-level grid orders | Range-bound markets |
| Avellaneda-Stoikov | Optimal MM theory | Professional MM |

## Performance

Target latencies:
- Market data processing: <10μs
- Signal generation: <50μs
- Order routing: <100μs
- End-to-end: <500μs

## Testing

```bash
# Run unit tests
cd build
ctest --output-on-failure

# Run benchmarks
./benchmark_core
./benchmark_strategies
```

## License

