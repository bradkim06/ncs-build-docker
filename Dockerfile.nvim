ARG BASE_IMAGE
FROM ${BASE_IMAGE:-ncs-build:v2.4.2}
WORKDIR /root

# Set default shell during Docker image build to bash
SHELL ["/bin/bash", "-c"]

ENV DEBIAN_FRONTEND=noninteractive

ARG LLVM_VERSION=16
ARG APT_ARGS="--no-install-recommends -y"

# Install base packages
RUN apt-get -y update && \
	apt-get -y upgrade 

# Install dependencies
RUN <<EOT
    apt-get install ${APT_ARGS} lsb-release software-properties-common gnupg \
    neovim git curl fzf bat ripgrep
EOT

# Install LLVM and Clang
RUN wget ${WGET_ARGS} https://apt.llvm.org/llvm.sh && \
	chmod +x llvm.sh && \
	./llvm.sh ${LLVM_VERSION} all && \
	rm -f llvm.sh && \
    apt-get install ${APT_ARGS} clangd-12 && \
    update-alternatives --install /usr/bin/clangd clangd /usr/bin/clangd-12 100

# Install nodejs
RUN <<EOT
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
    apt-get install ${APT_ARGS} nodejs
EOT

# neovim configure
RUN <<EOT
    mkdir /root/.config
    git clone https://github.com/bradkim06/setting_files.git /root/.config/nvim
EOT

# Install vim plug
RUN sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim' && \
    nvim -u ~/.config/nvim/init.vim -c 'PlugUpdate' -c quitall

# Install lazygit
RUN <<EOT
    LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | grep -Po '"tag_name": "v\K[^"]*')
    curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/latest/download/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
    tar xf lazygit.tar.gz lazygit
    install lazygit /usr/local/bin
EOT

# Install fasd
RUN <<EOT
    add-apt-repository -y ppa:aacebedo/fasd
    apg-get update
    apt-get install fasd
    # fasd bashrc
    echo -e "\n# Fasd comes with some useful aliases
alias a='fasd -a'        # any
alias s='fasd -si'       # show / search / select
alias d='fasd -d'        # directory
alias f='fasd -f'        # file
alias sd='fasd -sid'     # interactive directory selection
alias sf='fasd -sif'     # interactive file selection
alias z='fasd_cd -d'     # cd, same functionality as j in autojump
alias zz='fasd_cd -d -i' # cd with interactive selection
" >> ~/.bashrc
source ~/.bashrc
    echo 'eval "$(fasd --init auto)"' >> ~/.bashrc
EOT

# Clean up stale packages
RUN apt-get clean -y && \
	apt-get autoremove --purge -y && \
	rm -rf /var/lib/apt/lists/*

WORKDIR /ncs

