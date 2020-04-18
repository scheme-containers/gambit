# -*- mode: dockerfile; coding: utf-8 -*-

# NOTE: A shallow clone (git clone --depth 1) causes an obscure build
# failure for unknown reasons. That's why we use a full clone.

FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential ghostscript texinfo ca-certificates git \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
RUN git clone https://github.com/gambit/gambit.git
WORKDIR /build/gambit
RUN ./configure --prefix=/usr/local \
      --enable-single-host \
      --enable-multiple-threaded-vms
RUN make
RUN make modules
RUN make check
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      gcc libc-dev \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
RUN ln -s gsi /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
