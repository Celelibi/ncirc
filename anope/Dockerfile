FROM alpine:latest AS builder
LABEL maintainer="Celelibi <celelibi@gmail.com>"
RUN apk upgrade && apk add build-base openssl openssl-dev
RUN apk add cmake gettext-dev perl sqlite-dev

RUN adduser --disabled-password --gecos ircd irc
USER irc
WORKDIR /home/irc
RUN ANOPEVER=2.0.7 \
	&& ANOPEDIR=anope-$ANOPEVER-source \
	&& ANOPESRC=$ANOPEDIR.tar.gz \
	&& ANOPESRC_URL=https://github.com/anope/anope/releases/download/$ANOPEVER/$ANOPESRC \
	&& wget $ANOPESRC_URL \
	&& tar -xzf $ANOPESRC \
	&& cd $ANOPEDIR \
	&& echo 'INSTDIR="/home/irc/services"' > config.cache \
	&& echo -e "m_ssl_openssl.cpp\nm_sqlite.cpp\nstats\nq" | ./extras \
	&& yes "" | ./Config --nointro --quick \
	&& cd build \
	&& make -j $(grep -c ^processor /proc/cpuinfo) \
	&& make install



FROM alpine:latest
LABEL maintainer="Celelibi <celelibi@gmail.com>"
ARG hostuid=1000

RUN apk upgrade --no-cache
# openssl is only used for sendmail. See config/anope/services.conf:978
RUN apk add --no-cache libgcc libstdc++ gettext sqlite-libs openssl

RUN adduser --disabled-password --gecos ircd --uid $hostuid irc
USER irc
COPY --chown=irc:irc --from=builder /home/irc/services /home/irc/services
WORKDIR /home/irc/services
CMD ["/home/irc/services/bin/services", "--nofork"]
