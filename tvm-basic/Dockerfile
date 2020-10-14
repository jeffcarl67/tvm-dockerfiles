FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04
MAINTAINER jeffcarl67

RUN apt-get update
RUN apt-get install -y python3 python3-dev python3-setuptools gcc \
	libtinfo-dev zlib1g-dev build-essential cmake libedit-dev libxml2-dev \
	git python3-pip wget clinfo ocl-icd-libopencl1 opencl-headers ocl-icd-opencl-dev

RUN wget https://cmake.org/files/v3.16/cmake-3.16.7-Linux-x86_64.tar.gz && \
	tar zxvf cmake-3.16.7-Linux-x86_64.tar.gz && \
	mv cmake-3.16.7-Linux-x86_64 /opt/cmake-3.16.7 && \
	ln -sf /opt/cmake-3.16.7/bin/* /usr/bin/

RUN apt-get install -y llvm-4.0-dev
RUN pip3 install numpy decorator attrs
RUN pip3 install tornado psutil xgboost

RUN mkdir -p /etc/OpenCL/vendors && \
    echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility

ARG USER_ID
ARG GROUP_ID

RUN if [ ${USER_ID:-0} -ne 0 ] && [ ${GROUP_ID:-0} -ne 0 ]; then \
	groupadd --gid ${GROUP_ID} tvm && \
	useradd --create-home --shell /bin/bash --uid ${USER_ID} --gid ${GROUP_ID} tvm; \
fi

USER tvm

WORKDIR /home/tvm

RUN echo "git clone --recursive https://github.com/apache/incubator-tvm tvm && \
	cd tvm && git checkout 728b829575e5e690870b111ae2256cbe0f3dbe6f && \
	echo set\(USE_LLVM llvm-config-4.0\) >> config.cmake && \
	echo set\(USE_CUDA ON\) >> config.cmake && \
	mkdir -p build && cd build && cmake .. && make -j4" > build_tvm.sh

#RUN cd tvm/python && python3 setup.py install --user

#RUN cd tvm/nnvm/python && python3 setup.py install --user

#RUN cd tvm/topi/python && python3 setup.py install --user

CMD ["bash"]