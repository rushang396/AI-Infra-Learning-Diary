# AI-Infra-Learning-Diary

[![GitHub license](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/)

## 🚀 项目简介
本项目记录了我对 [模型名称，如: Llama-3-8B] 在 [推理框架，如: vLLM] 下的推理性能调优实验。旨在通过分析显存占用与吞吐量瓶颈，提升推理服务的并发能力。

## 🛠️ 技术栈
- **硬件环境**: NVIDIA RTX 4090 (24GB VRAM)
- **软件环境**: CUDA 12.1, PyTorch 2.3, vLLM 0.4.0
- **核心技术**: PagedAttention, KV Cache 优化, 动态批处理 (Continuous Batching)

## 📊 实验数据 (你的核心卖点)
*面试官最爱看数据，用表格呈现你的优化成果*

| Batch Size | 原生 vLLM 吞吐量 (tokens/s) | 优化后吞吐量 (tokens/s) | 提升幅度 |
| :--- | :--- | :--- | :--- |
| 8 | 120 | 160 | +33% |
| 16 | 180 | 250 | +38% |

> **关键发现**: 通过调整 `max_num_seqs` 和 `block_size` 参数，我发现显存碎片率从 15% 降至 3%。

## 🛠️ 遇到的核心挑战 (这体现你的解决问题能力)
* **挑战**: 运行压力测试时出现 `CUDA OOM (Out of Memory)`。
* **排查过程**:
  1. 使用 `nvidia-smi` 监控显存占用峰值。
  2. 使用 `nsys` 分析 Kernel 执行时间，定位到是 KV Cache 预分配过大。
* **解决方案**: 动态调整显存预分配比例 (GPU Memory Utilization)，成功在 24GB 显存内跑通了更大 Batch Size。

## 📂 目录结构
```text
.
├── src/                # 性能测试脚本
├── logs/               # 实验记录与报错日志
├── scripts/            # 自动化部署 Shell 脚本
└── README.md
