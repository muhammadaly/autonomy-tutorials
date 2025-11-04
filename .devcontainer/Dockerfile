# .devcontainer/Dockerfile
FROM python:3.11-slim

# Avoid prompts, keep image small
ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    POETRY_VIRTUALENVS_CREATE=false

# create a non-root user (vscode is conventional)
ARG USER=vscode
ARG UID=1000
ARG GID=1000
RUN groupadd --gid ${GID} ${USER} \
 && useradd --uid ${UID} --gid ${GID} -m ${USER} \
 && apt-get update \
 && apt-get install -y --no-install-recommends build-essential git curl ca-certificates \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace

# Copy only dependency file first for docker cache
COPY requirements.txt /tmp/requirements.txt

# Install python deps (no cache)
RUN python -m pip install --upgrade pip \
 && pip install --no-cache-dir -r /tmp/requirements.txt || true

# Clear tmp and set user
RUN rm -f /tmp/requirements.txt
USER ${USER}

# Add a default PATH for local bin
ENV PATH="/home/${USER}/.local/bin:${PATH}"

# Default command does nothing (VS Code will run commands)
CMD [ "sleep", "infinity" ]
