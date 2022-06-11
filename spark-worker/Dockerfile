FROM spark-base

# Copy `worker.sh` for init worker node.
COPY worker.sh /

# Envs to comunicate with master node.
ENV SPARK_WORKER_WEBUI_PORT 8081
ENV SPARK_WORKER_LOG /spark/logs
ENV SPARK_MASTER "spark://spark-master:7077"

CMD ["/bin/bash", "/worker.sh"]