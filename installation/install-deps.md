# Installing ForkBase Dependencies #

Quick Reference (A-Z order): [boost](#boost), [ccache](#ccache), [CMake](#cmake), [crypto++](#crypto), [CZMQ](#czmq), [gflags](#gflags), [gtest](#google-test), [JDK](#jdk), [Maven](#maven), [OpenSSL](#openssl), [protobuf](#protocol-buffers), [RocksDB](#rocksdb), [ZMQ](#zmq) 

This documentation shows the key compilation commands and environment variable settings for installing ForkBase dependencies. All the dependencies are installed through the corresponding source-code releases. Generally, in order to install a dependency, you need to 

1. download the source code package (`.gz`, `.bz2` or `.zip`), 
2. unzip the package, 
3. go to the unzipped folder (i.e., root of the source code),
4. run the compilation commands listed below, and 
5. configure environment variables in `~/.profile` or `~/.bash_profile`. 

Some common dependencies can be easily resolved by using the package management utility shipped along with the Linux distribution, such as `apt` in Ubuntu and `yum` in RHEL/CentOS/Fedora. Or, you may alternatively install those dependencies via source code. 

The following commands and settings have been manually verified on Ubuntu 16.04 and Fedora 28. There may exist slight difference (e.g., names of packages provided by the package manager) on other Linux distributions. But the overall compilation commands as well as the environment variable settings should be compatible.  

## Preparation ##

#### Installing Compilation Tools ####

Ubuntu:

    $ sudo apt install build-essential

RHEL/CentOS/Fedora:

    $ sudo yum group install "Development Tools" "C Development Tools and Libraries"

Make sure the GCC version is `4.9` or higher. 

#### Setting Environment Variables ####

    export SHARE_HOME="/path/to/dependency/share"
    export NCORES="$(cat /proc/cpuinfo | grep "cpu cores" | uniq | awk '{print $NF}')"

Note that the parallelism of compilation is set to the number of CPU cores by default, as defined in the `NCORES` environment variable. You may change it to any parallelism as you want. 

## Core ##

### [CMake](https://cmake.org/download/) ###

Minimum required version: `2.8`

#### Installing CMake ####

    $ ./bootstrap --prefix=$SHARE_HOME/cmake && make -j$NCORES && make install

#### Setting CMake Environment Variables #### 

    # CMake
    export CMAKE_HOME="$SHARE_HOME/cmake"
    export PATH="$CMAKE_HOME/bin:$PATH"

### [OpenSSL](https://www.openssl.org/source/) ###

Ubuntu:

    $ sudo apt install openssl


RHEL/CentOS/Fedora:

    $ sudo yum install openssl

***[TODO]*** *Need to figure out how to install OpenSSL via source code.* 

### [Boost](https://www.boost.org/users/download/) ###

Minimum required version: `1.66.0`

#### Installing Boost ####

    $ ./bootstrap.sh --prefix=$SHARE_HOME/boost && ./b2 install -j $NCORES

#### Setting Boost Environment Variables #### 

    # Boost
    export BOOST_ROOT="$SHARE_HOME/boost"
    export CPATH="$BOOST_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$BOOST_ROOT/lib:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$BOOST_ROOT/lib:$LIBRARY_PATH"

### [Gflags](https://github.com/gflags/gflags/releases) ###

Minimum required version: `2.1.1`

#### Installing Gflags ####

    $ mkdir -p build && rm -rf build/* && cd build && cmake -DCMAKE_INSTALL_PREFIX=$SHARE_HOME/gflags -DBUILD_SHARED_LIBS=ON -DBUILD_STATIC_LIBS=ON .. && make -j$NCORES && make install

#### Setting Gflags Environment Variables #### 

    # gflags
    export GFLAGS_ROOT="$SHARE_HOME/gflags"
    export CPATH="$GFLAGS_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$GFLAGS_ROOT/lib:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$GFLAGS_ROOT/lib:$LIBRARY_PATH"
    export CMAKE_INCLUDE_PATH="$GFLAGS_ROOT/include:$CMAKE_INCLUDE_PATH"
    export CMAKE_LIBRARY_PATH="$GFLAGS_ROOT/lib:$CMAKE_LIBRARY_PATH"

### [Protocol Buffers](https://github.com/google/protobuf/releases/) ###

Minimum required version: `2.6.1`

#### Installing Protobuf ####

    $ ./autogen.sh && ./configure --prefix=$SHARE_HOME/protobuf && make -j$NCORES && make install

#### Setting Protobuf Environment Variables #### 

    # Protocol Buffers
    export PROTOBUF_ROOT="$SHARE_HOME/protobuf"
    export PATH="$PROTOBUF_ROOT/bin:$PATH"
    export CPATH="$PROTOBUF_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$PROTOBUF_ROOT/lib:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$PROTOBUF_ROOT/lib:$LIBRARY_PATH"
    export CMAKE_INCLUDE_PATH="$PROTOBUF_ROOT/include:$CMAKE_INCLUDE_PATH"
    export CMAKE_LIBRARY_PATH="$PROTOBUF_ROOT/lib:$CMAKE_LIBRARY_PATH"

### [Crypto++](https://www.cryptopp.com/#download) ###

Minimum required version: `6.1.0`

#### Installing Crypto++ ####

    $ make -j$NCORES libcryptopp.a libcryptopp.so && make install PREFIX=$SHARE_HOME/cryptopp

#### Setting Crypto++ Environment Variables ####

    # Crypto++
    export CRYPTOPP_ROOT="$SHARE_HOME/cryptopp"
    export CPATH="$CRYPTOPP_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$CRYPTOPP_ROOT/lib:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$CRYPTOPP_ROOT/lib:$LIBRARY_PATH"
    export CMAKE_INCLUDE_PATH="$CRYPTOPP_ROOT/include:$CMAKE_INCLUDE_PATH"
    export CMAKE_LIBRARY_PATH="$CRYPTOPP_ROOT/lib:$CMAKE_LIBRARY_PATH"

### [ZMQ](https://github.com/zeromq/libzmq/releases) ###

Minimum required version: `4.2.1`

#### Installing ZMQ ####

    $ ./autogen.sh && ./configure --prefix=$SHARE_HOME/zmq && make -j$NCORES && make install

#### Setting ZMQ Environment Variables ####

    # ZMQ
    export ZMQ_ROOT="$SHARE_HOME/zmq"
    export CPATH="$ZMQ_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$ZMQ_ROOT/lib:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$ZMQ_ROOT/lib:$LIBRARY_PATH"
    export CMAKE_INCLUDE_PATH="$ZMQ_ROOT/include:$CMAKE_INCLUDE_PATH"
    export CMAKE_LIBRARY_PATH="$ZMQ_ROOT/lib:$CMAKE_LIBRARY_PATH"

### [CZMQ](https://github.com/zeromq/czmq/releases) ###

Minimum required version: `4.0.2`

#### Installing CZMQ ####

    $ ./autogen.sh && ./configure --prefix=$SHARE_HOME/czmq && make -j$NCORES && make install

#### Setting CZMQ Environment Variables #### 

    # CZMQ
    export CZMQ_ROOT="$SHARE_HOME/czmq"
    export CPATH="$CZMQ_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$CZMQ_ROOT/lib:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$CZMQ_ROOT/lib:$LIBRARY_PATH"
    export CMAKE_INCLUDE_PATH="$CZMQ_ROOT/include:$CMAKE_INCLUDE_PATH"
    export CMAKE_LIBRARY_PATH="$CZMQ_ROOT/lib:$CMAKE_LIBRARY_PATH"

### [RocksDB](https://github.com/facebook/rocksdb/releases) ###

Minimum required version: `5.8`

#### Resolving RocksDB Dependencies ####

Ubuntu:

    $ sudo apt install libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev

RHEL/CentOS/Fedora:

    $ sudo yum install snappy snappy-devel zlib zlib-devel bzip2 bzip2-devel lz4-devel libzstd-devel

#### Installing RocksDB ####

    $ USE_RTTI=1 DISABLE_WARNING_AS_ERROR=ON bash -c 'make -j$NCORES shared_lib' && INSTALL_PATH=$SHARE_HOME/rocksdb bash -c 'make install-shared'

#### Setting RocksDB Environment Variables #### 

    # RocksDB
    export ROCKSDB_ROOT="$SHARE_HOME/rocksdb"
    export CPATH="$ROCKSDB_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$ROCKSDB_ROOT/lib:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$ROCKSDB_ROOT/lib:$LIBRARY_PATH"
    export CMAKE_INCLUDE_PATH="$ROCKSDB_ROOT/include:$CMAKE_INCLUDE_PATH"
    export CMAKE_LIBRARY_PATH="$ROCKSDB_ROOT/lib:$CMAKE_LIBRARY_PATH"

## Optional ##

### [Google Test](https://github.com/google/googletest/releases) ###

Minimum required version: `1.8.0`

#### Installing Gtest ####

    $ mkdir -p build && rm -rf build/* && cd build && cmake -DCMAKE_INSTALL_PREFIX=$SHARE_HOME/gtest -DBUILD_SHARED_LIBS=ON .. && make -j$NCORES && make install

#### Setting Gtest Environment Variables #### 

    # gtest
    export GTEST_ROOT="$SHARE_HOME/gtest"
    export GTEST_LIB_DIR=`ls -d $GTEST_ROOT/lib*`
    export CPATH="$GTEST_ROOT/include:$CPATH"
    export LD_LIBRARY_PATH="$GTEST_LIB_DIR:$LD_LIBRARY_PATH"
    export LIBRARY_PATH="$GTEST_LIB_DIR:$LIBRARY_PATH"
    export CMAKE_INCLUDE_PATH="$GTEST_ROOT/include:$CMAKE_INCLUDE_PATH"
    export CMAKE_LIBRARY_PATH="$GTEST_LIB_DIR:$CMAKE_LIBRARY_PATH"

### [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) ###

#### Setting JDK Environment Variables #### 

    # JDK
    export JAVA_VER=10.0.2
    export JAVA_HOME=$SHARE_HOME/java/jdk-$JAVA_VER
    export PATH=$JAVA_HOME/bin:$PATH

You should change `JAVA_VER` according to your downloaded version.

### [Maven](https://maven.apache.org/download.cgi) ###

#### Setting Maven Environment Variables #### 

    # Maven
    export MAVEN_VER=3.5.4
    export MAVEN_HOME=$SHARE_HOME/maven/apache-maven-$MAVEN_VER
    export PATH=$MAVEN_HOME/bin:$PATH

You should change `MAVEN_VER` according to your downloaded version.

## Other ##

The following are not ForkBase dependencies. Instead, they could facilitate your development work. 

### [Ccache](https://ccache.samba.org/download.html) ###

#### Installing Ccache ####

    $ ./configure --prefix=$SHARE_HOME/ccache && make -j$NCORES && make install
    $ cd $SHARE_HOME/ccache/bin/ && ln -s ccache gcc && ln -s ccache g++ && ln -s ccache cc && ln -s ccache c++
    $ bash -c 'cd $SHARE_HOME/ccache/bin/ && ln -s ccache gcc && ln -s ccache g++ && ln -s ccache cc && ln -s ccache c++'

#### Setting Ccache Environment Variables #### 

    # ccache
    export CCACHE_HOME="$SHARE_HOME/ccache"
    export PATH="$CCACHE_HOME/bin:$PATH"
    export MANPATH="$CCACHE_HOME/share/man:$MANPATH"
