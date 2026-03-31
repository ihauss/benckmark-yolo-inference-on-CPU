# YOLOv8 Inference Benchmark (CPU)

This project benchmarks different inference engines for **YOLOv8n** on CPU, with a focus on real-world deployment performance.

The goal is to compare multiple runtimes under the same conditions, including preprocessing and post-processing (NMS), in order to obtain fair and reproducible results.

## Compared Engines

* PyTorch (Ultralytics)
* ONNX Runtime
* OpenVINO
* TensorFlow Lite (TFLite)

## Methodology

* Same model: YOLOv8n
* Same input size: 640x640
* Same preprocessing pipeline
* Post-processing (NMS) included in all exported models
* Multiple runs (30 iterations) with warmup
* Metrics:

  * Latency (ms)
  * FPS (frames per second)
  * Standard deviation (stability)

## Objective

This benchmark aims to answer:

> *Which inference engine provides the best performance on CPU under realistic conditions?*

It also highlights how runtime environments (e.g., virtualization, CPU constraints) impact performance across different frameworks.

## Results Overview

TFLite achieved the best performance in this setup, followed by ONNX and PyTorch, while OpenVINO underperformed due to hardware and environment limitations.

Full results and analysis are available below.

## Implementation

[Notebook link](https://colab.research.google.com/drive/1nkX69-WIJYuTYAQ-6p7AOPCv6jKGqtEL?usp=sharing)

## Results

| engine   | mean_time | std_time | fps      | iters | latency_ms |
|----------|-----------|----------|----------|-------|------------|
| TFLite   | 0.160710  | 0.005133 | 6.222388 | 30    | 160.710009 |
| ONNX     | 0.199717  | 0.038277 | 5.007092 | 30    | 199.716703 |
| PyTorch  | 0.221439  | 0.007849 | 4.515920 | 30    | 221.438813 |
| OpenVINO | 0.264024  | 0.090061 | 3.787539 | 30    | 264.023670 |

## Benchmark Analysis

The benchmark shows that TFLite achieves the best performance on CPU, reaching 6.22 FPS, followed by ONNX and PyTorch, while OpenVINO performs the worst in this setup.

**Key insights**:

TFLite provides the best performance and stability, making it well-suited for lightweight CPU inference.
ONNX offers a strong balance between speed and portability, making it a practical choice for production systems.
PyTorch is slower due to its dynamic execution model and higher runtime overhead.
OpenVINO, despite being optimized for Intel CPUs, underperforms in this environment.

Explanation:

* The unexpected OpenVINO performance can be attributed to:

* Limited CPU resources (1 core / 2 threads)
Virtualized environment (KVM)
Reduced efficiency of low-level optimizations (SIMD, threading)
Conclusion:

* In constrained or virtualized CPU environments, lightweight runtimes like TFLite may outperform traditionally optimized frameworks like OpenVINO.
