# Cakeshop from scratch

This worked as described on a VM image of Ubuntu 18.04. Note that Haskell is memory-hungry and needs 2GB at a minimum, possibly more just to complete the `install` process. It also works with `brew` on macOS with somewhat less effort. These steps work as of 2019, Jun 29.

## Base environment

Some of these may already exist on the system you're using. Go lang needs to be 1.7+, Java 7+, there is some minimum requirement for Haskell as well, so just take latest. Minimum requirements for Berkeley DB (libdb) are also there. Extra steps around go-lang are only needed for the default Ubuntu 18.04 image. 

```shell
sudo apt update
sudo apt-get -y install openssh-server \
    openjdk-8-jdk \
    git \
    maven \
    nodejs \
    npm \
    golang \
    curl \
    libsodium-dev \
    libdb5.3-dev \
    libleveldb-dev \
    zlib1g-dev \
    libtinfo-dev
```

## Cakeshop build

```shell
mkdir -p ~/Projects/cakeshop.git
cd ~/Projects/cakeshop.git
git clone https://github.com/jpmorganchase/cakeshop.git
```

```shell
cd ~/Projects/cakeshop.git/cakeshop
mvn clean install -DskipTests
cd cakeshop-api
mvn install -DskipTests
java -jar target/cakeshop-0.11.0.war
```

## Related projects

```shell
mkdir -p ~/Projects/quorum.git
cd ~/Projects/quorum.git
git clone -b master --single-branch https://github.com/jpmorganchase/quorum.git
```

```shell
mkdir -p ~/Projects/istanbul-tools.git
cd ~/Projects/istanbul-tools.git
git clone -b master --single-branch https://github.com/getamis/istanbul-tools.git
```

```shell
mkdir ~/Projects/constellation.git
cd ~/Projects/constellation.git
git clone -b master --single-branch https://github.com/jpmorganchase/constellation.git
```

```shell
mkdir -p ~/Projects/tessera.git
cd ~/Projects/tessera.git
git clone -b master --single-branch https://github.com/jpmorganchase/tessera.git
```

## Related builds

These steps aren't strictly necessary. The repo was updated with the below binaries as listed on 2018, Jun 30.

```shell
cd ~/Projects/quorum.git/quorum
make all
mv ./build/bin/geth ~/Projects/cakeshop.git/cakeshop/cakeshop-api/src/main/resources/bin/geth/linux/geth
mv ./build/bin/bootnode ~/Projects/cakeshop.git/cakeshop/cakeshop-api/src/main/resources/bin/geth/linux/bootnode
```

```shell
cd ~/Projects/constellation.git/constellation
stack setup
stack install
mv ~/.local/bin/constellation-node ~/Projects/cakeshop.git/cakeshop/cakeshop-api/src/main/resources/bin/constellation/linux/constellation-node
```

Note that the istanbul build will report a failure with `urfave/cli`, ignore it and continue. Building istanbul-tools must be done with go 1.11+.

```shell
cd ~/Projects/istanbul-tools.git/istanbul-tools
go get github.com/getamis/istanbul-tools/cmd/istanbul
make
mkdir ~/Projects/cakeshop.git/cakeshop/cakeshop-api/src/main/resources/geth//istanbul-tools/linux
mv ~/go/bin/istanbul ~/Projects/cakeshop.git/cakeshop/cakeshop-api/src/main/resources/geth/quorum/istanbul-tools/linux/istanbul
```
