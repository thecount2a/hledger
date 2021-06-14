FROM alpine:edge

# Borrowed from fossas/haskell-static-alpine, copied here for transparency
# Setup build environment
RUN apk --no-cache add binutils-gold curl gcc g++ git gmp-dev ncurses-dev ncurses-static libffi-dev make xz tar perl zlib-dev zlib-static
RUN mkdir -p ~/.ghcup/bin && curl https://downloads.haskell.org/~ghcup/x86_64-linux-ghcup > ~/.ghcup/bin/ghcup && chmod +x ~/.ghcup/bin/ghcup
ENV PATH="/root/.cabal/bin:/root/.ghcup/bin:$PATH"
RUN ghcup install ghc 8.10.4 && ghcup set ghc 8.10.4 && ghcup install cabal

# Run cabal update
RUN cabal update

# Get access to hledger source to build
COPY . /hledger

# Build static hledger binary
RUN cd hledger && cabal build --enable-executable-static all

# Strip symbols from binary
RUN cd hledger && mkdir /tmp/hledger && cp dist-newstyle/build/x86_64-linux/ghc-*/hledger-web-*/x/hledger-web/build/hledger-web/hledger-web /tmp/hledger/hledger-web-static-linux-x64 && cp dist-newstyle/build/x86_64-linux/ghc-*/hledger-ui-*/x/hledger-ui/build/hledger-ui/hledger-ui /tmp/hledger/hledger-ui-static-linux-x64 && cp dist-newstyle/build/x86_64-linux/ghc-*/hledger-*/x/hledger/build/hledger/hledger /tmp/hledger/hledger-static-linux-x64 && strip /tmp/hledger/hledger-web-static-linux-x64 && strip /tmp/hledger/hledger-ui-static-linux-x64 && strip /tmp/hledger/hledger-static-linux-x64
