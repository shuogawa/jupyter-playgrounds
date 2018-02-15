FROM conda/miniconda2

RUN conda install jupyter -y --quiet

RUN apt-get update

RUN apt-get remove --purge nodejs

RUN apt-get install -y git build-essential curl

RUN set -eux \
 && curl -sL https://deb.nodesource.com/setup_8.x | bash - \
 && apt-get install -y nodejs

RUN set -eux \
 && git clone https://github.com/notablemind/jupyter-nodejs \
 && cd jupyter-nodejs \
 && mkdir -p /root/.ipython/kernels/nodejs \
 && npm install \
 && node install.js \
 && npm run build \
 && npm run build-ext \
 && cd ./node_modules/zmq \
 && npm run install

COPY . /docker-build-context

RUN set -eux \
 && cd /docker-build-context \
 && mkdir /root/.jupyter \
 && cp __root__.jupyter__jupyter_notebook_config.py /root/.jupyter/jupyter_notebook_config.py

RUN set -eux \
 && python --version \
 && conda --version \
 && jupyter --version \
 && node -v \
 && npm -v

CMD jupyter notebook --notebook-dir=/notebook --ip='*' --port=8888 --no-browser --allow-root
