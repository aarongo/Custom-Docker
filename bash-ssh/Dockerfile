FROM www.edward-001.com:5000/centos:6.6
MAINTAINER  aaron "aaron.docker@gmail.com"

#Install supervisor
#RUN rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
#RUN yum -y update  && yum -y install python-pip && /usr/bin/pip install distribute supervisor
RUN yum -y install wget tar
RUN wget https://pypi.python.org/packages/source/s/setuptools/setuptools-18.3.1.tar.gz && tar xzf setuptools-18.3.1.tar.gz && cd setuptools-18.3.1 && /usr/bin/python setup.py install
RUN wget https://pypi.python.org/packages/source/p/pip/pip-7.1.2.tar.gz && tar xzf pip-7.1.2.tar.gz && cd pip-7.1.2 && /usr/bin/python setup.py install
RUN /usr/bin/pip install distribute supervisor
RUN mkdir -p /etc/supervisor/conf.d && mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisord.conf
VOLUME /var/log/supervisor

#install sshd
RUN yum install -y openssh-server && sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config
# select root pasword
RUN echo "root:pasword" | chpasswd && echo "root   ALL=(ALL)       ALL" >> /etc/sudoers
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key && ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN mkdir /var/run/sshd

EXPOSE 22
CMD ["/usr/bin/supervisord"]
