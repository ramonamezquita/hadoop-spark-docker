FROM spark-base

ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_WEBUI_PORT 8080
ENV SPARK_MASTER_LOG /spark/logs
ENV SPARK_HOME /spark

# Set up OS for Python requirements.
RUN \ 
	apk add --no-cache --update-cache  postgresql-dev g++ linux-headers gfortran build-base bash libffi-dev && \
	apk add --no-cache --update-cache libpng-dev openblas-dev wget freetype-dev python3-dev py3-pip && \
    ln -s /usr/include/locale.h /usr/include/xlocale.h && \
    pip3 install setuptools wheel 

# Installing Jupyter.
RUN \ 
	python3 -m pip --no-cache-dir install pip -U && \
	python3 -m pip --no-cache-dir install jupyter jupyterlab && \
 	jupyter serverextension enable --py jupyterlab --sys-prefix && \
 	mkdir /notebooks

# Installing requirements.
#COPY requirements.txt .
# RUN pip3 install -r requirements.txt

# Copy `master.sh` for init master node.
COPY master.sh /

CMD ["/bin/bash", "/master.sh"]