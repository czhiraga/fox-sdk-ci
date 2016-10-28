# About Docker

## 1. Install dockertoolbox via Homebrew

### 1.1 Install Command

> `brew cask install docker-toolbox`

### 1.2 Comfirm

> `docker-machine version`

### 1.3 Check available VM

> `docker-machine ls`

### 1.4 Create new VM

Create new VM if not exist available VM.

`docker-machine create --driver virtualbox default`

> ※ choice `virtualbox` by `--driver` option. `default` is VM's name.

### 1.5 Display created VM info

`docker-machine env default`

> 作成したVMとDockerコマンドがやりとりするために必要な情報が表示されるので、`export`を手動で実行するか<br>`eval "$(docker-machine env default)"`を実行する

```sh
$ docker-machine env default
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://172.16.62.130:2376"
export DOCKER_CERT_PATH="/Users/<yourusername>/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
# Run this command to configure your shell:
# eval "$(docker-machine env default)"
```

## 2. Create a Dockerfile

### 2.1 launch container

`docker run -it ubuntu:16.04 sh`

### 2.2 Dockerfile sample

```sh
FROM ubuntu:16.04

MAINTAINER Garhira

# Install sudo
RUN apt-get update \
  && apt-get -y install sudo \
  && useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo

# Install 32bit lib
RUN sudo apt-get -y install lib32stdc++6 lib32z1

# Install Java8
RUN apt-get install -y software-properties-common curl \
    && add-apt-repository -y ppa:openjdk-r/ppa \
    && apt-get update \
    && apt-get install -y openjdk-8-jdk

# Download Android SDK
RUN sudo apt-get -y install wget \
  && cd /usr/local \
  && wget https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz \
  && tar zxvf android-sdk_r24.4.1-linux.tgz \
  && rm -rf /usr/local/android-sdk_r24.4.1-linux.tgz

# Environment variables
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV ANDROID_HOME /usr/local/android-sdk-linux
ENV PATH $ANDROID_HOME/platform-tools:$ANDROID_HOME/tools:$PATH

# Update of Android SDK
RUN echo y | android update sdk --no-ui --all --filter "android-23,build-tools-23.0.3" \
  && echo y | android update sdk --no-ui --all --filter "extra-google-m2repository,extra-android-m2repository"
```

## 2.3 Create an image from Dockerfile

In saved Dockerfile directory, type below command

`docker build -t garhira/fox-android-sdk:1.0` ./


**Check docker images**

`docker images`
