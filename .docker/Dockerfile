# File: .docker/Dockerfile.python
FROM python:3.8-slim-bookworm

# Avoid prompts from apt
ENV DEBIAN_FRONTEND=noninteractive

# Create a non-root user
RUN useradd -m -s /bin/bash researcher

# Set environment variables
ENV HOME=/home/researcher \
    JUPYTER_RUNTIME_DIR=/home/researcher/.jupyter/runtime \
    JUPYTER_DATA_DIR=/home/researcher/.jupyter

# Set working directory
WORKDIR /litreview

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    g++ \
    git \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements file
COPY requirements.txt .

# Install Python packages
RUN pip install --no-cache-dir -r requirements.txt

# Create Jupyter directories and fix permissions
RUN mkdir -p /home/researcher/.jupyter/runtime && \
    chown -R researcher:researcher /home/researcher

# Expose Jupyter port
EXPOSE 8888

# Command to run Jupyter Notebook
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--NotebookApp.token=''", "--NotebookApp.password=''"]

# File: .docker/Dockerfile.latex
FROM texlive/texlive:latest

# Install LaTeX packages and other dependencies
RUN tlmgr update --self && tlmgr install \
    latexmk \
    texliveonfly \
    biblatex \
    biber \
    hyperref \
    cleveref \
    csquotes \
    booktabs \
    graphicx \
    caption \
    subfigure \
    amsmath \
    amssymb \
    && apt-get update && apt-get install -y \
    python3-pygments \
    && rm -rf /var/lib/apt/lists/*

# Add script to install additional packages
RUN echo '#!/bin/bash\n\
tlmgr install "$@"' > /usr/local/bin/install-tex-packages && \
    chmod +x /usr/local/bin/install-tex-packages

# Set working directory
WORKDIR /litreview

# Keep container running
CMD ["tail", "-f", "/dev/null"]
