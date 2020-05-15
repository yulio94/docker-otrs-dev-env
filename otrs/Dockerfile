FROM ubuntu:latest

LABEL maintainer="Julio Cesar Estrada <j.cesarestrada@hotmail.com>"

ENV TZ=America/Guatemala

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get -y update && \
    apt-get -y install wget

RUN apt-get -y install libapache2-mod-perl2 libdbd-mysql-perl libtimedate-perl libnet-dns-perl libnet-ldap-perl \
    libio-socket-ssl-perl libpdf-api2-perl libdbd-mysql-perl libsoap-lite-perl libtext-csv-xs-perl \
    libjson-xs-perl libapache-dbi-perl libxml-libxml-perl libxml-libxslt-perl libyaml-perl \
    libarchive-zip-perl libcrypt-eksblowfish-perl libencode-hanextra-perl libmail-imapclient-perl \
    libtemplate-perl libdatetime-perl libauthen-ntlm-perl libdigest-md5-perl cpanminus gcc build-essential \
    libdbd-pg-perl

RUN wget https://ftp.otrs.org/pub/otrs/otrs-6.0.28.tar.gz && \
    tar xzf otrs-6.0.28.tar.gz && \
    mv otrs-6.0.28 /opt/otrs

RUN cpanm DateTime::TimeZone \
    Crypt::Random::Source::Weak::devurandom \
    Exporter::Tiny \
    Math::Random::ISAAC \
    Math::Random::Secure \
    Module::Find \
    Moo \
    Type::Tiny \
    namespace::clean

RUN perl /opt/otrs/bin/otrs.CheckModules.pl

RUN useradd -d /opt/otrs -c 'OTRS user' otrs && \
    usermod -G www-data otrs

RUN cp /opt/otrs/Kernel/Config.pm.dist /opt/otrs/Kernel/Config.pm

RUN perl -cw /opt/otrs/bin/cgi-bin/index.pl && \
    perl -cw /opt/otrs/bin/cgi-bin/customer.pl && \
    perl -cw /opt/otrs/bin/otrs.Console.pl

RUN apt-get -y install apache2 libapache2-mod-perl2

RUN ln -s /opt/otrs/scripts/apache2-httpd.include.conf /etc/apache2/sites-enabled/zzz_otrs.conf

RUN a2enmod perl

RUN echo 'ServerName localhost' >> /etc/apache2/apache2.conf

RUN cd /opt/otrs/ && \
    bin/otrs.SetPermissions.pl

EXPOSE 80

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
