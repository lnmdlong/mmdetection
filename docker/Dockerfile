ARG PYTORCH="1.8.1"
ARG CUDA="10.2"
ARG CUDNN="8"

# FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel
# FROM tnn-images.tencentcloudcr.com/backend/tnn_troch:tnn-libtorch-10.2
FROM mmdetection_convert:latest 

ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0+PTX"
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"

RUN apt-get update && apt-get install -y ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install MMCV
RUN pip install --upgrade pip
RUN pip install mmcv-full==1.3.9 -f https://download.openmmlab.com/mmcv/dist/cu102/torch1.8.1/index.html 

# Install MMDetection
# RUN conda clean --all
#RUN git clone https://github.com/open-mmlab/mmdetection.git /mmdetection
RUN git clone -b torchscript_export https://github.com/lnmdlong/mmdetection.git /mmdetection
WORKDIR /mmdetection
ENV FORCE_CUDA="1"
RUN pip install -r requirements/build.txt
RUN pip install --no-cache-dir -e .
