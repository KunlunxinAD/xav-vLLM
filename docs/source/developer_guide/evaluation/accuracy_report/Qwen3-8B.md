# Qwen3-8B

* vLLM Version: vLLM: 0.10.1.1 , vLLM-KunLun Version: v0.10.1.1
* Software Environment:OS: Ubuntu 22.04, PyTorch ≥ 2.5.1
* Hardware Environment: KunLun P800
* Parallel mode:TP1

```bash
+-----------+--------------------------+--------------------+------+--------+---------+
| Dataset   | Metric                   | Subset             | Num  | Score  | Cat.0   |
+-----------+--------------------------+--------------------+------+--------+---------+
| gsm8k     | mean_acc                 | main               | 1319 | 0.9143 | default |
| humaneval | pass@1                   | openai_humaneval   | 164  | 0.8049 | default |
| ifeval    | mean_prompt_level_strict | default            | 541  | 0.8503 | default |
| ifeval    | mean_inst_level_strict   | default            | 541  | 0.8971 | default |
| ifeval    | mean_prompt_level_loose  | default            | 541  | 0.8762 | default |
| ifeval    | mean_inst_level_loose    | default            | 541  | 0.9165 | default |
| math_500  | mean_acc                 | Level 1            | 43   | 0.907  | default |
+-----------+--------------------------+--------------------+------+--------+---------+
```