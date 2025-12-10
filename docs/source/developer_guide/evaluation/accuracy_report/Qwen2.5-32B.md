# Qwen2.5-32B

* vLLM Version: vLLM: 0.10.1.1 , vLLM-KunLun Version: v0.10.1.1
* Software Environment:OS: Ubuntu 22.04, PyTorch ≥ 2.5.1
* Hardware Environment: KunLun P800
* Parallel mode:TP4

```bash
+-----------+--------------------------+------------------+------+--------+---------+
| Dataset   | Metric                   | Subset           | Num  | Score  | Cat.0   |
+-----------+--------------------------+------------------+------+--------+---------+
| gsm8k     | mean_acc                 | main             | 1319 | 0.9158 | default |
| humaneval | pass@1                   | openai_humaneval |  164 | 0.878  | default |
| ifeval    | mean_prompt_level_strict | default          |  541 | 0.8059 | default |
| ifeval    | mean_inst_level_strict   | default          |  541 | 0.8765 | default |
| ifeval    | mean_prompt_level_loose  | default          |  541 | 0.8262 | default |
| ifeval    | mean_inst_level_loose    | default          |  541 | 0.8916 | default |
+-----------+--------------------------+------------------+------+--------+---------+
```