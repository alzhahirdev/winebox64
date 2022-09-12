FROM ubuntu:20.04

LABEL maintainer="alzhahir"

ENV DEBIAN_FRONTEND noninteractive

## add container user
RUN useradd -m -d /home/container container
ENV USER=container HOME=/home/container

## install dependencies
RUN apt update && apt upgrade -y
RUN apt install -y iproute2 gnupg2 tzdata software-properties-common libntlm0 winbind xvfb xauth python3 wget ca-certificates locales apt-transport-https

## add ryanfortner's box64 repo
RUN wget https://ryanfortner.github.io/box64-debs/box64.list -O /etc/apt/sources.list.d/box64.list
RUN wget -O- https://ryanfortner.github.io/box64-debs/KEY.gpg | gpg --dearmor |  tee /usr/share/keyrings/box64-debs-archive-keyring.gpg
RUN apt update

## install dependencies and box64 itself
RUN apt install -y libc6 libstdc++6 box64

## install box86
RUN dpkg --add-architecture armhf
RUN wget https://ryanfortner.github.io/box86-debs/box86.list -O /etc/apt/sources.list.d/box64.list
RUN wget -O- https://ryanfortner.github.io/box86-debs/KEY.gpg | gpg --dearmor |  tee /usr/share/keyrings/box86-debs-archive-keyring.gpg
RUN apt update && apt install -y libc6:armhf box86

## arch-qualify the current repositories
RUN sed -i "s/deb h/deb [arch=arm64] h/g" /etc/apt/sources.list

## add amd64 architecture to prepare for wine
RUN dpkg --add-architecture amd64

## add amd64's repos
RUN echo "# amd64 repositories" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu/ focal main restricted" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu/ focal-updates main restricted" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu/ focal universe" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu/ focal-updates universe" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu/ focal multiverse" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu/ focal-updates multiverse" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://security.ubuntu.com/ubuntu/ focal-security main restricted" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://security.ubuntu.com/ubuntu/ focal-security universe" >> /etc/apt/sources.list
RUN echo "deb [arch=amd64] http://security.ubuntu.com/ubuntu/ focal-security multiverse" >> /etc/apt/sources.list
RUN apt update && apt upgrade -y
#RUN apt-get install libgnutls30:amd64 libldap-2.4-2:amd64 libgpg-error0:amd64 libxml2:amd64 libasound2-plugins:amd64 libsdl2-2.0-0:amd64 libfreetype6:amd64 libdbus-1-3:amd64 libsqlite3-0:amd64 -y

## add x86 architecture to prepare for wine
#RUN dpkg --add-architecture i386

## add i386's repos
#RUN echo "# i386 repositories" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://archive.ubuntu.com/ubuntu/ focal main restricted" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://archive.ubuntu.com/ubuntu/ focal-updates main restricted" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://archive.ubuntu.com/ubuntu/ focal universe" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://archive.ubuntu.com/ubuntu/ focal-updates universe" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://archive.ubuntu.com/ubuntu/ focal multiverse" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://archive.ubuntu.com/ubuntu/ focal-updates multiverse" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://security.ubuntu.com/ubuntu/ focal-security main restricted" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://security.ubuntu.com/ubuntu/ focal-security universe" >> /etc/apt/sources.list
#RUN echo "deb [arch=i386] http://security.ubuntu.com/ubuntu/ focal-security multiverse" >> /etc/apt/sources.list
#RUN apt update && apt upgrade -y

#RUN apt update && apt install -y libstdc++6:amd64

## add winehq repo and install wine
#RUN apt update; exit 0;
#RUN apt-add-repository --remove 'deb https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Ubuntu_20.04_standard ./'
#RUN wget -nc -O /usr/share/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key
#RUN wget -nc -P /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/focal/winehq-focal.sources
#RUN apt update && apt clean && apt upgrade -y; exit 0;
#RUN apt install -y --install-recommends libncurses5:amd64 libncurses6:amd64 wine-stable cabextract xvfb

#Alternative method using openSUSE
#RUN apt-add-repository -r 'deb https://dl.winehq.org/wine-builds/ubuntu/ focal main'
#RUN wget -q https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_20.04/Release.key -O Release.key -O- | apt-key add -
#RUN apt-add-repository 'deb https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_20.04/ ./'
#RUN apt update && apt install --install-recommends wine-devel -y

# Install winehq-stable and with recommends
RUN     wget -nc https://dl.winehq.org/wine-builds/winehq.key \
        && apt-key add winehq.key \
        && echo "deb https://dl.winehq.org/wine-builds/ubuntu/ focal main" >> /etc/apt/sources.list \
        && wget -O- -q download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_20.04/Release.key | apt-key add - \
        && echo "deb http://download.opensuse.org/repositories/Emulators:/Wine:/Debian/xUbuntu_20.04 ./" | tee /etc/apt/sources.list.d/wine-obs.list \
        && apt update \
		&& apt install -y --install-recommends libncurses5:amd64 libncurses6:amd64 wine-devel cabextract xvfb winbind

## setup winetricks
RUN wget -q -O /usr/sbin/winetricks https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks \
        && chmod +x /usr/sbin/winetricks

ENV HOME=/home/container
ENV WINEARCH=win64
ENV WINEPREFIX=/home/container/.wine64
ENV WINEDLLOVERRIDES="mscoree,mshtml="
ENV DISPLAY=:0
ENV DISPLAY_WIDTH=1024
ENV DISPLAY_HEIGHT=768
ENV DISPLAY_DEPTH=16
ENV AUTO_UPDATE=1
ENV XVFB=1


## configure locale
RUN update-locale lang=en_US.UTF-8 && dpkg-reconfigure --frontend noninteractive locales

WORKDIR /home/container
COPY ./entrypoint.sh /entrypoint.sh
CMD ["/bin/bash", "/entrypoint.sh"]
