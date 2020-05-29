FROM alpine:latest AS builder
LABEL maintainer="Celelibi <celelibi@gmail.com>"
RUN apk upgrade && apk add build-base
RUN apk add abuild python3 python3-dev

COPY abuild.conf /etc/abuild.conf

RUN adduser --disabled-password --gecos ircd irc
RUN addgroup irc abuild
USER irc
WORKDIR /home/irc

# Create a signing key and install it
RUN abuild-keygen --append -n
USER root
RUN cp /home/irc/.abuild/*.pub /etc/apk/keys
USER irc

RUN mkdir -p $HOME/testing/pyyaml \
	$HOME/testing/ircmatch \
	$HOME/testing/passlib \
	$HOME/testing/pylinkirc

COPY pyyaml.APKBUILD /home/irc/testing/pyyaml/APKBUILD
RUN cd testing/pyyaml && abuild checksum && abuild -r

COPY ircmatch.APKBUILD /home/irc/testing/ircmatch/APKBUILD
RUN cd testing/ircmatch && abuild checksum && abuild -r

# Only useful to make pylink-mkpasswd work (optional)
COPY passlib.APKBUILD /home/irc/testing/passlib/APKBUILD
RUN cd testing/passlib && abuild checksum && abuild -r

COPY pylinkirc.APKBUILD /home/irc/testing/pylinkirc/APKBUILD
RUN cd testing/pylinkirc && abuild checksum && abuild -r



FROM alpine:latest
LABEL maintainer="Celelibi <celelibi@gmail.com>"
ARG hostuid=1000

RUN apk upgrade --no-cache
RUN adduser --disabled-password --gecos ircd --uid $hostuid irc
COPY --from=builder /etc/apk/keys/*.pub /etc/apk/keys/
COPY --chown=irc:irc --from=builder /home/irc/packages /home/irc/packages
RUN echo /home/irc/packages/testing/ >> /etc/apk/repositories

# Optional packages:
# - py3-cachetools for servprotect module
# - py3-passlib: used for pylink-mkpasswd to setup password config
# - py3-psutil: used for cron-support module
# - py3-unidecode: used (optionally) by the relay module
RUN apk add --no-cache py3-pylinkirc \
	py3-cachetools \
	py3-passlib \
	py3-psutil \
	py3-unidecode

USER irc
WORKDIR /home/irc
CMD ["/usr/bin/pylink", "conf/pylink.yml"]
