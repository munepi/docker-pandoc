## Pandoc 2.14 + pandoc-crossref 0.3.11.0

## build pandoc-crossref
FROM --platform=linux/arm64 munepi/tllangjapanese-base AS arm64-pandoc-2.14-build

ENV ARCH		arm64
ENV PANDOC_VERSION	2.14
ENV PANDOCXREF_VERSION	0.3.11.0

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


FROM --platform=linux/amd64 munepi/tllangjapanese-base AS amd64-pandoc-2.14-build

ENV ARCH		amd64
ENV PANDOC_VERSION	2.14
ENV PANDOCXREF_VERSION	0.3.11.0

RUN wget "https://github.com/jgm/pandoc/releases/download/${PANDOC_VERSION}/pandoc-${PANDOC_VERSION}-1-${ARCH}.deb" && \
    dpkg -i *.deb
RUN wget "https://github.com/lierdakil/pandoc-crossref/releases/download/v${PANDOCXREF_VERSION}/pandoc-crossref-Linux.tar.xz" && \
    tar xf pandoc-crossref-Linux.tar.xz -C /usr/local/bin


## use prebuild image
FROM --platform=linux/arm64 munepi/pandoc:2.14.20230607 AS arm64-pandoc-2.14-prebuild
FROM --platform=linux/amd64 munepi/pandoc:2.14.20230607 AS amd64-pandoc-2.14-prebuild


## build main image
FROM --platform=linux/arm64 munepi/tllangjapanese:2022 AS arm64-pandoc-2.14-tl22

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            libnuma1 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# COPY --from=arm64-pandoc-2.14-build  /usr/bin/pandoc /usr/local/bin/pandoc
# COPY --from=arm64-pandoc-2.14-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
COPY --from=arm64-pandoc-2.14-prebuild  /usr/local/bin/pandoc /usr/local/bin/pandoc
COPY --from=arm64-pandoc-2.14-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/amd64 munepi/tllangjapanese:2022 AS amd64-pandoc-2.14-tl22

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            liblua5.3-0 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# COPY --from=amd64-pandoc-2.14-build  /usr/bin/pandoc          /usr/local/bin/pandoc
# COPY --from=amd64-pandoc-2.14-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
COPY --from=amd64-pandoc-2.14-prebuild  /usr/local/bin/pandoc /usr/local/bin/pandoc
COPY --from=amd64-pandoc-2.14-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/arm64 munepi/tllangjapanese:2021 AS arm64-pandoc-2.14-tl21

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            libnuma1 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# COPY --from=arm64-pandoc-2.14-build  /usr/bin/pandoc /usr/local/bin/pandoc
# COPY --from=arm64-pandoc-2.14-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
COPY --from=arm64-pandoc-2.14-prebuild  /usr/local/bin/pandoc /usr/local/bin/pandoc
COPY --from=arm64-pandoc-2.14-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/amd64 munepi/tllangjapanese:2021 AS amd64-pandoc-2.14-tl21

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            liblua5.3-0 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# COPY --from=amd64-pandoc-2.14-build  /usr/bin/pandoc          /usr/local/bin/pandoc
# COPY --from=amd64-pandoc-2.14-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
COPY --from=amd64-pandoc-2.14-prebuild  /usr/local/bin/pandoc /usr/local/bin/pandoc
COPY --from=amd64-pandoc-2.14-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/arm64 munepi/tllangjapanese:2019 AS arm64-pandoc-2.14-tl19

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            libnuma1 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# COPY --from=arm64-pandoc-2.14-build  /usr/bin/pandoc /usr/local/bin/pandoc
# COPY --from=arm64-pandoc-2.14-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
COPY --from=arm64-pandoc-2.14-prebuild  /usr/local/bin/pandoc /usr/local/bin/pandoc
COPY --from=arm64-pandoc-2.14-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]


FROM --platform=linux/amd64 munepi/tllangjapanese:2019 AS amd64-pandoc-2.14-tl19

## pandoc: install shared libraries
RUN apt-get update && \
        apt-get install -y --no-install-recommends \
            liblua5.3-0 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# COPY --from=amd64-pandoc-2.14-build  /usr/bin/pandoc          /usr/local/bin/pandoc
# COPY --from=amd64-pandoc-2.14-build  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref
COPY --from=amd64-pandoc-2.14-prebuild  /usr/local/bin/pandoc /usr/local/bin/pandoc
COPY --from=amd64-pandoc-2.14-prebuild  /usr/local/bin/pandoc-crossref /usr/local/bin/pandoc-crossref

WORKDIR /data
# ENTRYPOINT ["/bin/bash", "-c"]
# CMD ["pandoc", "--help"]
ENTRYPOINT [ "" ]
CMD [ "/bin/bash" ]

# end of file
