# Refer to https://github.com/big-data-europe/docker-spark/tree/2.4.1-hadoop2.7/base

FROM federatedai/python:1.5.0-release as source

FROM centos:7 

ENV ENABLE_INIT_DAEMON true
ENV INIT_DAEMON_BASE_URI http://identifier/init-daemon
ENV INIT_DAEMON_STEP spark_master_init

ENV SPARK_VERSION=2.4.1
ENV HADOOP_VERSION=2.7

RUN set -eux && \
    rpm --rebuilddb && \
    rpm --import /etc/pki/rpm-gpg/RPM* && \
    yum -y install gcc gcc-c++ make openssl-devel gmp-devel mpfr-devel libmpc-devel\
    libmpcdevel libaio numactl autoconf automake libtool libffi-devel  \
    snappy snappy-devel zlib zlib-devel bzip2 bzip2-devel lz4-devel libasan lsof sysstat telnet psmisc wget && \
    yum install -y which java-1.8.0-openjdk java-1.8.0-openjdk-devel && \
    yum clean all && \
    wget https://archive.apache.org/dist/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
    tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
    mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark && \
    rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
    cd /

COPY --from=source /data/projects/fate/python /data/projects/fate/python
COPY --from=source /data/projects/python/ /data/projects/python/
ENV PYTHONPATH=$PYTHONPATH:/data/projects/fate/python
ENV PYSPARK_PYTHON=/data/projects/python/venv/bin/python
ENV JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64