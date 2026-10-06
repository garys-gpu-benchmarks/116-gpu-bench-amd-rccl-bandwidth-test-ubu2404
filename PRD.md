# PRD.md:  "The Why"; Product requirements, benchmark metadata table, high-level requirements, etc.

Product Requirements Document

"The Why"; Product requirements, benchmark metadata table, high-level requirements, etc. Defines the benchmark goal, validation objective, test name, benchmark number, category, and high-level success criteria.

## Benchmark Matrix Document Metadata (via benchmark_specification.json)

This PRD.md section is populated from benchmark_specification.json, which is the structured source of benchmark-specific product requirements.

## Workload Number
116

## Workload Name
RCCL Bandwidth Test

## Execution Summary (Run and Measure)
Build src/rccl_perf.cpp and run bin/all_reduce_perf with yaml min_bytes, max_bytes, iterations, warmup, step_factor, and num_gpus. Save the transcript to rccl_all_reduce.txt. On one GPU the bandwidth metrics are na; two or more requested GPUs must initialize

## Main Goal
Measure single-GPU RCCL AllReduce bandwidth from a custom probe

## Validation Objective
Validates that RCCL runs all five collectives across the message-size sweep. With one rank there is no GPU-to-GPU traffic, so the three metrics are na (requires at least two GPUs)

## Workload Category
Memory, Bandwidth & Data Movement

## Validation Requirement

The benchmark must include an automated SQLite-integrated validation layer that verifies persisted results from `results/benchmark.db`. Validation must confirm:

1. The benchmark run completed successfully with no tool errors.
2. Required samples and aggregate metrics were persisted for every swept shape.
3. Metrics are finite and physically sensible (positive, within plausible bounds).
4. Measured values satisfy configured thresholds when the workload defines pass/fail gates.
5. The benchmark fails validation when required data is missing, invalid, or outside bounds.

## Non-Functional Requirements

| Requirement | Target |
|---|---|
| Automation | Runs to completion without manual intervention after `bash run_benchmark.sh` |
| Idempotency | Re-running `run_benchmark.sh` appends a new run; never corrupts existing rows |
| Persistence | All metrics survive script exit; `results/benchmark.db` is the durable record |
