#!/usr/bin/env bash

FROM dataquestio/ubuntu-base

# Export env settings
ENV TERM=xterm
ENV LANG en_US.UTF-8

RUN apt-get update -y && apt-get install build-essential -y

ADD apt-packages.txt /tmp/apt-packages.txt
RUN xargs -a /tmp/apt-packages.txt apt-get install -y

RUN pip install virtualenv
RUN /usr/local/bin/virtualenv /opt/ds --distribute --python=/usr/bin/python3

ADD /requirements/ /tmp/requirements

RUN /opt/ds/bin/pip install -r /tmp/requirements/pre-requirements.txt
RUN /opt/ds/bin/pip install -r /tmp/requirements/requirements.txt

RUN useradd --create-home --home-dir /home/datafirst --shell /bin/bash datafirst
RUN chown -R datafirst /opt/ds
RUN adduser datafirst sudo

ADD run_ipython.sh /home/datafirst
RUN chmod +x /home/datafirst/run_ipython.sh
RUN chown datafirst /home/datafirst/run_ipython.sh

ADD .bashrc.template /home/datafirst/.bashrc

EXPOSE 8888
RUN usermod -a -G sudo datafirst
RUN echo "datafirst ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
USER datafirst
RUN mkdir -p /home/datafirst/notebooks
ENV HOME=/home/datafirst
ENV SHELL=/bin/bash
ENV USER=ds
VOLUME /home/datafirst/notebooks
WORKDIR /home/datafirst/notebooks

CMD ["/home/datafirst/run_ipython.sh"]
