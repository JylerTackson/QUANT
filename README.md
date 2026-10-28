# QUANT

A NASDAQ TotalView-ITCH 5.0 feed handler written in C++20.

- `FeedHandler/` - CMake project (header-only `itch` library plus GoogleTest tests)
- `ITCH_READER.cpp` - ITCH file reader (work in progress)

## Market data (not included in this repo)

The sample data is a full day of NASDAQ ITCH 5.0 messages (12/30/2019):

| File | Size |
|------|------|
| `12302019.NASDAQ_ITCH50.gz` | ~3.5 GB |
| `12302019.NASDAQ_ITCH50` (decompressed) | ~8.3 GB |

These files are excluded via `.gitignore` and must never be committed. Make sure you have ~12 GB of free disk space.

### Download

Place the data in a `data/` directory at the repo root:

```sh
mkdir -p data && cd data
curl -O "https://emi.nasdaq.com/ITCH/Nasdaq%20ITCH/12302019.NASDAQ_ITCH50.gz"
gunzip -k 12302019.NASDAQ_ITCH50.gz   # -k keeps the .gz; omit to save disk space
```

Other dates are available from the same directory listing:
<https://emi.nasdaq.com/ITCH/Nasdaq%20ITCH/>. If the link has moved, search for
"NASDAQ TotalView-ITCH sample data". The protocol spec is the *Nasdaq TotalView-ITCH 5.0* specification on nasdaqtrader.com.

## Build and test

```sh
cd FeedHandler
cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
cmake --build build
ctest --test-dir build
```
