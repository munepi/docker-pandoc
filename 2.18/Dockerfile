## Pandoc 2.18 + pandoc-crossref 0.3.12.2
## pandoc/core:2.18-ubuntu  cat /etc/os-release
## => VERSION="20.04.4 LTS (Focal Fossa)"
## texlive-base (2019.20200218-1)
##
## linux/amd64
## pandoc 2.18
## Compiled with pandoc-types 1.22.2, texmath 0.12.5, skylighting 0.12.3,
## citeproc 0.7, ipynb 0.2, hslua 2.1.0
## Scripting engine: Lua 5.3
##
## pandoc-crossref v0.3.12.2 git commit 4902e83bdc8662b63a895b40dfa7b8aba936465e
## (HEAD) built with Pandoc v2.18, pandoc-types v1.22.2 and GHC 8.8.4
##
## linux/arm64
## pandoc 2.18
## Compiled with pandoc-types 1.22.2, texmath 0.12.5, skylighting 0.12.3,
## citeproc 0.7, ipynb 0.2, hslua 2.2.0
## Scripting engine: Lua 5.4
##
## pandoc-crossref v0.3.12.2 git commit 14cbc8c2886c18eeb64046b6e8c0e7accf321f10
## (HEAD) built with Pandoc v2.17.1.1, pandoc-types v1.22.1 and GHC 8.10.7

## build pandoc-crossref
FROM --platform=linux/amd64 pandoc/core:2.18-ubuntu AS amd64-pandoc-2.18-build

FROM --platform=linux/arm64 munepi/tllangjapanese-base AS arm64-pandoc-2.18-build

ENV ARCH		arm64
ENV PANDOC_VERSION	2.18
ENV PANDOCXREF_VERSION	0.3.12.2a

VOLUME ["/root/.stack/"]

RUN wget "https://github.com/jgm/pandoc/releases/download/${PANDOC_VERSION}/pandoc-${PANDOC_VERSION}-1-${ARCH}.deb" && \
    dpkg -i *.deb

## pandoc-crossref filter: Building from repo with stack
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            haskell-stack llvm-9 zlib1g-dev libnuma-dev libtinfo5 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/lierdakil/pandoc-crossref.git && \
    cd pandoc-crossref && \
    git checkout v${PANDOCXREF_VERSION} && \
    stack install && \
    mv -v ${HOME}/.local/bin/* /usr/local/bin/ && \
    cd ../ && rm -rf pandoc-crossref

# # FROM --platform=linux/amd64 tllangjapanese-base AS amd64-pandoc-2.18-build
#
# ENV ARCH		amd64
# ENV PANDOC_VERSION	2.18
# ENV PANDOCXREF_VERSION	0.3.12.2a
#
# RUN wget "https://github.com/jgm/pandoc/releases/download/${PANDOC_VERSION}/pandoc-${PANDOC_VERSION}-1-${ARCH}.deb" && \
#     dpkg -i *.deb
#
# RUN wget "https://github.com/lierdakil/pandoc-crossref/releases/download/v${PANDOCXREF_VERSION}/pandoc-crossref-Linux.tar.xz" && \
#     tar xf pandoc-crossref-Linux.tar.xz -C /usr/local/bin
#
# ## pandoc-crossref filter: Building from repo with stack
# # RUN apt-get update && \
# #         apt-get install -y --no-install-recommends \
# #             ghc haskell-stack llvm-9 zlib1g-dev libnuma-dev libtinfo5 && \
# #         apt-get clean && \
# #         rm -rf /var/lib/apt/lists/*
#
# # RUN git clone https://github.com/lierdakil/pandoc-crossref.git && \
# #     cd pandoc-crossref && \
# #     git checkout v0.3.12.2a && \
# #     stack --no-install-ghc --system-ghc setup && \
# #     stack config set system-ghc  true  --global && \
# #     stack config set install-ghc false --global && \
# #     stack install && \
# #     mv -v ${HOME}/.local/bin/* /usr/local/bin/ && \
# #     cd ../ && rm -rf pandoc-crossref


## build main image
FROM --platform=linux/arm64 munepi/tllangjapanese:2021 AS arm64-pandoc-2.18-tl21

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            libnuma1 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

COPY --from=arm64-pandoc-2.18-build  /usr/bin/pandoc /usr/local/bin/pandoc
COPY --from=arm64-pandoc-2.18-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/amd64 munepi/tllangjapanese:2021 AS amd64-pandoc-2.18-tl21

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            liblua5.3-0 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

COPY --from=amd64-pandoc-2.18-build  /usr/local/bin/pandoc          /usr/local/bin/pandoc
COPY --from=amd64-pandoc-2.18-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/arm64 munepi/tllangjapanese:2019 AS arm64-pandoc-2.18-tl19

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            libnuma1 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

COPY --from=arm64-pandoc-2.18-build  /usr/bin/pandoc /usr/local/bin/pandoc
COPY --from=arm64-pandoc-2.18-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/amd64 munepi/tllangjapanese:2019 AS amd64-pandoc-2.18-tl19

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            liblua5.3-0 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

COPY --from=amd64-pandoc-2.18-build  /usr/local/bin/pandoc          /usr/local/bin/pandoc
COPY --from=amd64-pandoc-2.18-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]

# end of file
