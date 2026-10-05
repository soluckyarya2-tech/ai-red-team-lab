FROM kalilinux/kali-rolling

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    nmap \
    sqlmap \
    hydra \
    nikto \
    whatweb \
    exploitdb \
    wordlists \
    curl \
    wget \
    git \
    python3 \
    python3-pip \
    python3-venv \
    netcat-openbsd \
    iputils-ping \
    dnsutils \
    socat \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Adversarial LLM scanner (pinned for reproducible rebuilds)
RUN pip3 install --break-system-packages garak==0.17.0 \
    || pip3 install --break-system-packages garak

CMD ["bash"]
