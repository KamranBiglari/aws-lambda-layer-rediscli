FROM public.ecr.aws/lambda/provided:latest
#
# versions
#
USER root
RUN mkdir -p /opt
WORKDIR /opt
#
# tools
#

RUN yum update -y \
    && yum install -y zip unzip wget tar gzip which pkgconfig \
    && yum group install -y "Development Tools"

#
# redis cli
#

RUN curl https://download.redis.io/redis-stable.tar.gz -o redis-stable.tar.gz
RUN tar -xzvf redis-stable.tar.gz 
RUN cd redis-stable/ && make  \
    && make install

# organize for self-contained usage

RUN mv  $(eval which redis-cli)  /opt/rediscli

# cleanup
RUN rm -rf /opt/awscli/v2/current/dist/awscli/examples

#
# create the bundle
#

RUN cd /opt \
    &&  zip -9 -j ../layer.zip /opt/rediscli \
    && echo "/layer.zip is ready" \
    && ls -alh /layer.zip;

RUN /opt/rediscli --version
WORKDIR /
ENTRYPOINT ["/bin/bash"]
