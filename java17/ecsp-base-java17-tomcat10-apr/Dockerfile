# Copyright (c) 2023-24 Harman International.
# SPDX-License-Identifier: Apache-2.0

FROM azul/zulu-openjdk-alpine:17.0.9-17.46.19-jre-headless AS BUILDER
LABEL authors="https://accounts.eclipse.org/mailing-list/ecsp-dev"

# Env vars
ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME
ENV OPENSSL_DIR /etc/ssl

# let "Tomcat Native" live somewhere isolated
ENV TOMCAT_NATIVE_LIBDIR $CATALINA_HOME/native-jni-lib
ENV LD_LIBRARY_PATH ${LD_LIBRARY_PATH:+$LD_LIBRARY_PATH:}$TOMCAT_NATIVE_LIBDIR

ENV TOMCAT_MAJOR 10
ENV TOMCAT_VERSION 10.1.40
ENV TOMCAT_TGZ_URL http://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz

RUN apk add --update file
#run scrip for tomcat extraction and installation
RUN set -x \
	\
	&& apk add --no-cache --virtual .fetch-deps \
		ca-certificates \
		tar \
		openssl \
	&& wget -O tomcat.tar.gz "$TOMCAT_TGZ_URL" \
	&& tar -xvf tomcat.tar.gz --strip-components=1 \
	&& rm bin/*.bat \
	&& rm tomcat.tar.gz* \
	\
	&& nativeBuildDir="$(mktemp -d)" \
	&& tar -xvf bin/tomcat-native.tar.gz -C "$nativeBuildDir" --strip-components=1 \
	&& apk add --no-cache --virtual .native-build-deps \
		apr-dev \
		gcc \
		libc-dev \
		make \
		openssl-dev \
	&& ( \
		export CATALINA_HOME="$PWD" \
		&& cd "$nativeBuildDir/native" \
		&& ./configure \
			--libdir="$TOMCAT_NATIVE_LIBDIR" \
			--prefix="$CATALINA_HOME" \
			--with-apr="$(which apr-1-config)" \
			--with-java-home="$JAVA_HOME" \
            --with-ssl="$OPENSSL_DIR" \
		&& make -j$(getconf _NPROCESSORS_ONLN) \
		&& make install \
	) \
	&& runDeps="$( \
		scanelf --needed --nobanner --recursive "$TOMCAT_NATIVE_LIBDIR" \
			| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
			| sort -u \
			| xargs -r apk info --installed \
			| sort -u \
	)" \
	&& apk add --virtual .tomcat-native-rundeps $runDeps \
	&& apk del .fetch-deps .native-build-deps \
	&& rm -rf "$nativeBuildDir" \
	&& rm bin/tomcat-native.tar.gz

# Minimal Tomcat: remove all default webapps
RUN rm -rf \
  ${CATALINA_HOME}/webapps/*

FROM azul/zulu-openjdk-alpine:17.0.9-17.46.19-jre-headless
RUN mkdir /usr/local/tomcat
COPY --from=builder usr/local/tomcat /usr/local/tomcat
# check tomcat version and verify proper installation
RUN /usr/local/tomcat/bin/catalina.sh version
EXPOSE 8080
