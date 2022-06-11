FROM alpine:latest

ENV ENABLE_INIT_DAEMON false
ENV INIT_DAEMON_BASE_URI http://identifier/init-daemon
ENV INIT_DAEMON_STEP spark_master_init

ENV BASE_URL=https://dlcdn.apache.org/spark/
ENV SPARK_VERSION=3.2.1
ENV HADOOP_VERSION=3.2
ENV SPARK_HOME=/spark
ENV HADOOP_CONF_DIR=/hadoop/conf
ENV HADOOP_USER_NAME=hadoop

# Install Spark.
RUN apk add --no-cache curl bash openjdk8-jre python3 py-pip nss libc6-compat coreutils procps \
      && ln -s /lib64/ld-linux-x86-64.so.2 /lib/ld-linux-x86-64.so.2 \
      && wget ${BASE_URL}/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark \
      && rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && cd /

# Make Spark available from any Python shell.
ENV PYTHONPATH="${SPARK_HOME}/python:${PYTHONPATH}"
ENV PYTHONPATH="${SPARK_HOME}/python/lib/py4j-0.10.9.3-src.zip:${PYTHONPATH}"

# Fix the value of PYTHONHASHSEED
# Note: this is needed when you use Python 3.3 or greater
ENV PYTHONHASHSEED 1

# Copy configuration files.
COPY hadoop-conf /hadoop/conf
COPY spark-conf /spark/conf