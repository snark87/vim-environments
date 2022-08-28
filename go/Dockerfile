FROM golang:1.19.0-alpine as base
RUN     apk update && \
        apk add bash ctags curl git python3 nodejs vim &&\
        GO111MODULE=off go get github.com/jstemmer/gotags &&\
        mkdir -p  /root/.vim/autoload /root/.vim/bundle &&\
        curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
FROM base AS plugins
RUN git clone https://github.com/preservim/nerdtree.git /root/.vim/bundle/nerdtree && \
    git clone https://github.com/preservim/tagbar.git /root/.vim/bundle/tagbar && \
    git clone https://github.com/jistr/vim-nerdtree-tabs.git /root/.vim/bundle/vim-nerdtree-tabs && \
    git clone https://github.com/jparise/vim-graphql.git /root/.vim/bundle/vim-graphql && \
    git clone https://github.com/fatih/vim-go.git /root/.vim/bundle/vim-go && \
    git clone --branch release https://github.com/neoclide/coc.nvim.git /root/.vim/bundle/coc.nvim && \
    git clone https://github.com/tpope/vim-rhubarb /root/.vim/bundle/vim-rhubarb && \
    git clone https://github.com/tpope/vim-fugitive.git /root/.vim/bundle/vim-fugitive && \
    git clone https://github.com/micha/vim-colors-solarized.git /root/.vim/bundle/vim-colors-solarized

# additional installation steps for go-vim

FROM base AS go-vim-install
COPY plugins.vim /root/.vimrc
COPY --from=plugins /root/.vim/bundle/vim-go /root/.vim/bundle/vim-go
RUN     vim +GoInstallBinaries +qa

FROM base AS ncoc-install
RUN apk add yarn
COPY plugins.vim /root/.vimrc
COPY coc-settings.json /root/.vim/coc-settings.json
COPY --from=plugins /root/.vim/bundle/coc.nvim /root/.vim/bundle/coc.nvim
RUN vim -c 'CocInstall -sync coc-go coc-html coc-css coc-json|qa'

COPY --from=plugins /root/.vim/bundle /root/.vim/bundle
COPY --from=go-vim-install /go/bin /go/bin
COPY vimrc /root/.vimrc

WORKDIR /src
CMD vim +NERDTreeTabsOpen
