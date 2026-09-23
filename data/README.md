# Data

The notebooks download public market data through [AKShare](https://akshare.akfamily.xyz/) and cache successful responses locally.

## Expected local layout

Depending on which notebook has been run, this directory may contain:

```text
data/
├── csi300_daily_cache.csv
└── hmm_sw_rotation/
    ├── csi300_daily.csv
    └── sw_first_level.csv
```

The exact cache filenames may change with the data interface used by AKShare. Cached market data are intentionally excluded from version control.

## Main data inputs

- CSI 300 daily price-index observations.
- Shenwan Level-1 industry daily index observations.
- Trading dates inferred from the available CSI 300 series.

The notebooks standardise dates and closing prices, remove invalid observations, sort the series chronologically, check duplicates and enforce minimum industry coverage.

## Reproducibility notes

Public data endpoints can be revised, temporarily unavailable or rate-limited. Therefore, a fresh run may not exactly reproduce the saved outputs if the provider has corrected historical observations or changed an interface.

For serious replication work, record the retrieval date, archive a permitted data snapshot privately, and document any changes to index classifications or data definitions.

## Data licence

This repository does not redistribute raw market data. Data remain subject to the terms and licensing requirements of their original providers.
