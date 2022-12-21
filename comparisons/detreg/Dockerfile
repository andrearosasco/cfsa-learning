# docker run -it --rm --gpus=all -v $(pwd):/home/user --network host --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --device="/dev/video4:/dev/video0" --ipc=host cfsa:detreg
FROM nvidia/cuda:11.3.1-cudnn8-devel-ubuntu20.04

ENV CONDA_VERSION "py38_4.11.0"
ENV CONDA_MD5 718259965f234088d785cad1fbd7de03

ENV PYTHONDONTWRITEBYTECODE=true

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y -qq apt-utils build-essential ca-certificates cmake cmake-curses-gui curl emacs-nox git git-lfs glmark2 gnupg2 gpg htop iputils-ping jq libssl-dev libusb-1.0-0-dev libz-dev lsb-release mesa-utils nano psmisc sudo unzip vim wget zip ffmpeg libsm6 libxext6 && \
    rm -rf /var/lib/apt/lists/*

#  Install conda
RUN apt-get update && apt-get install -y --no-install-recommends wget bzip2 \
    && addgroup user \
    && useradd -ms /bin/bash user -g user \
    && wget --quiet https://repo.continuum.io/miniconda/Miniconda3-$CONDA_VERSION-Linux-x86_64.sh \
    && mv Miniconda3-$CONDA_VERSION-Linux-x86_64.sh miniconda.sh \
    && sh ./miniconda.sh -b -p /opt/conda \
    && ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh \
    && echo ". /opt/conda/etc/profile.d/conda.sh" >> /home/user/.bashrc \
    && echo "conda activate base" >> /home/user/.bashrc \
    && find /opt/conda/ -follow -type f -name '*.a' -delete \
    && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
    && /opt/conda/bin/conda clean -afy \
    && chown -R user:user /opt/conda

# Setting user with passwordless sudo
RUN sudo usermod -a -G sudo user && \
    sed -i.bkp -e 's/%sudo\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%sudo ALL=NOPASSWD:ALL/g' /etc/sudoers

ENV PATH="/opt/conda/bin:/opt/conda/condabin:${PATH}"
USER user:user
WORKDIR /home/user

RUN conda install --yes --freeze-installed python=3.7 \
    && conda install --yes --freeze-installed pytorch==1.8.0 torchvision==0.9.0 torchaudio==0.8.0 cudatoolkit=10.2 -c pytorch \
    && pip install pycocotools tqdm cython scipy opencv-contrib-python==4.5.1.48 pandas seaborn opencv-python \
    && conda clean -afy \
    && find /opt/conda/ -follow -type f -name '*.a' -delete ; 2>/dev/null\
    && find /opt/conda/ -follow -type f -name '*.pyc' -delete ; 2>/dev/null\
    && find /opt/conda/ -follow -type f -name '*.js.map' -delete -delete ; 2>/dev/null\
    && find /opt/conda/lib/python*/site-packages/bokeh/server/static -follow -type f -name '*.js' ! -name '*.min.js' -delete ; 2>/dev/null


ENV PYTHONPATH "/home/user"
