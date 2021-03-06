FROM ubuntu:17.04

LABEL maintainer="Anton Shchukin <szwtdy@gmail.com>"
LABEL version="17.04-375.66-3"
LABEL com.prometheus.monitoring="true" com.prometheus.port="9090"

# install python modules for monitoring
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update \
 && apt-get install -yqq --no-install-recommends \
	python \
	python-pip \
	python-wheel \
	python-setuptools \
 && pip install nvidia-ml-py \
 && pip install prometheus_client \
 && apt-get -yqq remove --purge python-pip \
 && apt-get -yqq autoremove --purge \
 && apt-get -yqq clean \
 && rm -rf /var/lib/apt/lists/*

COPY resources/etc/OpenCL/vendors/nvidia.icd /etc/OpenCL/vendors/nvidia.icd
COPY resources/usr/bin/nvidia-smi /usr/bin/nvidia-smi
COPY resources/usr/share/nvidia/nvidia-application-profiles-375.66-rc \
 /usr/share/nvidia/nvidia-application-profiles-375.66-rc
COPY resources/usr/lib/x86_64-linux-gnu/libcuda.so.375.66 \
 resources/usr/lib/x86_64-linux-gnu/libnvidia-fatbinaryloader.so.375.66 \
 resources/usr/lib/x86_64-linux-gnu/libnvidia-opencl.so.375.66 \
 resources/usr/lib/x86_64-linux-gnu/libOpenCL.so.1.0.0 \
 resources/usr/lib/x86_64-linux-gnu/libnvidia-ml.so.375.66 \
 /usr/lib/x86_64-linux-gnu/
COPY resources/usr/local/bin/monitor.py /usr/local/bin/

RUN chmod +x /usr/local/bin/monitor.py

RUN ln -s /usr/lib/x86_64-linux-gnu/libcuda.so.375.66 /usr/lib/x86_64-linux-gnu/libcuda.so.1 \
 && ln -s /usr/lib/x86_64-linux-gnu/libnvidia-opencl.so.375.66 /usr/lib/x86_64-linux-gnu/libnvidia-opencl.so.1 \
 && ln -s /usr/lib/x86_64-linux-gnu/libnvidia-ml.so.375.66 /usr/lib/x86_64-linux-gnu/libnvidia-ml.so.1 \
 && ln -s /usr/lib/x86_64-linux-gnu/libnvidia-ml.so.1 /usr/lib/x86_64-linux-gnu/libnvidia-ml.so \
 && ln -s /usr/lib/x86_64-linux-gnu/libOpenCL.so.1.0.0 /usr/lib/x86_64-linux-gnu/libOpenCL.so.1.0 \
 && ln -s /usr/lib/x86_64-linux-gnu/libOpenCL.so.1.0 /usr/lib/x86_64-linux-gnu/libOpenCL.so.1 \
 && ln -s /usr/lib/x86_64-linux-gnu/libOpenCL.so.1 /usr/lib/x86_64-linux-gnu/libOpenCL.so \
 && ldconfig
