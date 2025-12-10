# Qwen3-30B-A3B-coder

* vLLM Version: vLLM: 0.10.1.1 , vLLM-KunLun Version: v0.10.1.1
* Software Environment:OS: Ubuntu 22.04, PyTorch ≥ 2.5.1
* Hardware Environment: KunLun P800
* Parallel mode:TP4

```bash
+-----------------+-------------+--------------------+------+--------+---------+
| Dataset         | Metric      | Subset             | Num  | Score  | Cat.0   |
+-----------------+-------------+--------------------+------+--------+---------+
| gsm8k           | mean_acc    | main               | 1319 | 0.9272 | default |
| humaneval       | pass@1      | openai_humaneval   | 164  | 0.9146 | default |
| live_code_bench | pass@1      | release_latest     | 714  | 0.5644 | default |
+-----------------+-------------+--------------------+------+--------+---------+
```