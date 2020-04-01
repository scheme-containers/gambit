# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential ca-certificates git \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
RUN git clone https://github.com/gambit/gambit.git --depth 1
WORKDIR /build/gambit
RUN ./configure --prefix=/usr/local --enable-single-host
RUN rm -rf boot gsc-boot
RUN make
RUN make check
RUN make modules
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      gcc libc-dev \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
RUN ln -s gsi /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
