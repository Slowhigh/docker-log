# Base Image
FROM jupyter/base-notebook

# Maintainer
LABEL maintainer "SomeDay"

# Configure environment
ENV CONDA_DIR=/opt/conda \
	NB_USER=jovyan

# Install Jupyter Notebook and Hub
RUN conda install --quiet --yes -c anaconda \
	numpy \
	scipy \
	sympy \
	matplotlib \
	&& conda clean -tipsy \
	&& fix-permissions $CONDA_DIR

# Install Sample Notebook
# COPY sample_notebook/CavityFlow_with_Navier-Stokes.ipynb /home/$NB_USER/
