#####################################################
# Build an image for compiling safe_vault for ARMV7 #
#####################################################


# Set the base image to Debian
FROM debian:jessie


# Set this to avoid warning messages.
ENV DEBIAN_FRONTEND noninteractive


# Create a working folder:

RUN mkdir /projects


# Install the GCC toolchain and various other tools:

RUN apt-get update && apt-get -y upgrade && apt-get -y install \
	autoconf \
	build-essential \
	curl \
	git \
	libtool \
	pkg-config \
	tar \
	&& apt-get clean \
	&& apt-get autoremove \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# Setup the C cross-compile software environment:

# Add the emdebian repo:
RUN printf "\n\ndeb http://emdebian.org/tools/debian/ jessie main\n" >> /etc/apt/sources.list
RUN curl http://emdebian.org/tools/debian/emdebian-toolchain-archive.key | apt-key add -

# Enable the foreign architecture:
RUN dpkg --add-architecture armhf && apt-get -y update

# Install crossbuild toolchain:
RUN apt-get -y install crossbuild-essential-armhf


# Install the Sodium system library (libsodium):

ENV PKG_CONFIG_ALLOW_CROSS 1

RUN cd /projects \
	&& git clone https://github.com/jedisct1/libsodium.git \
	&& cd libsodium \
	&& ./autogen.sh \
	&& ./configure --enable-static LDFLAGS=-static --host=arm-linux-gnueabihf \
	&& make && make install \
	&& cd /projects \
	&& rm -rf libsodium


# Set up the Rust compile environment:

ENV PATH /root/.cargo/bin:$PATH

RUN cd /projects \
	&& curl https://sh.rustup.rs -o rustup.sh \
	&& chmod +x /projects/rustup.sh \
	&& /projects/rustup.sh -y \
	&& rustup target add armv7-unknown-linux-gnueabihf

# To tell cargo to use the correct linker for this target, create a .cargo/config file with the appropriate entry:
RUN printf '[target.armv7-unknown-linux-gnueabihf]\nlinker = "/usr/bin/arm-linux-gnueabihf-gcc"\n' > /root/.cargo/config


# Create the compile script:

RUN printf '#! /bin/sh\n\nexport PKG_CONFIG_ALLOW_CROSS=1\ncd /projects\nrm -rf safe_vault >/dev/null\ngit clone http://github.com/maidsafe/safe_vault\ncd safe_vault\ncargo build --release --target=armv7-unknown-linux-gnueabihf\ntouch /done123\n' > /compile

RUN chmod +x /compile


# Testing only: Build safe_vault and copy the safe_vault binary to root dir:

#RUN /compile \
#	&& cp /projects/safe_vault/target/armv7-unknown-linux-gnueabihf/release/safe_vault / \
#	&& rm -rf /projects/safe_vault \
#	&& rm -rf /root/.cargo/registry/*


# Remove compile-finished-flag file:

RUN rm -f /done123

