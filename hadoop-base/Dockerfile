FROM apache/hadoop:3

# Environment variables.
ENV HOME /opt/hadoop
ENV HADOOP_CONF_DIR=$HOME/etc/hadoop
ENV HADOOP_USER_NAME=hadoop

# Adding hadoop configuration files.
COPY config-files $HOME/etc/hadoop

# Notes
# -----
# -> datanodes connect to namenode and for that they should know namenode's hostname and port. This is specified in core-site.xml.
# -> By default I am using hostname 'namenode' and port '8020'. If you change this in core-site.xml then all
#    datanodes would be trying to connect to your changed hostname and port. 
#    Therefore, be sure to change your namenode's to hostname to whatever you specified in core-site.xml
#    since by default docker assigns random hostnames to containers.
