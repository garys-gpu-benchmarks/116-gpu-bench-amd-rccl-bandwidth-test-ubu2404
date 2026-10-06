# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Builds src/rccl_perf.cpp and runs bin/all_reduce_perf only. min_bytes, max_bytes, num_iterations, warmup_iters, step_factor, and num_gpus are forwarded. On one GPU the bandwidth metrics are na; two or more requested GPUs must initialize. Copied sibling perf names are not launched. Sweep dimensions: num_gpus, collective, min_bytes, max_bytes, step_factor, warmup_iters, num_iterations.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| num_gpus | `--num-gpus` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| collective | `--collective` | smoke=all, baseline=all, extended=all | all | From Parameter list; see Execution Description With Parameters. |
| min_bytes | `--min-bytes` | smoke=8, baseline=8, extended=8 | 8 | From Parameter list; see Execution Description With Parameters. |
| max_bytes | `--max-bytes` | smoke=1024, baseline=67108864, extended=67108864 | 67108864 | From Parameter list; see Execution Description With Parameters. |
| step_factor | `--step-factor` | smoke=2, baseline=2, extended=2 | 2 | From Parameter list; see Execution Description With Parameters. |
| warmup_iters | `--warmup-iters` | smoke=1, baseline=5, extended=10 | 5 | From Parameter list; see Execution Description With Parameters. |
| num_iterations | `--num-iterations` | smoke=2, baseline=440000, extended=1355000 | 440000 | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Build src/rccl_perf.cpp and run bin/all_reduce_perf. Copied sibling *_perf names are the same AllReduce probe and are not launched
```

## Raw Output Format

all_reduce_perf stdout in rccl_all_reduce.txt plus one CSV row. A single row is duplicated to two samples

sample_index,status,collective,num_gpus,bytes,algbw_gb_s,busbw_gb_s,latency_us,error_message
0,ok,all_reduce,1,1024,na (requires at least two GPUs),na (requires at least two GPUs),na (requires at least two GPUs),

## Metrics

- **#1: Bus bandwidth at max message size, GB/s** — stored as `busbw_gb_s`.
- **#2: Algorithm bandwidth at max message size, GB/s** — stored as `algbw_gb_s`.
- **#3: Latency at min message size, us** — stored as `latency_us`.

## Framework

Builds src/rccl_perf.cpp and runs bin/all_reduce_perf only. min_bytes, max_bytes, num_iterations, warmup_iters, step_factor, and num_gpus are forwarded. On one GPU the bandwidth metrics are na; two or more requested GPUs must initialize.

## Installation and Execution Summary

Build src/rccl_perf.cpp and run bin/all_reduce_perf with yaml min_bytes, max_bytes, iterations, warmup, step_factor, and num_gpus. Save the transcript to rccl_all_reduce.txt. On one GPU the bandwidth metrics are na; two or more requested GPUs must initialize

## Platform Portability

- **AMD (primary):** ```bash
Build src/rccl_perf.cpp and run bin/all_reduce_perf. Copied sibling *_perf names are the same AllReduce probe and are not launched
```
- **NVIDIA:** Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

all_reduce_perf stdout in rccl_all_reduce.txt plus one CSV row. A single row is duplicated to two samples

sample_index,status,collective,num_gpus,bytes,algbw_gb_s,busbw_gb_s,latency_us,error_message
0,ok,all_reduce,1,1024,na (requires at least two GPUs),na (requires at least two GPUs),na (requires at least two GPUs),

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Builds src/rccl_perf.cpp and runs bin/all_reduce_perf only. min_bytes, max_bytes, num_iterations, warmup_iters, step_factor, and num_gpus are forwarded. On one GPU the bandwidth metrics are na; two or more requested GPUs must initialize.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Builds src/rccl_perf.cpp and runs bin/all_reduce_perf only. min_bytes, max_bytes, num_iterations, warmup_iters, step_factor, and num_gpus are forwarded. On one GPU the bandwidth metrics are na; two or more requested GPUs must initialize.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
