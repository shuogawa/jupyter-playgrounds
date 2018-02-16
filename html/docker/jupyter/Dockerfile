FROM conda/miniconda3

# bash
RUN set -eux \
 && rm -f /bin/sh \
 && ln -s /bin/bash /bin/sh

RUN set -eux \
 && apt-get update \
 && apt-get install -y curl unzip tree jq

RUN set -eux \
 && conda install jupyter -y --quiet

RUN set -eux \
 && pip install bash_kernel \
 && python -m bash_kernel.install

COPY . /docker-build-context

RUN set -eux \
 && cd /docker-build-context \
 && mkdir /root/.jupyter \
 && mkdir /root/.jupyter/custom \
 && cp __root__.jupyter__custom__custom.js /root/.jupyter/custom/custom.js \
 && cp __root__.jupyter__jupyter_notebook_config.py /root/.jupyter/jupyter_notebook_config.py

RUN set -eux \
 && python -V \
 && conda -V \
 && conda info -e \
 && jupyter kernelspec list

CMD jupyter notebook --notebook-dir=/notebook --ip='*' --port=8888 --no-browser --allow-root
