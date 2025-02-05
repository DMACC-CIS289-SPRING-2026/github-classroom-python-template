FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

ARG DEBIAN_FRONTEND=noninteractive
ARG USER=vscode
RUN sudo apt-get update \
    && sudo apt-get install -y curl wget git sudo build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev \
    && sudo apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && usermod -s /usr/bin/zsh ${USER}

USER ${USER}
ARG HOME="/home/${USER}"
WORKDIR ${HOME}

# ENV BREW_PREFIX=/home/linuxbrew/.linuxbrew
# ENV PATH=${BREW_PREFIX}/sbin:${BREW_PREFIX}/bin:${PATH}
# COPY library-scripts/homebrew-debian.sh /tmp/library-scripts/
# RUN sudo bash /tmp/library-scripts/homebrew-debian.sh
RUN /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" \
    && echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/vscode/.zshrc \
    && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

ARG PYTHON_VERSION=3.11
ENV PYENV_ROOT=${HOME}/.pyenv
ARG PYENV_PATH="${PYENV_ROOT}/bin:${PYENV_ROOT}/shims"
ARG PDM_PATH="${HOME}/.local/bin"
ENV PATH="${PYENV_PATH}:${PDM_PATH}:$PATH"
RUN set -x \
    && curl -fsSL https://pyenv.run | bash \
    && echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc \
    && echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc \
    && echo 'eval "$(pyenv init -)"' >>${HOME}/.zshrc \
    && pyenv install -v ${PYTHON_VERSION} \
    && pyenv global ${PYTHON_VERSION}


RUN set -x \
&& brew install pyenv-virtualenv \
&& echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc

# ARG ZSH_CUSTOM=${HOME}/.oh-my-zsh/custom
# RUN curl -sSL https://pdm.fming.dev/install-pdm.py | python - \
#     && mkdir ${ZSH_CUSTOM}/plugins/pdm \
#     && pdm completion zsh > ${ZSH_CUSTOM}/plugins/pdm/_pdm \
#     && sed -i "s|^plugins=(|&pdm |" ${HOME}/.zshrc
