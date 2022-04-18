# Base Image
FROM jupyter/base-notebook

# Maintainer
LABEL maintainer "Shiho ASA"

# Configure environment
ENV CONDA_DIR=/opt/conda \
    NB_USER=jovyan
    
# Install Jupyter Notebook and Hub
RUN conda install --quiet --yes numpy=1.21.5
RUN conda install --quiet --yes scipy
RUN conda install --quiet --yes sympy
RUN conda install --quiet --yes matplotlib
RUN conda clean -tipsy
RUN fix-permissions $CONDA_DIR


# Install Sample Notebook
COPY sample_notebook/CavityFlow_with_Navier-Stokes.ipynb /home/$NB_USER/
