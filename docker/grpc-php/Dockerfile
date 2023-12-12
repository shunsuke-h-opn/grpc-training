# Dockerfile

FROM php:7.2.32-cli

WORKDIR /tmp
RUN apt-get update -y && apt-get install -y wget zip git automake zlib1g-dev libtool vim

# install protoc command
# releases: https://github.com/protocolbuffers/protobuf/releases
ENV proto_version 3.12.3
RUN wget https://github.com/protocolbuffers/protobuf/releases/download/v${proto_version}/protoc-${proto_version}-linux-x86_64.zip \
    && unzip -d protoc protoc-${proto_version}-linux-x86_64.zip \
    && mv protoc/bin/* /usr/local/bin/ \
    && mv protoc/include/* /usr/local/include/

# install grpc php plugin
# releases: https://github.com/grpc/grpc/releases
ENV grpc_version v1.30.2
WORKDIR /var/local
RUN git clone -b ${grpc_version} https://github.com/grpc/grpc --depth=1
WORKDIR /var/local/grpc
RUN git submodule update --init \
    && make grpc_php_plugin \
    && cp -p ./bins/opt/grpc_php_plugin /usr/local/bin/

RUN pecl install grpc

RUN curl -sS https://getcomposer.org/installer | php && php composer.phar

WORKDIR /var/local
RUN mkdir protos \
    && mkdir gen


COPY ./php.ini /usr/local/etc/php/php.ini
COPY ./services.sh /etc/services.sh

RUN chmod a+x /etc/services.sh

RUN sed -i -e '9,13s:^#::' /root/.bashrc

# ENTRYPOINT ["protoc", "--proto_path=/var/local/protos", "--php_out=/var/local/gen", "--grpc_out=/var/local/gen", "--plugin=protoc-gen-grpc=/usr/local/bin/grpc_php_plugin"]
# CMD ["hello.proto"]