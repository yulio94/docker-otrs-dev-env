FROM centos:7

LABEL maintainer="Julio Cesar Estrada <j.cesarestrada@hotmail.com>"

RUN yum -y update && yum -y install wget && yum install -y epel-release

RUN wget https://ftp.otrs.org/pub/otrs/RPMS/rhel/7/otrs-6.0.28-01.noarch.rpm

RUN yum -y install --nogpgcheck  otrs-6.0.28-01.noarch.rpm

RUN yum -y install "perl(YAML::XS)" && yum -y install "perl(DBD::mysql)" && yum clean all

RUN /opt/otrs/bin/otrs.CheckModules.pl

EXPOSE  80

CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]

#RUN /opt/otrs/bin/otrs.Daemon.pl start

#RUN /opt/otrs/bin/Cron.sh start

