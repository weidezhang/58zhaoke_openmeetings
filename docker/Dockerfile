FROM ubuntu:14.04
MAINTAINER Weide Zhang <weoccc@gmail.com>

#install openmeetings general instructions 
# https://cwiki.apache.org/confluence/display/OPENMEETINGS/Tutorials+for+installing+OpenMeetings+and+Tools?preview=/27838216/52036068/Installation%20OpenMeetings%203.0.x%20on%20Ubuntu%2014.04.pdf
# Keep upstart from complaining
RUN dpkg-divert --local --rename --add /sbin/initctl
RUN ln -sf /bin/true /sbin/initctl

# Let the conatiner know that there is no tty
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get -y upgrade
 
# Basic Requirements
RUN apt-get -y install imagemagick gdebi libgif4 libjpeg62 synaptic zlib1g-dev liboil0.3 unzip make build-essential libgstreamer0.10-0 libsoup2.4-1 libpango1.0-0  libgstreamer-plugins-base0.10-0 liboil0.3 libpangox-1.0-0 libpangoxft-1.0-0 glib-networking  libproxy1 glib-networking-services glib-networking-services   glib-networking-common  gsettings-desktop-schemas mariadb-server vim git git-core openssh-server

RUN mkdir -p /var/run/sshd

# mysql config
#RUN sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf

#RUN easy_install supervisor
#ADD ./start.sh /start.sh
#ADD ./foreground.sh /etc/apache2/foreground.sh
#ADD ./supervisord.conf /etc/supervisord.conf

#install java 8 
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:webupd8team/java
RUN add-apt-repository ppa:libreoffice/ppa
RUN apt-get update 
RUN echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && apt-get install -y oracle-java7-installer
RUN apt-get install -y libreoffice libasound2 oracle-java7-set-default
ADD https://launchpad.net/ella-renaissance/ella-renaissance-beta/beta1/+download/swftools_0.9.1-1_amd64.deb /tmp/swftools_0.9.1-1_amd64.deb
RUN cd /tmp; dpkg -i swftools_0.9.1-1_amd64.deb
RUN mkdir /opt; cd /opt; wget http://jodconverter.googlecode.com/files/jodconverter-core-3.0-beta-4-dist.zip; unzip jodconverter-core-3.0-beta-4-dist.zip 
 
# install sox 14.2.2
ADD http://downloads.sourceforge.net/project/sox/sox/14.4.2/sox-14.4.2.tar.gz?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fsox%2Ffiles%2Fsox%2F14.4.2%2F&ts=1441564169&use_mirror=skylineservers /tmp/sox-14.4.2.tar.gz 
RUN cd /tmp; tar -zxvf sox-14.4.2.tar.gz; cd sox-14.4.2; ./configure && make && make install 

# install ffmpeg 
RUN apt-get -y install autoconf automake libass-dev libfreetype6-dev libgpac-dev libsdl1.2-dev libtheora-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev pkg-config texi2html zlib1g-dev nasm libx264-dev cmake mercurial libopus-dev 
RUN apt-get -y install libmp3lame-dev
ADD http://ffmpeg.org/releases/ffmpeg-0.10.16.tar.gz /tmp/ffmpeg-0.10.16.tar.gz
RUN cd /tmp; tar -zxvf ffmpeg-0.10.16.tar.gz; cd ffmpeg-0.10.16; ./configure --enable-libmp3lame --enable-libx264 --enable-gpl && make && make install 

# install database 
RUN apt-get -y install mariadb-server pwgen 
ADD create_openmeeting.sh /tmp/create_openmeeting.sh 
RUN chmod 777 /tmp/*.sh; sh /tmp/create_openmeeting.sh

# build and deploy openmeetings 
RUN apt-get -y install ant maven subversion 
RUN mkdir /opt/red5306; cd /opt/red5306; git clone https://github.com/weidezhang/openmeetings.git; cd openmeetings;  git checkout personal/wzhang/3.0.x; 
RUN cd /opt/red5306/openmeetings; mvn install:install-file -Dfile=external_jar/jaudiotagger-2.0.4-SNAPSHOT.jar -DgroupId=org -DartifactId=jaudiotagger -Dversion=2.0.4-SNAPSHOT -Dpackaging=jar;
RUN cd /opt/red5306/openmeetings; ant;  
#RUN cd /var/www; tar zxvf moodle-latest.tgz; mv /var/www/moodle /var/www/html
#ADD ./mod/openmeetings /var/www/html/moodle/mod/openmeetings
#ADD ./theme/klass /var/www/html/moodle/theme/klass
#RUN chown -R www-data:www-data /var/www/html/moodle
#RUN mkdir /var/moodledata
#RUN chown -R www-data:www-data /var/moodledata; chmod 777 /var/moodledata
#RUN chmod 755 /start.sh /etc/apache2/foreground.sh

#EXPOSE 22 80
#CMD ["/bin/bash", "/start.sh"]

