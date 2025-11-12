FROM proxysql/packaging:build-ubuntu24-v3.0.3

LABEL authors="Miro Stauder <miro@proxysql.com>"

# set git safe dir
RUN git config --system --add safe.directory /opt/

# install all dependencies first before breaking apt with mysql 5.7
RUN apt-get -y update && \
	apt-get -y install libboost-all-dev ruby ruby-dev ruby-ffi ruby-dotenv

# fpm packaging
RUN gem install fpm

# mysql 5.7 libs - installed last because it breaks apt
RUN apt-get -y purge mysql-client libmariadbclient-dev libmariadb-dev libmysqlclient-dev && \
	wget -q https://repo.mysql.com/apt/ubuntu/pool/mysql-5.7/m/mysql-community/libmysqlclient20_5.7.38-1ubuntu18.04_amd64.deb && \
	dpkg -i libmysqlclient20_5.7.38-1ubuntu18.04_amd64.deb && \
	wget -q https://repo.mysql.com/apt/ubuntu/pool/mysql-5.7/m/mysql-community/libmysqlclient-dev_5.7.38-1ubuntu18.04_amd64.deb && \
	dpkg --force-all -i libmysqlclient-dev_5.7.38-1ubuntu18.04_amd64.deb && \
	wget -q -O /usr/include/mysql/hash.h https://raw.githubusercontent.com/mysql/mysql-server/5.7/include/hash.h

# clean apt cache
RUN apt clean && \
	rm -rf /var/cache/apt/* && \
	rm -rf /var/lib/apt/lists/*
