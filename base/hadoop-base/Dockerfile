FROM apache/hadoop:3

# Environment variables.
ENV HOME /opt/hadoop
ENV HADOOP_CONF_DIR=$HOME/etc/hadoop
ENV HADOOP_USER_NAME=hadoop

# Adding hadoop configuration files.
COPY config-files $HOME/etc/hadoop

# Notes
# -----
# -> Datanodes connect to Namenode, so Datanodes should know Namenode's hostname and port, which is specified in core-site.xml under the `fs.defaultFS` property.
