# https://github.com/ContinuumIO/docker-images/tree/master/miniconda3
# https://github.com/gusutabopb/imongo
# https://github.com/gusutabopb/imongo/issues/3

FROM conda/miniconda3

# bash
RUN set -eux \
 && rm -f /bin/sh \
 && ln -s /bin/bash /bin/sh

RUN set -eux \
 && apt-get update \
 && apt-get install -y git build-essential mongodb

RUN set -eux \
 && conda install jupyter -y --quiet

#RUN set -eux \
# && pip install imongo-kernel

RUN set -eux \
 && git clone https://github.com/gusutabopb/imongo \
 && cd imongo \
 && python setup.py install \
 && pip install pyyaml

COPY . /docker-build-context

RUN set -eux \
 && cd /docker-build-context \
 && mkdir /root/.jupyter \
 && cp __root__.jupyter__imongo_config.yml /root/.jupyter/imongo_config.yml \
 && cp __root__.jupyter__jupyter_notebook_config.py /root/.jupyter/jupyter_notebook_config.py

RUN set -eux \
 && python -V \
 && conda -V \
 && conda info -e \
 && jupyter kernelspec list \
 && mongo --version

CMD jupyter notebook --notebook-dir=/notebook --ip='*' --port=8888 --no-browser --allow-root
