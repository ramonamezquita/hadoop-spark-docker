FROM hadoop-base:latest

# Add config files.
ADD config-files/hdfs-site.xml $HOME/etc/hadoop/

# Create directories for namenode storage.
RUN mkdir /opt/hdfs
RUN mkdir /opt/hdfs/namenode

# Necessary before running namenode
RUN hdfs namenode -format

CMD hdfs namenode
