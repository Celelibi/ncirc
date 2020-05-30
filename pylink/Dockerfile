FROM alpine:latest AS builder
LABEL maintainer="Celelibi <celelibi@gmail.com>"
RUN apk upgrade && apk add build-base
RUN apk add abuild python3 python3-dev git

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

RUN mkdir -p $HOME/testing/pyyaml
COPY pyyaml.APKBUILD /home/irc/testing/pyyaml/APKBUILD
RUN cd testing/pyyaml && abuild checksum && abuild -r

RUN mkdir -p $HOME/testing/ircmatch
COPY ircmatch.APKBUILD /home/irc/testing/ircmatch/APKBUILD
RUN cd testing/ircmatch && abuild checksum && abuild -r

# Only useful to make pylink-mkpasswd work (optional)
RUN mkdir -p $HOME/testing/passlib
COPY passlib.APKBUILD /home/irc/testing/passlib/APKBUILD
RUN cd testing/passlib && abuild checksum && abuild -r

RUN mkdir -p $HOME/testing/pylinkirc
COPY pylinkirc.APKBUILD /home/irc/testing/pylinkirc/APKBUILD
RUN cd testing/pylinkirc && abuild checksum && abuild -r

# Required by pylink-discord
RUN mkdir -p $HOME/testing/holster
COPY holster.APKBUILD /home/irc/testing/holster/APKBUILD
RUN cd testing/holster && abuild checksum && abuild -r

# Required by pylink-discord
RUN mkdir -p $HOME/testing/disco-py
COPY disco-py.APKBUILD /home/irc/testing/disco-py/APKBUILD
RUN cd testing/disco-py && abuild snapshot && abuild checksum && abuild -r

# Optionally used by pylink-discord
RUN mkdir -p $HOME/testing/libgravatar
COPY libgravatar.APKBUILD /home/irc/testing/libgravatar/APKBUILD
RUN cd testing/libgravatar && abuild checksum && abuild -r

RUN mkdir -p $HOME/testing/pylink-discord
COPY pylink-discord.APKBUILD /home/irc/testing/pylink-discord/APKBUILD
RUN cd testing/pylink-discord && abuild checksum && abuild -r



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
# - py3-libgravatar: used (optionally) by pylink-discord
RUN apk add --no-cache py3-pylink-discord \
	py3-cachetools \
	py3-passlib \
	py3-psutil \
	py3-unidecode \
	py3-libgravatar


USER irc
WORKDIR /home/irc
CMD ["/usr/bin/pylink-discord", "conf/pylink.yml"]
