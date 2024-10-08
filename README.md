# Runner Comparison Depot vs. Hosted GitHub Actions

This benchmark is configured to run the same GitHub Actions workflow job on two different runners to compare performance and cost.

## Runners
| Runner             | Host   | CPU | Memory | Storage | Cache Size | Cost per Minute |
| ------------------ | ------ | --- | ------ | ------- | ---------- | --------------- |
| ubuntu-22.04       | github | 2   | 7Gb    | 14Gb    | 10Gb       | $0.008 USD      |
| depot-ubuntu-22.04 | depot  | 2   | 8Gb    | 100Gb   | 100Gb      | $0.004 USD      |

Note: GitHub Actions runners are billed per minute, rounded up to the next minute. Depot runners are billed per second. The cost estimate in each run accounts for this.

## Benchmarks #1

The test will checkout a recent tag of the [Astro](https://github.com/withastro/astro) project, install dependencies, and build the project using the [`actions/setup-node`](https://github.com/actions/setup-node) with caching enabled for `pnpm`.


### Run 1

The first run will not contain any cache, so the runner will have to install all dependencies from scratch.

| Runner             | Install Time (s) | Build Time (s) | Total Time (m) | Cost (USD) | Logs                                                                                              |
| ------------------ | ---------------- | -------------- | -------------- | ---------- | ------------------------------------------------------------------------------------------------- |
| ubuntu-22.04       | 17.7             | 81.706         | 2m 12s         | $0.024     | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10910152335/job/30279957077) |
| depot-ubuntu-22.04 | 10.1             | 44.481         | 1m 40s         | $0.0066    | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10910152335/job/30279957438) |


Notes: Depot's faster network speeds allowed for 70% faster download of dependencies. Though the number of CPU cores is the same, Depot was able to build the project over 45% faster than the default GitHub runner.

### Run 2

The second run will pull the dependencies from the cache created in the first run.

| Runner             | Install Time (s) | Build Time (s) | Total Time (m) | Cost (USD) | Logs                                                                                              |
| ------------------ | ---------------- | -------------- | -------------- | ---------- | ------------------------------------------------------------------------------------------------- |
| ubuntu-22.04       | 11.1s            | 86.819         | 2m 1s          | $0.024     | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10910569666/job/30281295191) |
| depot-ubuntu-22.04 | 4.8s             | 45.236s        | 1m 35s         | $0.0063    | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10910569666/job/30281295485) |

Notes: Although just one second over the 2 minute mark, the GitHub runner will be charged at 3 minutes. With caching enabled. Depot was still able to download and unpack the cache 56.7% faster than the GitHub runner.

## Benchmarks #2 (Larger Cache)


The test will checkout a recent tag of the [Next.js](https://github.com/vercel/next.js/tags) project, install dependencies, and build the project using the [`actions/setup-node`](https://github.com/actions/setup-node) with caching enabled for `pnpm`. Next.js has a larger dependency tree than Astro, so this test will be more representative of a larger real-world project.

Note: Both runners have had their caches cleared before running this test.

### Run 1

The first run will not contain any cache, so the runner will have to install all dependencies from scratch.

| Runner             | Install Time (s) | Build Time (s) | Total Time (m) | Cost (USD) | Logs                                                                                              |
| ------------------ | ---------------- | -------------- | -------------- | ---------- | ------------------------------------------------------------------------------------------------- |
| ubuntu-22.04       | 53.8s            | 105.641        | 4m 49s         | $0.04      | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10911079312/job/30282961574) |
| depot-ubuntu-22.04 | 40.5             | 61.658         | 3m 32s         | $0.014     | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10911079312/job/30282961917) |

Notes: Depot is 24% faster to download and install dependencies and builds over 60% faster while being 65% cheaper.

### Run 2

The second run will pull the dependencies from the cache created in the first run.

| Runner             | Install Time (s) | Build Time (s) | Total Time (m) | Cost (USD) | Logs                                                                                              |
| ------------------ | ---------------- | -------------- | -------------- | ---------- | ------------------------------------------------------------------------------------------------- |
| ubuntu-22.04       | 16.8             | 109.457        | 4m 6s          | $0.04      | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10911289557/job/30283616457) |
| depot-ubuntu-22.04 | 10.7             | 59.139         | 3m 1s          | $0.012     | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10911289557/job/30283616770) |

With cache enabled, Depot was 57% faster to download and unpack the cache.

### Run 3

The second run will pull the dependencies from the cache created in the first run.

| Runner             | Install Time (s) | Build Time (s) | Total Time (m) | Cost (USD) | Logs                                                                                              |
| ------------------ | ---------------- | -------------- | -------------- | ---------- | ------------------------------------------------------------------------------------------------- |
| ubuntu-22.04       | 19.5             | 108.473        | 4m 17s         | $0.04      | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10923129402/job/30319011061) |
| depot-ubuntu-22.04 | 10.4             | 63.558         | 2m 44s         | $0.0109    | [🔗](https://github.com/depot/Compare-Runners-Cache-Test/actions/runs/10923129402/job/30319011567) |

