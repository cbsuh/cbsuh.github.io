# 이런저런 언어의 Compiler Build 해보기

## Build 환경

* CPU: intel i7-8700 3.20GHz
* RAM: 32GB
* SSD: NVMe
* WSL

## Go

> [Go - source](https://go.googlesource.com/go)

[Go - Installing Go from source](https://golang.org/doc/install/source) 문서를 따라하면 된다.

1. Go 언어를 설치.

    * Go의 toolchain이 Go로 만들어졌기 때문.

    > [Go - Getting Started](https://golang.org/doc/install)를 따라서 설치

1. Source Code 받기

    ``` bash
    git clone https://go.googlesource.com/go goroot
    cd goroot
    git checkout go1.13
    ```

1. Build

    ``` bash
    cd src
    ./all.bash
    ```

* 일부 test가 WSL에서 실패함.
  > 아마, WSL이 진짜 리눅스와 좀 달라서 그런 듯

* 결과

  ``` bash
  goroot/bin/
  ```

* 소요 시간

  ``` bash
  real    11m22.156s
  user    11m43.047s
  sys     25m28.188s
  ```

## Swift

> [Swift: Source Code](https://swift.org/source-code/)

[GitHub: apple/swift/README.md](https://github.com/apple/swift) 문서를 따라하면 된다.

1. 필요한 package 설치

    ``` bash
    sudo apt-get install git cmake ninja-build clang python uuid-dev libicu-dev icu-devtools libedit-dev libxml2-dev libsqlite3-dev swig libpython-dev libncurses5-dev pkg-config libcurl4-openssl-dev systemtap-sdt-dev tzdata rsync
    ```

1. 작업 folder 생성

    ``` bash
    mkdir swift-source
    cd swift-source
    ```

    * Source code가 이 folder의 하위 folder에 저장되어야 한다!!!

1. Git으로 source code 받기

    ``` bash
    git clone https://github.com/apple/swift.git
    ./swift/utils/update-checkout --clone
    ```

    * SSH로도 받을 수 있음. SSH가 http보다 좀 빠를 것임 :)

1. build

    ``` bash
    swift/utils/build-script --release-debuginfo
    ```

* 소요 시간

    ``` bash
    real    58m49.693s
    user    408m58.344s
    sys     82m2.938s
    ```

## JavaScript - V8

> [Documentation V8](https://v8.dev/docs)

1. depot_tools 설치

    > [depot_tools_tutorial(7) Manual Page](https://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools_tutorial.html#_setting_up) 참고

    ``` bash
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
    export PATH=$PATH:/path/to/depot_tools
    ```

1. Update depot_tools

    > [Checking out the V8 source code](https://v8.dev/docs/source-code) 참고.

    ``` bash
    gclient 
    ```

1. Source Code 받기

    > [Checking out the V8 source code](https://v8.dev/docs/source-code) 참고.

    ``` bash
    mkdir ~/v8
    cd ~/v8
    fetch v8
    cd v8
    ```

1. Build

    > [Building V8 from source](https://v8.dev/docs/build) 참고.

    * Installing build dependencies

        ``` bash
        gclient sync
        ./build/install-build-deps.sh
        ```

        + 이게 생각보다 시간이 많이 걸렸다.
        + 내부적으로 `sudo`를 하므로 가끔 암호를 입력해야 한다.

    * Building V8

        ``` bash
        cd ~/v8/v8
        git checkout master
        git pull && gclient sync
        tools/dev/gm.py x64.release.check
        ```

* 소요 시간

    ``` bash
    real    11m53.411s
    user    86m58.391s
    sys     30m50.859s
    ```

## Erlang

[GitHub: Erlang/OTP](https://github.com/erlang/otp)

``` bash
git clone https://github.com/erlang/otp.git
cd otp
./otp_build autoconf
./configure
make
```

* 소요 시간

``` bash
real    18m57.416s
user    14m4.063s
sys     19m17.172s
```
