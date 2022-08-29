FROM python:3.10-alpine3.16 as base
RUN     apk update && \
        apk add curl git vim libstdc++&& \
        mkdir -p  /root/.vim/autoload /root/.vim/bundle &&\
        curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim

FROM base as plugins
RUN git clone https://github.com/preservim/nerdtree.git /root/.vim/bundle/nerdtree && \
    git clone https://github.com/preservim/tagbar.git /root/.vim/bundle/tagbar && \
    git clone https://github.com/jistr/vim-nerdtree-tabs.git /root/.vim/bundle/vim-nerdtree-tabs && \
    git clone https://github.com/ycm-core/YouCompleteMe /root/.vim/bundle/YouCompleteMe && \
    cd /root/.vim/bundle/YouCompleteMe && \
    git submodule update --init --recursive && \
    git clone https://github.com/tpope/vim-rhubarb /root/.vim/bundle/vim-rhubarb && \
    git clone https://github.com/tpope/vim-fugitive.git /root/.vim/bundle/vim-fugitive && \
    git clone https://github.com/micha/vim-colors-solarized.git /root/.vim/bundle/vim-colors-solarized


FROM base AS install_ycm
COPY --from=plugins /root/.vim/bundle/YouCompleteMe /root/.vim/bundle/YouCompleteMe
WORKDIR /root/.vim/bundle/YouCompleteMe
RUN     apk add cmake make clang build-base llvm-static llvm-dev clang-static clang-dev linux-headers && \
        python3 ./install.py --force-sudo
FROM base
COPY plugins.vim /root/.vimrc
COPY --from=plugins /root/.vim/bundle /root/.vim/bundle
COPY --from=install_ycm /root/.vim/bundle/YouCompleteMe /root/.vim/bundle/YouCompleteMe
WORKDIR /src
CMD vim +NERDTreeTabsOpen
