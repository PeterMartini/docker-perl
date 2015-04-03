FROM buildpack-deps
MAINTAINER Peter Martini <PeterCMartini@GMail.com>

RUN apt-get update \
    && apt-get install -y curl procps \
    && rm -fr /var/lib/apt/lists/*

RUN mkdir /usr/src/perl
WORKDIR /usr/src/perl

RUN curl -SL https://cpan.metacpan.org/authors/id/D/DO/DOM/perl-5.12.5.tar.bz2 -o perl-5.12.5.tar.bz2 \
    && echo '68ff2f4a0901d1e4ea263f452001db0563d2edb3 *perl-5.12.5.tar.bz2' | sha1sum -c - \
    && tar --strip-components=1 -xjf perl-5.12.5.tar.bz2 -C /usr/src/perl \
    && rm perl-5.12.5.tar.bz2 \
    && ./Configure -Dusethreads -Duse64bitall  -des \
    && make -j$(nproc) \
    && TEST_JOBS=$(nproc) make test_harness \
    && make install \
    && cd /usr/src \
    && curl -LO https://raw.githubusercontent.com/miyagawa/cpanminus/master/cpanm \
    && chmod +x cpanm \
    && ./cpanm App::cpanminus \
    && rm -fr ./cpanm /root/.cpanm /usr/src/perl

WORKDIR /root

CMD ["perl5.12.5","-de0"]
