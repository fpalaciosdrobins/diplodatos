En esta carpeta dejamos el Dockerfile para crear el entorno

Descargar el Dockerfile y crear la imágen mediante el comando
docker build - < Dockerfile

Genera la imágen con las siguiente configuración

# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.

FROM jupyter/base-notebook

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

USER root

# Install all OS dependencies for fully functional notebook server
RUN apt-get update && apt-get install -yq --no-install-recommends \
    build-essential \
    emacs \
    git \
    inkscape \
    jed \
    libsm6 \
    libxext-dev \
    libxrender1 \
    lmodern \
    netcat \
    pandoc \
    python-dev \
    texlive-fonts-extra \
    texlive-fonts-recommended \
    texlive-generic-recommended \
    texlive-latex-base \
    texlive-latex-extra \
    texlive-xetex \
    unzip \
    nano \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Agregamos las libreras necesarias para el práctico (pandas y seaborn)
RUN conda install --quiet --yes \
    'conda-forge::blas=*=openblas' \
    'pandas=0.23*' \
    'seaborn=0.8*'

RUN fix-permissions /home/$NB_USER

#Descarga el trabajo practico de limpieza de datos mediante el comando clone
#Subimos a este directorio también el archivo de salida del práctico tweet_energia_ok.csv

RUN cd /home/jovyan/work && \
    git clone https://github.com/fpalaciosdrobins/diplo-docker


