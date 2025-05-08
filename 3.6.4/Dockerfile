## Pandoc 3.6.4 + pandoc-crossref 0.3.18.2

## build pandoc-crossref
FROM --platform=linux/arm64 munepi/tllangjapanese:2024 AS arm64-pandoc-3.6.4-build
ENV ARCH=arm64
ENV PANDOC_VERSION=3.6.4
ENV PANDOCXREF_VERSION=0.3.18.2

RUN wget "https://github.com/jgm/pandoc/releases/download/${PANDOC_VERSION}/pandoc-${PANDOC_VERSION}-1-${ARCH}.deb" && \
    dpkg -i pandoc-${PANDOC_VERSION}-1-${ARCH}.deb

## pandoc-crossref filter: Building from repo with stack
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            build-essential curl zlib1g-dev libgmp-dev libnuma-dev libtinfo6 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh && \
    . ${HOME}/.ghcup/env && \
    ghcup install ghc && \
    ghcup install cabal

RUN . ${HOME}/.ghcup/env && \
    git clone https://github.com/lierdakil/pandoc-crossref.git && \
    cd pandoc-crossref && \
    git checkout v${PANDOCXREF_VERSION} && \
    rm -f cabal.project.freeze && \
    cabal v2-update && \
    cabal v2-install --install-method=copy --constraint pandoc==${PANDOC_VERSION}

RUN cp -avf ${HOME}/.cabal/bin/* /usr/local/bin/


FROM --platform=linux/amd64 munepi/tllangjapanese:2024 AS amd64-pandoc-3.6.4-build
ENV ARCH=amd64
ENV PANDOC_VERSION=3.6.4
ENV PANDOCXREF_VERSION=0.3.18.2

RUN wget "https://github.com/jgm/pandoc/releases/download/${PANDOC_VERSION}/pandoc-${PANDOC_VERSION}-1-${ARCH}.deb" && \
    dpkg -i pandoc-${PANDOC_VERSION}-1-${ARCH}.deb
RUN wget "https://github.com/lierdakil/pandoc-crossref/releases/download/v${PANDOCXREF_VERSION}/pandoc-crossref-Linux-X64.tar.xz" && \
    tar xf pandoc-crossref-Linux-X64.tar.xz -C /usr/local/bin


################################################################################
## use prebuild image
FROM --platform=linux/arm64 munepi/pandoc:3.6.4.20250508 AS arm64-pandoc-3.6.4-prebuild
FROM --platform=linux/amd64 munepi/pandoc:3.6.4.20250508 AS amd64-pandoc-3.6.4-prebuild


## build main image
FROM --platform=linux/arm64 munepi/tllangjapanese:2024 AS arm64-pandoc-3.6.4-tl24
## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            libnuma1 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

COPY --from=arm64-pandoc-3.6.4-build  /usr/bin/pandoc /usr/local/bin/pandoc
COPY --from=arm64-pandoc-3.6.4-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/amd64 munepi/tllangjapanese:2024 AS amd64-pandoc-3.6.4-tl24
## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            liblua5.4-0 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

COPY --from=amd64-pandoc-3.6.4-build  /usr/bin/pandoc          /usr/local/bin/pandoc
COPY --from=amd64-pandoc-3.6.4-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


################################################################################
## for TeX Live 2023
FROM --platform=linux/arm64 munepi/tllangjapanese:2023 AS arm64-pandoc-3.6.4-tl23
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            libnuma1 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*
COPY --from=arm64-pandoc-3.6.4-prebuild  /usr/local/bin/pandoc /usr/local/bin/pandoc
COPY --from=arm64-pandoc-3.6.4-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
WORKDIR /data
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/amd64 munepi/tllangjapanese:2023 AS amd64-pandoc-3.6.4-tl23
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            liblua5.4-0 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*
COPY --from=amd64-pandoc-3.6.4-prebuild  /usr/local/bin/pandoc          /usr/local/bin/pandoc
COPY --from=amd64-pandoc-3.6.4-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
WORKDIR /data
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]

# end of file
