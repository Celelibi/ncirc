FROM alpine:latest AS builder
LABEL maintainer="Celelibi <celelibi@gmail.com>"
RUN apk upgrade && apk add build-base
RUN apk add openssl openssl-dev
RUN apk add gnupg

RUN adduser --disabled-password --gecos ircd irc
USER irc
WORKDIR /home/irc
RUN UNREALIRCDDIR=unrealircd-5.0.5.1 \
	&& UNREALIRCDSRC=$UNREALIRCDDIR.tar.gz \
	&& UNREALIRCDSRC_URL=https://www.unrealircd.org/downloads/$UNREALIRCDSRC \
	&& wget $UNREALIRCDSRC_URL $UNREALIRCDSRC_URL.asc \
	&& gpg --keyserver keys.gnupg.net --recv-keys 0xA7A21B0A108FF4A9 \
	&& gpg --verify $UNREALIRCDSRC.asc $UNREALIRCDSRC \
	&& tar -xzf $UNREALIRCDSRC \
	&& cd $UNREALIRCDDIR \
	&& echo "BASEPATH=\"$HOME/unrealircd\"" > config.settings \
	&& echo "GENCERTIFICATE=\"0\"" >> config.settings \
	&& ./Config --nointro --quick \
	&& make -j $(grep -c ^processor /proc/cpuinfo) \
	&& make install



FROM alpine:latest
LABEL maintainer="Celelibi <celelibi@gmail.com>"
ARG hostuid=1000

RUN apk upgrade --no-cache
RUN adduser --disabled-password --gecos ircd --uid $hostuid irc
USER irc
COPY --chown=irc:irc --from=builder /home/irc/unrealircd /home/irc/unrealircd
WORKDIR /home/irc/unrealircd
CMD ["/home/irc/unrealircd/bin/unrealircd", "-F"]
EXPOSE 6667 6697
