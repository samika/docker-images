FROM debian:buster

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    libev-dev  libglib2.0-dev libjson-c-dev libcurl4-openssl-dev git build-essential automake autoconf

RUN mkdir /build
WORKDIR /build
RUN git clone https://github.com/taviso/nntpit.git . && \
    autoreconf --install && \
    autoconf -i && \
    automake --add-missing && \
    ./configure && \
    make 


FROM debian:buster-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get upgrade -y && apt-get install -y libcurl4 libjson-c3 libglib2.0 libev-dev  && apt-get clean all && adduser user --uid 1000 --system
COPY --from=0 /build/nntpit /usr/local/bin/nntpit

USER 1000
EXPOSE 8119

ENTRYPOINT ["/usr/local/bin/nntpit", "-D", "-l", "0.0.0.0", "-p", "8119"]
