FROM lcr.loongnix.cn/library/debian:sid
MAINTAINER znley<shanjiantao@loongson.cn>

WORKDIR /usr/src/perl

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       bzip2 \
       ca-certificates \
       # cpio \
       curl \
       dpkg-dev \
       # file \
       gcc \
       # g++ \
       # libbz2-dev \
       # libdb-dev \
       libc6-dev \
       # libgdbm-dev \
       # liblzma-dev \
       make \
       netbase \
       # procps \
       zlib1g-dev \
       xz-utils \
       libssl-dev \
    && curl -fL https://github.com/Loongson-Cloud-Community/docker-perl/releases/download/perl-5.36.0/perl-5.36.0.tar.xz -o perl-5.36.0.tar.xz \
    && echo '0f386dccbee8e26286404b2cca144e1005be65477979beb9b1ba272d4819bcf0 *perl-5.36.0.tar.xz' | sha256sum --strict --check - \
    && tar --strip-components=1 -xaf perl-5.36.0.tar.xz -C /usr/src/perl \
    && rm perl-5.36.0.tar.xz \
    && gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
    && archBits="$(dpkg-architecture --query DEB_BUILD_ARCH_BITS)" \
    && archFlag="$([ "$archBits" = '64' ] && echo '-Duse64bitall' || echo '-Duse64bitint')" \
    && ./Configure -Darchname="$gnuArch" "$archFlag" -Duseshrplib -Dvendorprefix=/usr/local  -des \
    && make -j$(nproc) \
    && TEST_JOBS=$(nproc) make test_harness \
    && make install \
    && cd /usr/src \
    && curl -fLO https://github.com/Loongson-Cloud-Community/docker-perl/releases/download/perl-5.36.0/App-cpanminus-1.7046.tar.gz \
    && echo '3e8c9d9b44a7348f9acc917163dbfc15bd5ea72501492cea3a35b346440ff862 *App-cpanminus-1.7046.tar.gz' | sha256sum --strict --check - \
    && tar -xzf App-cpanminus-1.7046.tar.gz && cd App-cpanminus-1.7046 && perl bin/cpanm . && cd /root \
    && curl -fL https://github.com/Loongson-Cloud-Community/docker-perl/releases/download/perl-5.36.0/cpm -o /usr/local/bin/cpm \
    # sha256 checksum is from docker-perl team, cf https://github.com/docker-library/official-images/pull/12612#issuecomment-1158288299
    && echo '7dee2176a450a8be3a6b9b91dac603a0c3a7e807042626d3fe6c93d843f75610 */usr/local/bin/cpm' | sha256sum --strict --check - \
    && chmod +x /usr/local/bin/cpm \
    && savedPackages="ca-certificates make netbase zlib1g-dev libssl-dev" \
    && apt-mark auto '.*' > /dev/null \
    && apt-mark manual $savedPackages \
    && rm -fr /var/cache/apt/* /var/lib/apt/lists/* \
    && rm -fr /root/.cpanm /usr/src/perl /usr/src/App-cpanminus-1.7046* /tmp/* \
    && cpanm --version && cpm --version

WORKDIR /

CMD ["perl5.36.0","-de0"]
