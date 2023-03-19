FROM nvidia/cuda:11.1.1-cudnn8-devel-ubuntu20.04

# コンテナ内のworkディレクトリの指定
WORKDIR /work

# TimeZoneの設定
ENV TZ=Asia/Tokyo
ENV DEBIAN_FRONTEND=noninteractive
RUN ln -snf /user/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 起動シェルをshからbashに変更
SHELL ["/bin/bash", "-c"]

RUN apt update && \
    apt install -y \
    wget \
    bzip2 \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    libgl1-mesa-dev \
    git \
    curl \
    unzip \
    file \
    xz-utils \
    python3 \
    python3-pip

# ユーザー名を指定
ARG USERNAME=user
ARG GROUPNAME=user

# UID, GIDはidコマンドで確認し，ログインユーザーのUID, GIDを指定する
ARG UID=1000
ARG GID=1000

# ユーザーを作成
RUN groupadd -g $GID $GROUPNAME && \ 
    useradd -m -u $UID -g $GID -s /bin/bash $USERNAME

# 初期ユーザーの変更
USER $USERNAME

# workdirの指定
WORKDIR /work

# pathを通す
ENV PATH="$PATH:/home/user/.local/bin"

# requirements.txtのコピー (いる?)
COPY requirements.txt ${PWD}

# パッケージのインストール
### cuda11.1, torch-1.9.0を指定
### cuda11.1は，ベースのイメージで指定したもの
### https://download.pytorch.org/whl/torch_stable.htmlからtorch="<version>"を探す
RUN pip3 install --no-cache-dir -U pip setuptools wheel \
    && pip3 install --no-cache-dir -r /work/requirements.txt \
    && pip3 install --no-cache-dir torch==1.9.0+cu111 torchvision==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html 
