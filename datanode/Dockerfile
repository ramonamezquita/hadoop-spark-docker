FROM namenode:latest

# Add config files.
ADD config-files/hdfs-site.xml $HOME/etc/hadoop/

# Create directories for datanode storage.
RUN mkdir /opt/hdfs/datanode

CMD hdfs datanode