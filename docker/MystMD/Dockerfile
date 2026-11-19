FROM python:latest

# Necessary for MyST to be accessible from the host
ENV HOST=0.0.0.0

# Install Node.js LTS and useful tools
RUN apt-get update && \
    apt-get install -y curl git && \
    curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install MyST Markdown CLI
RUN npm install -g mystmd

# Optional Python packages commonly useful with technical documentation
RUN pip install --no-cache-dir \
    jupyter \
    jupyterlab \
    matplotlib \
    pandas \
    numpy \
    scipy \
    seaborn \
    plotly

WORKDIR /workspace

EXPOSE 3000

CMD ["myst", "start", "--keep-host", "--port", "3000"]