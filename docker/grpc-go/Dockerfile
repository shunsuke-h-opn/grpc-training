FROM --platform=linux/x86_64 golang:1.19-buster

# Install basics
RUN apt-get update \
 && apt-get install -y vim git zip wget
RUN apt install -y protobuf-compiler golang-goprotobuf-dev

# Install grpc, protoc-gen-go
ENV GOPATH=
RUN go mod init grpc-go 
RUN go get -u google.golang.org/grpc 
RUN go get -u google.golang.org/protobuf/cmd/protoc-gen-go


# Install protoc
RUN wget https://github.com/protocolbuffers/protobuf/releases/download/v3.6.1/protoc-3.6.1-linux-x86_64.zip \
 && unzip protoc-3.6.1-linux-x86_64.zip -d protoc \
 && cd protoc \
 && mv bin/protoc /usr/bin

ENV PATH $PATH:$GOPATH/bin

COPY ./services.sh /etc/services.sh

RUN chmod a+x /etc/services.sh

EXPOSE 50051

RUN sed -i -e '9,13s:^#::' /root/.bashrc