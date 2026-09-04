# Internet Speed Dataset

Open, monthly-aggregated internet speed statistics — download, upload, ping —
by country and ISP, from real measurements taken on
**[internetspeedtest.net](https://internetspeedtest.net)**, a free browser-based
speed test built on the open-source [LibreSpeed](https://github.com/librespeed/speedtest)
engine.

This repo publishes exactly what powers the live, human-readable
**[Internet Speed Report](https://internetspeedtest.net/reports/internet-speed-report)**
— the same JSON files, in the same immutable-once-published form, just in a
place built for people who want to pull the numbers into their own analysis,
charts, or research instead of reading them on a web page.

## What's in here

One JSON file per month in [`data/`](data/), named `YYYY-MM.json`. Each file
contains:

- **Global stats**: sample count, average, median, p25, p75 for download
  (Mbps), upload (Mbps), and ping (ms), across all valid tests that month.
- **Per-country breakdown**: the same stats, for any country with at least
  30 valid samples that month.
- **Per-ISP breakdown**: the same stats, for any ISP with at least 20 valid
  samples that month.
- **Methodology metadata**: exclusion rules, percentile method, and an
  explicit caveat about how country/ISP attribution works (see below).

A month is only published at all if it has at least 200 valid tests
globally — thin months are skipped rather than published with misleadingly
precise numbers.

## Methodology

- **Percentiles**: linear interpolation.
- **Exclusions**: a row with a missing or failed ISP lookup is dropped
  entirely. A zero value for one metric (say, upload) excludes that row from
  *that metric's* stats only — it doesn't disqualify the row's valid download
  or ping numbers.
- **Country/ISP attribution**: self-reported by the client at test time via a
  third-party IP-lookup service, not independently verified. Treat this as
  *measurements observed by internetspeedtest.net*, not a statistically
  representative national broadband sample — sample sizes and geographic
  coverage vary a lot month to month and skew toward internetspeedtest.net's
  own visitor mix, not the general population.
- **Immutability**: once a month's file is published, its numbers don't
  change retroactively. If a mistake is ever found, a correction will be
  noted rather than silently editing history.

## Using the data

No API, no auth, no rate limit — just fetch the raw file:

```bash
curl https://raw.githubusercontent.com/internetspeedtest-net/internetspeedtest-dataset/main/data/2026-08.json
```

Or clone the whole history:

```bash
git clone https://github.com/internetspeedtest-net/internetspeedtest-dataset.git
```

## License

Data files in this repository are licensed under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use them for
anything, commercial or not, as long as you credit **internetspeedtest.net**
and link back to [internetspeedtest.net](https://internetspeedtest.net) or
this repository. See [`LICENSE`](LICENSE) for the full text.

## Questions / corrections

Open an issue on this repo. If you spot something that looks wrong in a
published month, please say which file and which field — since months are
meant to be immutable, a real correction will be documented rather than
silently changed.
