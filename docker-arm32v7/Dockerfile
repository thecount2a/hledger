FROM arm32v7/ubuntu:21.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get -qq update && apt-get install -y --no-install-recommends tzdata

RUN apt-get install -y gcc g++ make xz-utils wget vim libtinfo5 git python3 libgmp-dev libtinfo-dev zlib1g-dev locales haskell-platform

RUN locale-gen en_US.UTF-8

RUN update-locale LC_ALL=en_US.UTF-8
RUN update-locale LANG=en_US.UTF-8
RUN update-locale LANGUAGE=en_US.UTF-8

ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8

RUN wget --no-check-certificate "https://github.com/llvm/llvm-project/releases/download/llvmorg-9.0.1/clang+llvm-9.0.1-armv7a-linux-gnueabihf.tar.xz" \
        && wget --no-check-certificate "https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-armv7-deb10-linux.tar.xz" \
        && tar xvf clang+llvm-9.0.1-armv7a-linux-gnueabihf.tar.xz \
        && tar xvf ghc-8.10.4-armv7-deb10-linux.tar.xz

RUN cd clang+llvm-9.0.1-armv7a-linux-gnueabihf && mv bin/* /usr/local/bin/ && mv include/* /usr/local/include/ && mv lib/* /usr/local/lib/ && cd .. && rm clang+llvm-9.0.1-armv7a-linux-gnueabihf.tar.xz

RUN cd ghc-8.10.4 && ./configure && make install && cd .. && rm -r ghc-8.10.4 && rm ghc-8.10.4-armv7-deb10-linux.tar.xz

# Run cabal update
RUN cabal update

# Build dependancies
#RUN cabal build --dependencies-only hledger-lib

# Not ideal to do a manual clone but let's try it, since we don't know where the one checked out by github actions is located
RUN git clone https://github.com/simonmichael/hledger.git

# Build static hledger binary
RUN cd hledger && cabal build --enable-executable-static --with-compiler=/usr/local/bin/ghc-8.10.4 hledger || true
