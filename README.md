# Runner Comparison Depot vs. Hosted GitHub Actions

## Runners
| Runner             | Host   | CPU | Memory | Storage | Cache Size | Cost per Minute |
| ------------------ | ------ | --- | ------ | ------- | ---------- | --------------- |
| ubuntu-24.04       | github | 2   | 7Gb    | 14Gb    | 10Gb       | $0.008 USD      |
| depot-ubuntu-22.04 | depot  | 2   | 8Gb    | 100Gb   | 100Gb      | $0.004 USD      |

Note: GitHub Actions runners are billed per minute, rounded up to the next minute. Depot runners are billed per second. The cost estimate in each run accounts for this.

## Benchmarks

### Run 1

The first run will not contain any cache, so the runner will have to install all dependencies from scratch.

| Runner             | Install Time (s) | Build Time (s) | Total Time (m) | Cost (USD) | Logs                                                                                              |
| ------------------ | ---------------- | -------------- | -------------- | ---------- | ------------------------------------------------------------------------------------------------- |
| ubuntu-24.04       | 17.7             | 81.706         | 2m 12s         | $0.024     | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10910152335/job/30279957077) |
| depot-ubuntu-22.04 | 10.1             | 44.481         | 1m 40s         | $0.006     | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10910152335/job/30279957438) |


Notes: Depot's faster network speeds allowed for 70% faster download of dependencies. Though the number of CPU cores is the same, Depot was able to build the project over 45% faster than the default GitHub runner.

### Run 2

The second run will pull the dependencies from the cache created in the first run.

| Runner             | Install Time (s) | Build Time (s) | Total Time (m) | Cost (USD) | Logs  |
| ------------------ | ---------------- | -------------- | -------------- | ---------- | ----- |
| ubuntu-24.04       |                  |                |                |            | [🔗]() |
| depot-ubuntu-22.04 |                  |                |                |            | [🔗]() |