FROM arm32v7/ubuntu:21.04

RUN apt-get -qq update && \
    apt-get install -y gcc g++ make xz-utils wget vim libtinfo5 git python3 libgmp-dev libtinfo-dev zlib1g-dev locales haskell-platform

RUN locale-gen en_US.UTF-8

RUN update-locale LC_ALL=en_US.UTF-8
RUN update-locale LANG=en_US.UTF-8
RUN update-locale LANGUAGE=en_US.UTF-8

ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8

# Run cabal update
RUN cabal update

# Build dependancies
#RUN cabal build --dependencies-only hledger-lib

# Build static hledger binary
RUN cabal build --enable-executable-static hledger
