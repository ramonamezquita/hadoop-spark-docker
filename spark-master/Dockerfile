FROM spark-base

ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_WEBUI_PORT 8080
ENV SPARK_MASTER_LOG /spark/logs
ENV SPARK_HOME /spark

# Set up OS for Python requirements.
RUN apk update && \
	apk upgrade && \
	apk add --no-cache --update-cache build-base \
	postgresql-dev \
	g++ \
	linux-headers \
	gfortran  \
	libffi-dev \
	libpng-dev \  
	openblas-dev \ 
	wget \
	freetype-dev \
	python3-dev \
	py3-pip && \
    ln -s /usr/include/locale.h /usr/include/xlocale.h && \
    pip3 install setuptools wheel 

# Set up OS for apache-arrow.
RUN apk update && \ 
 	apk upgrade && \
    apk add --no-cache \
    autoconf \
    bison \
    boost-dev \
    cmake \
    flex \
    libressl-dev \
    zlib-dev


# Download and build apache-arrow from source.
# Reference: https://gist.github.com/bskaggs/fc3c8d0d553be54e2645616236fdc8c6
RUN pip install --no-cache-dir six pytest numpy cython
RUN pip install --no-cache-dir pandas

ARG ARROW_VERSION=3.0.0
ARG ARROW_SHA1=c1fed962cddfab1966a0e03461376ebb28cf17d3
ARG ARROW_BUILD_TYPE=release

ENV ARROW_HOME=/usr/local \
    PARQUET_HOME=/usr/local

RUN mkdir /arrow \
    && wget -q https://github.com/apache/arrow/archive/apache-arrow-${ARROW_VERSION}.tar.gz -O /tmp/apache-arrow.tar.gz \
    && echo "${ARROW_SHA1} *apache-arrow.tar.gz" | sha1sum /tmp/apache-arrow.tar.gz \
    && tar -xvf /tmp/apache-arrow.tar.gz -C /arrow --strip-components 1 \
    && mkdir -p /arrow/cpp/build \
    && cd /arrow/cpp/build \
    && cmake -DCMAKE_BUILD_TYPE=$ARROW_BUILD_TYPE \
        -DOPENSSL_ROOT_DIR=/usr/local/ssl \
        -DCMAKE_INSTALL_LIBDIR=lib \
        -DCMAKE_INSTALL_PREFIX=$ARROW_HOME \
        -DARROW_WITH_BZ2=ON \
        -DARROW_WITH_ZLIB=ON \
        -DARROW_WITH_ZSTD=ON \
        -DARROW_WITH_LZ4=ON \
        -DARROW_WITH_SNAPPY=ON \
        -DARROW_PARQUET=ON \
        -DARROW_PYTHON=ON \
        -DARROW_PLASMA=ON \
        -DARROW_BUILD_TESTS=OFF \
        .. \
    && make -j$(nproc) \
    && make install \
    && cd /arrow/python \
    && python3 setup.py build_ext --build-type=$ARROW_BUILD_TYPE --with-parquet \
    && python3 setup.py install \
    && rm -rf /arrow /tmp/apache-arrow.tar.gz


# Installing Jupyter.
RUN \ 
	python3 -m pip --no-cache-dir install pip -U && \
	python3 -m pip --no-cache-dir install jupyter jupyterlab && \
 	jupyter serverextension enable --py jupyterlab --sys-prefix && \
 	mkdir /notebooks

# Installing requirements.
COPY requirements.txt .
RUN pip3 install -r requirements.txt

# Copy `master.sh` for init master node.
COPY master.sh /

CMD ["/bin/bash", "/master.sh"]