FROM golang:1.19.0-alpine as base
RUN     apk update && \
        apk add git vim ctags python3 &&\
        GO111MODULE=off go get github.com/jstemmer/gotags &&\
        mkdir -p  /root/.vim/bundle
RUN     git clone https://github.com/VundleVim/Vundle.vim.git /root/.vim/bundle/Vundle.vim

FROM base AS go-vim-install
COPY    vim-go.vimrc /root/.vimrc
RUN     vim -u /root/.vimrc +PluginInstall +qa && \
        vim -u /root/.vimrc +GoInstallBinaries +qa

FROM base
COPY --from=go-vim-install /go/bin /go/bin
COPY ycm.vimrc /root/ycm.vimrc
RUN     vim -u /root/ycm.vimrc +PluginInstall +qa
WORKDIR /root/.vim/bundle/youcompleteme
RUN     apk add python3-dev cmake make clang build-base llvm-static llvm-dev clang-static clang-dev linux-headers && \
        python3 ./install.py --go-completer --force-sudo && \
        apk del python3-dev cmake make clang build-base llvm-static llvm-dev clang-static clang-dev linux-headers && \
        rm /root/ycm.vimrc

COPY vimrc /root/.vimrc
RUN     vim -u /root/.vimrc +PluginInstall +qa
WORKDIR /src
CMD vim +NERDTreeTabsOpen
