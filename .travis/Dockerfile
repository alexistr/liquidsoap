ARG DOCKER_TAG
FROM savonet/liquidsoap-full:$DOCKER_TAG

MAINTAINER The Savonet Team <savonet-users@lists.sourceforge.net>

# This looks like a bug..
ARG DOCKER_TAG
ARG TRAVIS_BRANCH
ARG TRAVIS_COMMIT
ARG TRAVIS_COMMIT_SHORT
ARG TRAVIS_PULL_REQUEST
ARG TRAVIS_PULL_REQUEST_BRANCH

USER opam

WORKDIR /tmp/liquidsoap-full

RUN eval $(opam config env) && make update && \
     cd liquidsoap && git fetch origin $TRAVIS_COMMIT && git checkout $TRAVIS_COMMIT && \
     cd .. && ./bootstrap && \
     ./configure --prefix=/usr --includedir=\${prefix}/include --mandir=\${prefix}/share/man \
                 --infodir=\${prefix}/share/info --sysconfdir=/etc --localstatedir=/var \
                 --with-camomile-data-dir=/usr/share/liquidsoap/camomile && \
     make clean && make && cd liquidsoap && make doc

USER root

ADD .travis/debian/ /tmp/liquidsoap-full/liquidsoap/debian

ADD .travis/build-deb.sh /tmp/liquidsoap-full/liquidsoap/

RUN chown -R opam /tmp/liquidsoap-full/liquidsoap/debian

USER opam

WORKDIR /tmp/liquidsoap-full/liquidsoap

RUN ./build-deb.sh "$TRAVIS_COMMIT_SHORT" "$TRAVIS_BRANCH" "$TRAVIS_PULL_REQUEST_BRANCH" "$TRAVIS_PULL_REQUEST" "$DOCKER_TAG"

#WORKDIR /tmp/liquidsoap-full/liquidsoap

#RUN eval $(opam config env) && make test