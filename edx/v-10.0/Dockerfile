ARG EDXAPP_RELEASE=open-release/hawthorn.1


FROM ubuntu:16.04 as base

# Configure locales
RUN apt-get update && \
    apt-get install -y gettext locales curl && \
    rm -rf /var/lib/apt/lists/*
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

#TODO use args for platform version
RUN curl -sLo edxapp.tar.gz https://github.com/edx/edx-platform/archive/open-release/hawthorn.1.tar.gz && \
    mkdir edxapp && \
    tar -xvzf edxapp.tar.gz &&  -C edxapp --strip-components 1\
    mv $(ls | grep edx-platform-open-release) edxapp && \
    rm ./*.tar.gz

ADD ./apt-req /apt-req

RUN apt-get update  && cat /apt-req | xargs apt-get install -y && \
    apt install -y python-pip && rm -rf /var/lib/apt/lists/*

WORKDIR edxapp

RUN pip install -r requirements/edx/base.txt

ENV SERVICE_VARIANT lms
ENV DJANGO_SETTINGS_MODULE lms.envs.aws

ADD ./*.json /

# Fake theme. Assuming xBlock can be rendered without a theme.
RUN  mkdir -p /edx/app/edxapp/themes



#RUN  apt-get update &&  apt-get install -y software-properties-common && \
#     while read line; do add-apt-repository  $line; done <  /edxapp/requirements/system/ubuntu/apt-repos.txt && \
#     apt-get update

#
#RUN apt-get update && \
#    apt-get install -y curl && \
#    apt-get install -y python && \
#    apt-get install -y git && \
#    apt-get install -y gcc && \
#    rm -rf /var/lib/apt/lists/*
##    curl -sLo get-pip.py https://bootstrap.pypa.io/get-pip.py && \
##    python ./get-pip.py  && \
##    rm ./get-pip.py && \
##    apt-get install -y build-essential autoconf libtool pkg-config \
##        python-opengl python-imaging python-pyrex python-pyside.qtopengl idle-python2.7 qt4-dev-tools qt4-designer \
##        libqtgui4 libqtcore4 libqt4-xml libqt4-test libqt4-script libqt4-network libqt4-dbus python-qt4 \
##        python-qt4-gl libgle3 python-dev
#
#
#RUN curl -sLo edxapp.tgz https://github.com/edx/edx-platform/archive/$EDXAPP_RELEASE.tar.gz && \
#    mv $(ls | grep edx-platform-open-release) edxapp && \
#    rm ./*.tar.gz
##    pip install -e . && \
##    pip install -e . requirements/edx/base.py


# TODO: Add custom settings
# General plan: Add export to the dockerfile.
#               Add custom env.json
#
#
#
#
#
#



