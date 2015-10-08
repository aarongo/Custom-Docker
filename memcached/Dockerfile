FROM www.edward-001.com:5000/centos:6.6
MAINTAINER  aaron "aaron.docker@gmail.com"
#Install supervisor
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
#Install libevent
RUN yum -y install gcc gcc-c++ && yum -y install perl-Test-Harness
RUN wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz &&\
        tar xzf libevent-2.0.21-stable.tar.gz &&\
        cd libevent-2.0.21-stable &&\
        ./configure --prefix=/usr &&\
        make && make install
#Install memcached
RUN useradd memcache
RUN wget http://memcached.org/latest && mv latest memcached-1.4.24.tar.gz && \
        tar xzf memcached-1.4.24.tar.gz &&\
        cd memcached-1.4.24 &&\
        ./configure --prefix=/usr/local/memcache --with-libevent=/usr &&\
        make && make install

EXPOSE 22 11211
CMD ["/usr/bin/supervisord"]
