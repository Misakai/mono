FROM debian:wheezy
MAINTAINER Roman Atachiants "roman@misakai.com"

# Environment variables
ENV llvm_version=mono-3.4.0
ENV mono_version=mono-3.12.0.76
WORKDIR /deploy

# Based on: https://github.com/mono/docker
RUN apt-get update \
	&& apt-get install -y curl git autoconf libtool automake build-essential mono-devel gettext python \
	&& rm -rf /var/lib/apt/lists/*

RUN apt-key adv --keyserver pgp.mit.edu --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF


# RUN echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots/$mono_version main" > /etc/apt/sources.list.d/mono-xamarin.list \
#	&& apt-get update \
#	&& apt-get install -y mono-devel ca-certificates-mono fsharp mono-vbnc nuget \
#	&& rm -rf /var/lib/apt/lists/*


# Compile LLVM
RUN git clone git://github.com/mono/llvm.git --branch $llvm_version
WORKDIR /deploy/llvm
RUN ./configure --enable-optimized --enable-targets="x86 x86_64"
RUN make
RUN make install

# back
WORKDIR /deploy

# Compile MONO
RUN git clone git://github.com/mono/mono --branch $mono_version 
RUN cd /deploy/mono
RUN bash ./autogen.sh --enable-llvm=yes
RUN make
RUN make install