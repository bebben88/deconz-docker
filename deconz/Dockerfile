FROM arm32v7/debian:jessie-slim

ENV SW_VERSION 2.04.99
ENV DEBIAN_FRONTEND noninteractive

RUN mkdir -p /usr/src/deconz
RUN echo 'deb http://archive.raspberrypi.org/debian/ jessie main' > /etc/apt/sources.list.d/raspi.list
RUN apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv 82B129927FA3303E
RUN apt-get update
RUN apt-get install -y --no-install-recommends vim curl sudo git
RUN curl -O http://www.dresden-elektronik.de/rpi/deconz/beta/deconz-${SW_VERSION}-qt5.deb
RUN (dpkg -i deconz-${SW_VERSION}-qt5.deb || true)
RUN rm deconz-${SW_VERSION}-qt5.deb
RUN apt-get update
RUN apt-get install -y -f --no-install-recommends
RUN curl -O http://www.dresden-elektronik.de/rpi/deconz-dev/deconz-dev-${SW_VERSION}.deb
RUN (dpkg -i deconz-dev-${SW_VERSION}.deb || true)
RUN apt-get update
RUN apt-get install -y -f --no-install-recommends
RUN git config --global http.sslVerify false
RUN apt-get install build-essential -y
RUN apt-get install make -y
WORKDIR /usr/src/deconz
RUN git clone https://github.com/dresden-elektronik/deconz-rest-plugin.git
WORKDIR /usr/src/deconz/deconz-rest-plugin

#Change version also under here!!

RUN git checkout -b mybranch V2_04_99
RUN qmake && make -j2
RUN cp ../libde_rest_plugin.so /usr/share/deCONZ/plugins

RUN chown root:root /usr/bin/deCONZ*
CMD /usr/bin/deCONZ --auto-connect=1 -platform minimal --http-port=80 --dbg-error=1
