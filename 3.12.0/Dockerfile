FROM debian:wheezy
MAINTAINER Roman Atachiants "roman@misakai.com"

# Environment variables
ENV LLVM_VERSION=mono-3.4.0
ENV MONO_VERSION=mono-3.12.0.76
ENV DEPENDENCIES="curl git autoconf libtool automake build-essential mono-devel gettext python"
WORKDIR /deploy

RUN apt-get update \
	&& apt-get install -y $DEPENDENCIES \
	&& rm -rf /var/lib/apt/lists/* \
	&& apt-key adv --keyserver pgp.mit.edu --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF \
	&& git clone git://github.com/mono/llvm.git --branch $LLVM_VERSION \
	&& cd /deploy/llvm \
	&& ./configure --enable-optimized --enable-targets="x86 x86_64" \
	&& make \
	&& make install \
	&& cd /deploy \
	&& git clone git://github.com/mono/mono --branch $MONO_VERSION  \
	&& cd /deploy/mono \
	&& bash ./autogen.sh --enable-llvm=yes \
	&& make \
	&& make install \
	&& apt-get remove -y --purge $DEPENDENCIES \
	&& rm -rf /deploy \
	&& mkdir /app

WORKDIR /app