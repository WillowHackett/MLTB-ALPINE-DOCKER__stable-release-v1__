FROM alpine:edge


# LABEL

LABEL org.opencontainers.image.source="https://github.com/amirulandalib/mltb-alpine"
LABEL org.opencontainers.image.description="Docker for MLTB by Anasty based on Alpine EDGE docker image"


ARG TARGETPLATFORM BUILDPLATFORM

# Setup Working Directory

WORKDIR /usr/src/app
RUN chmod 777 /usr/src/app && chmod +x /usr/src/app



# Installing required packages

RUN echo -e "\e[32m[INFO]: Installing required packages.\e[0m" && \
    apk update && apk upgrade && \
    apk add --upgrade --no-cache \
    musl-dev qbittorrent python3 python3-dev busybox \
    qbittorrent-nox py3-pip py3-lxml aria2 p7zip xz curl pv jq ffmpeg \
    musl-locales neofetch git make g++ gcc automake zip unzip autoconf \
    libtool curl-dev libsodium-dev mediainfo speedtest-cli \
    c-ares-dev sqlite-dev freeimage-dev swig boost-dev \
    libpthread-stubs zlib-dev libpq-dev libffi-dev py3-virtualenv libffi \
    bash alpine-sdk clang clang-dev dpkg cmake ccache icu-data-full gettext gettext-dev gawk \
    && apk add --upgrade --no-cache crypto++-dev crypto++ --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing && \
    pip3 install wheel


SHELL ["/bin/bash", "-c"]

        
# Building and Installing MegaSdkC++

ENV PYTHONWARNINGS=ignore
RUN echo -e "\e[32m[INFO]: Building and Installing MegaSdkC++.\e[0m" && \
    git clone https://github.com/meganz/sdk.git ~/home/sdk \
    && cd ~/home/sdk && rm -rf .git \
    && autoupdate -fIv && ./autogen.sh \
    && ./configure CFLAGS='-fpermissive' CXXFLAGS='-fpermissive' CPPFLAGS='-fpermissive' CCFLAGS='-fpermissive' \
    --disable-silent-rules --enable-python --with-sodium --disable-examples --with-python3 \
    && make -j$(nproc --all) \
    && cd bindings/python/ && python3 setup.py bdist_wheel \
    && cd dist && ls && pip3 install --no-cache-dir *.whl  

# Running Final Apk Update

RUN echo -e "\e[32m[INFO]: Running Final Apk Update.\e[0m" && \
    apk update && apk upgrade


# Setup Language Environments
ENV LANG="en_US.UTF-8" LANGUAGE="en_US:en" LC_ALL="en_US.UTF-8"
