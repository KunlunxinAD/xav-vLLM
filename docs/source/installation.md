# Installation

This document describes how to install vllm-kunlun manually.

## Requirements

- **OS**: Ubuntu 22.04 
- **Software**:
  - Python >=3.10
  - PyTorch ≥ 2.5.1
  - vLLM (same version as vllm-kunlun)

## Setup environment using container
We provide a clean, minimal base image for your use`iregistry.baidu-int.com/kunlunxin-self-driving/xav:v1.4.0`.You can pull it using the `docker pull` command.
### Container startup script

:::::{tab-set}
:sync-group: install

::::{tab-item} start_docker.sh
:selected:
:sync: pip
```{code-block} bash
   :substitutions:
#!/bin/bash
XPU_NUM=8
DOCKER_DEVICE_CONFIG=""
if [ $XPU_NUM -gt 0 ]; then
    for idx in $(seq 0 $((XPU_NUM-1))); do
        DOCKER_DEVICE_CONFIG="${DOCKER_DEVICE_CONFIG} --device=/dev/xpu${idx}:/dev/xpu${idx}"
    done
    DOCKER_DEVICE_CONFIG="${DOCKER_DEVICE_CONFIG} --device=/dev/xpuctrl:/dev/xpuctrl"
fi
export build_image="iregistry.baidu-int.com/kunlunxin-self-driving/xav:v1.4.0"
docker run -itd ${DOCKER_DEVICE_CONFIG} \
    --net=host \
    --cap-add=SYS_PTRACE --security-opt seccomp=unconfined \
    --tmpfs /dev/shm:rw,nosuid,nodev,exec,size=32g \
    --cap-add=SYS_PTRACE \
    -v /home/users/vllm-kunlun:/home/vllm-kunlun \
    -v /usr/local/bin/xpu-smi:/usr/local/bin/xpu-smi \
    --name "$1" \
    -w /workspace \
    "$build_image" /bin/bash
```
::::
:::::
## Install vLLM-kunlun
### Install vLLM 0.10.1.1
```
conda activate python310_torch25_cuda

pip install vllm==0.10.1.1 --no-build-isolation --no-deps 
```
### Build and Install
Navigate to the vllm-kunlun directory and build the package:
```
git clone https://github.com/KunlunxinAD/xav-vLLM.git

cd xav-vLLM

pip install -r requirements.txt

python setup.py build

python setup.py install

```
### Replace eval_frame.py
Copy the eval_frame.py patch:
```
cp xav-vLLM/patches/eval_frame.py /root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages/torch/_dynamo/eval_frame.py
```
## Install custom ops
```
pip install https://klx-sdk-release-public.su.bcebos.com/xav/xav_vllm/v0.10.1.1/20251106/xtorch_ops-0.1.2071%2B0207a916-cp310-cp310-linux_x86_64.whl

pip install https://cce-ai-models.bj.bcebos.com/liangyucheng/xspeedgate_ops-0.0.0-cp310-cp310-linux_x86_64.whl
```

## Quick Start

### Set up the environment

```
chmod +x /workspace/xav-vLLM/setup_env.sh && source /workspace/xav-vLLM/setup_env.sh
```

### Run the server
:::::{tab-set}
:sync-group: install

::::{tab-item} start_service.sh
:selected:
:sync: pip
```{code-block} bash
   :substitutions:
python -m vllm.entrypoints.openai.api_server \
      --host 0.0.0.0 \
      --port 8356 \
      --model /models/Qwen3-8B\
      --gpu-memory-utilization 0.9 \
      --trust-remote-code \
      --max-model-len 32768 \
      --tensor-parallel-size 1 \
      --dtype float16 \
      --max_num_seqs 128 \
      --max_num_batched_tokens 32768 \
      --max-seq-len-to-capture 32768 \
      --block-size 128 \
      --no-enable-prefix-caching \
      --no-enable-chunked-prefill \
      --distributed-executor-backend mp \
      --served-model-name Qwen3-8B \
      --compilation-config '{"splitting_ops": ["vllm.unified_attention_with_output_kunlun",
            "vllm.unified_attention", "vllm.unified_attention_with_output",
            "vllm.mamba_mixer2"]}' \
```
::::
:::::
