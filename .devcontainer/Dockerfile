FROM ubuntu:22.04@sha256:1aa979d85661c488ce030ac292876cf6ed04535d3a237e49f61542d8e5de5ae0 AS base

ARG UID=1000
ARG GID=1000
ARG USERNAME="ubuntu"
ENV DEBIAN_FRONTEND=noninteractive
SHELL ["/bin/bash", "-c"]

RUN apt-get -y update && \
	apt-get --no-install-recommends -y install \
    build-essential chrpath cpio debianutils diffstat \
    file gawk gcc git iputils-ping libacl1 liblz4-tool \
    locales python3 python3-git python3-jinja2 python3-pexpect \
    python3-pip python3-subunit socat texinfo unzip wget \
    xz-utils zstd screen usbutils \
		&& \
	apt-get clean -y && \
	apt-get autoremove --purge -y && \
	rm -rf /var/lib/apt/lists/*


FROM base AS rkdev
RUN apt-get -y update && \
	apt-get --no-install-recommends -y install \
    libudev-dev libusb-1.0-0-dev dh-autoreconf pkg-config libusb-1.0 \
		&& \
	apt-get clean -y && \
	apt-get autoremove --purge -y && \
	rm -rf /var/lib/apt/lists/*

ADD https://github.com/rockchip-linux/rkdeveloptool/archive/refs/heads/master.zip rkdeveloptool.zip
RUN unzip rkdeveloptool.zip
WORKDIR /rkdeveloptool-master
RUN ./autogen.sh && ./configure && make -j


FROM base
# Initialise system locale
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8
RUN locale-gen $LANG

RUN groupadd -r ${USERNAME} && useradd -m -s /bin/bash -r -g ${USERNAME} ${USERNAME}
ADD --chmod=a+x https://storage.googleapis.com/git-repo-downloads/repo /bin/repo
COPY --chmod=a+x --from=rkdev /rkdeveloptool-master/rkdeveloptool /bin/rkdeveloptool
USER ${USERNAME}
WORKDIR /home/${USERNAME}/workspace
