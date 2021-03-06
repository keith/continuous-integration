FROM centos:7 as centos7-java8

COPY --from=gcr.io/bazel-public/base:bazelisk /usr/local/bin/bazel /usr/local/bin/bazel
COPY --from=gcr.io/bazel-public/base:buildifier /usr/local/bin/buildifier /usr/local/bin/buildifier
COPY --from=gcr.io/bazel-public/base:saucelabs /usr/local/bin/sc /usr/local/bin/sc

# Install required packages.
COPY google-cloud-sdk.repo /etc/yum.repos.d/google-cloud-sdk.repo

RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    yum install -y \
    bind-utils \
    bzip2-libs.i686 \
    dpkg-dev \
    ed \
    file \
    gcc \
    gcc-c++ \
    git \
    gnupg2 \
    google-cloud-sdk \
    iproute \
    java-1.8.0-openjdk-devel \
    lcov \
    ncurses-libs.i686 \
    openssl-perl \
    patch \
    python \
    python-six \
    python36 \
    python36-PyYAML \
    python36-requests \
    python36-six \
    unzip \
    which \
    zip \
    zlib.i686 \
    && \
    yum clean all

RUN yum install -y yum-utils && \
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
    yum install -y containerd.io docker-ce docker-ce-cli && \
    yum clean all

# Ensure that Bazel can use its beloved ISO-8859-1 locale.
RUN localedef -i en_US -f ISO-8859-1 en_US.ISO-8859-1

FROM centos7-java8 AS centos7-releaser

# dpkg-source needs a newer GNU tar version that supports --sort=name.
RUN pushd /usr/local/src && \
    curl http://ftp.gnu.org/gnu/tar/tar-1.32.tar.bz2 | tar xvj && \
    cd tar-1.32 && \
    FORCE_UNSAFE_CONFIGURE=1 ./configure && \
    make -j && \
    make install && \
    popd && \
    rm -rf /usr/local/src/tar-1.32 && \
    ln -s tar /usr/local/bin/gtar
