FROM 'jupyter/datascience-notebook'
USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ssh \
    postgresql \
    mysql-client \
    python3-dev \
    libmysqlclient-dev

RUN echo 'jovyan ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
USER jovyan

# We should move to a less janky spot.
WORKDIR /home/jovyan

ARG ANALYTICS_CORE_DIR
ADD $ANALYTICS_CORE_DIR/requirements.txt requirements.txt
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

ADD $ANALYTICS_CORE_DIR/docker docker
RUN sudo mkdir -p /usr/share/nltk_data
RUN sudo chown jovyan /usr/share/nltk_data
RUN ./docker/download_nltk

ADD $ANALYTICS_CORE_DIR/home/.fonts .fonts
RUN sudo chown -R jovyan .fonts
ADD $ANALYTICS_CORE_DIR/home/.ipython .ipython
RUN sudo chown -R jovyan .ipython
ADD $ANALYTICS_CORE_DIR/home/.jupyter .jupyter
RUN sudo chown -R jovyan .jupyter

# Clear matplotlib fontcache
RUN rm -fr ~/.cache/matplotlib
