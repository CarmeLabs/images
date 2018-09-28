FROM carmelabs/jupyter-cpu:latest
LABEL maintainer="CarmeLabs <info@carme.ai>"

USER root
COPY config/* /tmp/config/

#Install Linux packages
RUN apt-get update && cat /tmp/config/packages.txt | xargs apt-get install -y --allow-downgrades --no-install-recommends

USER $NB_UID
RUN conda update -n base conda
#Install conda and pip requirements
RUN conda config --add channels fastai && conda config --add channels pytorch  && conda config --add channels peterjc123&& conda config --add channels conda-forge
RUN conda install --yes --file /tmp/config/conda-requirements.txt
RUN pip install -r /tmp/config/pip-requirements.txt
RUN jupyter serverextension enable --py nbgitpuller --sys-prefix

# Configure container startup
ENTRYPOINT ["tini", "-g", "--"]
CMD ["start-notebook.sh"]

# Switch back to jovyan to avoid accidental container runs as root
