FROM ubuntu:20.04
LABEL maintainer="Hugging Face"
LABEL repository="diffusers"

ENV DEBIAN_FRONTEND=noninteractive

# For Chinese users: set aliyun source
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list \
    && apt-get clean

RUN apt-get -y update \
    && apt-get install -y software-properties-common \
    && add-apt-repository ppa:deadsnakes/ppa

RUN apt install -y bash \
                   build-essential \
                   git \
                   git-lfs \
                   curl \
                   ca-certificates \
                   libsndfile1-dev \
                   python3.10 \
                   python3-pip \
                   libgl1 \
                   python3.10-venv && \
    rm -rf /var/lib/apt/lists

# make sure to use venv
RUN python3.10 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"


ENV RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
ENV RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
# install Rust toolchain (for uv installation)
# RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
SHELL [ "/bin/bash", "-c", "source ~/.cargo/env"]

RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# pre-install the heavy dependencies (these can later be overridden by the deps from setup.py)
RUN python3.10 -m pip install --no-cache-dir --upgrade pip uv==0.1.11 && \
    python3.10 -m uv pip install --no-cache-dir \
        torch \
        torchvision \
        torchaudio \
        invisible_watermark \
        --index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    python3.10 -m uv pip install --no-cache-dir \
        accelerate \
        datasets \
        hf-doc-builder \
        huggingface-hub \
        Jinja2 \
        librosa \
        numpy \
        scipy \
        tensorboard \
        transformers matplotlib \
        --index-url https://pypi.tuna.tsinghua.edu.cn/simple

CMD ["/bin/bash"]
