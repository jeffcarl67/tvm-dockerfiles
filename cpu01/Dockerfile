FROM ubuntu:18.04
MAINTAINER jeffcarl67

RUN apt-get update
RUN apt-get install -y python3 python3-dev python3-setuptools gcc \
	libtinfo-dev zlib1g-dev build-essential cmake libedit-dev libxml2-dev \
	git python3-pip
RUN apt-get install -y llvm-4.0-dev
RUN pip3 install numpy decorator attrs

RUN cd / && git clone --recursive https://github.com/apache/incubator-tvm tvm

RUN cd /tvm && git checkout 0d0d515c1b25f16dc3c6d4ab0277ef9173b1d992 && \
	echo set\(USE_LLVM llvm-config-4.0\) >> config.cmake && \
	mkdir -p build && cd build && cmake .. && make -j4

RUN cd /tvm/python && python3 setup.py install

RUN cd /tvm/nnvm/python && python3 setup.py install

RUN cd /tvm/topi/python && python3 setup.py install

CMD ["bash"]