FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04
MAINTAINER jeffcarl67

RUN apt-get update
RUN apt-get install -y python3 python3-dev python3-setuptools gcc \
	libtinfo-dev zlib1g-dev build-essential cmake libedit-dev libxml2-dev \
	git python3-pip
RUN apt-get install -y llvm-4.0-dev
RUN pip3 install numpy decorator attrs

ARG USER_ID
ARG GROUP_ID

RUN if [ ${USER_ID:-0} -ne 0 ] && [ ${GROUP_ID:-0} -ne 0 ]; then \
	groupadd --gid ${GROUP_ID} tvm && \
	useradd --create-home --shell /bin/bash --uid ${USER_ID} --gid ${GROUP_ID} tvm; \
fi

USER tvm

WORKDIR /home/tvm

RUN git clone --recursive https://github.com/apache/incubator-tvm tvm

RUN cd tvm && git checkout 0d0d515c1b25f16dc3c6d4ab0277ef9173b1d992 && \
	echo set\(USE_LLVM llvm-config-4.0\) >> config.cmake && \
	echo set\(USE_CUDA ON\) >> config.cmake && \
	mkdir -p build && cd build && cmake .. && make -j4

RUN cd tvm/python && python3 setup.py install --user

RUN cd tvm/nnvm/python && python3 setup.py install --user

RUN cd tvm/topi/python && python3 setup.py install --user

CMD ["bash"]