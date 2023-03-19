# Docker_PyTorch
- VSCode + PyTorchのDocker環境
- NVIDIA Dockerを使用


## 各種設定

### Python
Linter, Formatterを`.vscode/settings.json`で指定

- requirements.txt: `.devcontainer/requirements.txt`
- Linter: `flake8`
- Formatter: `black`


### CUDA, PyTorchのバージョン
`.devcontainer/Dockerfile`で指定

- CUDA 
    - https://hub.docker.com/r/nvidia/cuda/tags で探す
```
    FROM nvidia/cuda:11.1.1-cudnn8-devel-ubuntu20.04
```

- PyTorch
    - https://download.pytorch.org/whl/torch_stable.html で探す
```
    RUN pip3 install --no-cache-dir -U pip setuptools wheel \
    && pip3 install --no-cache-dir -r /work/requirements.txt \
    && pip3 install --no-cache-dir torch==1.9.0+cu111 torchvision==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html 

```

### GRAM
- `.devcontainer/docker-compose.yml`ファイル中の`shm_size`で指定