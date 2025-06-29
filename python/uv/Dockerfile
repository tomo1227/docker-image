FROM python:3.13.5-slim-bookworm

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV DEBIAN_FRONTEND=noninteractive

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt update \
    && apt install -y sudo bash-completion wget make curl openssh-client locales \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

RUN chsh --shell /bin/bash ${USERNAME}
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /usr/local/bin/
ENV UV_SYSTEM_PYTHON=true \
    UV_PROJECT_ENVIRONMENT=/usr/local/
RUN chmod +x /usr/local/bin/
RUN chmod -R 775 /usr/local/lib/python3.13/site-packages/
RUN chown -R $USERNAME:$USERNAME /usr/local/lib/python3.13/site-packages/ \
    && chmod -R 775 /usr/local/lib/python3.13/site-packages/ \
    && chown -R $USERNAME:$USERNAME /bin \
    && chmod -R 775 /bin \
    && chown -R $USERNAME:$USERNAME /usr/local/bin \
    && chmod -R 775 /usr/local/bin

RUN echo 'export PS1="\[\033[0;32m\]\u\[\033[0m\] ➜ \[\033[1;34m\]\w\[\033[0m\] \$(if git rev-parse --is-inside-work-tree >/dev/null 2>&1; then echo \"(\[\033[1;31m\]\$(git rev-parse --abbrev-ref HEAD)\[\033[0m\])\"; fi)\$ "' >> /home/$USERNAME/.bashrc

RUN echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen ja_JP.UTF-8 && \
    dpkg-reconfigure locales && \
    /usr/sbin/update-locale LANG=ja_JP.UTF-8

USER $USERNAME

SHELL ["/bin/bash", "-c"]
CMD ["/bin/bash"]
WORKDIR /workspace
