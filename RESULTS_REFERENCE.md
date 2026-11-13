# Results: 116 RCCL Bandwidth Test

**Main goal:** Measure single-GPU RCCL AllReduce bandwidth from a custom probe  
**Tools:** Build src/rccl_perf.cpp and run bin/all_reduce_perf across the message-size sweep

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_139 · recorded 2026-11-13). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 24.04.5 LTS · kernel 6.8.0-147-generic · driver 7.1.9.31600000
- **Run:** 2026-11-13T14:06:12Z · status **N/A-EXPECTED** (exit 0)
- **Time (mm:ss):** setup 00:12 · benchmark 00:03 · whole run 00:24
- **Source:** run 116_20261113_140612
- **Command:** `bash run_benchmark.sh --smoke --num-gpus 1 --min-bytes 8 --max-bytes 1024 --step-factor 2 --warmup-iters 1 --num-iterations 2`

| # | Metric | Result |
|---|---|---|
| 1 | All-reduce bus bandwidth at max message size, GB/s, na on single GPU (allreduce_busbw_gb_s) | na (expected: needs at least two GPUs) |
| 2 | All-reduce algorithm bandwidth at max message size, GB/s, na on single GPU (allreduce_algbw_gb_s) | na (expected: needs at least two GPUs) |
| 3 | All-reduce latency at min message size, us, na on single GPU (allreduce_latency_at_min_bytes_us) | na (expected: needs at least two GPUs) |
<!-- /profile:smoke -->
