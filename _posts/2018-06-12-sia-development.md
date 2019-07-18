---
title: Sia Development
tags: [blockchain, tutorial]
---

> Sia is the leading decentralized cloud storage platform. No signups, no servers, no trusted third parties. Sia leverages blockchain technology to create a data storage marketplace that is more robust and more affordable than traditional cloud storage providers.

## 开发环境

### 开发平台

- 最基本开发要求
  - 获取完整 [Sia](https://github.com/NebulousLabs/Sia) 源码
  - 能对源码进行编辑
  - 配置 [Go](https://golang.org/) 语言开发环境
- 建议开发环境
  - 操作系统：[Ubuntu Desktop 18.04](https://www.ubuntu.com/download/desktop?)
  - 集成开发环境：[GoLand](https://www.jetbrains.com/go/https://www.jetbrains.com/go/)（可通过 [Toolbox App](https://www.jetbrains.com/toolbox/app/) 安装，账户可通过华科邮箱[注册](https://www.jetbrains.com/shop/eform/students)，获得完整版本）

#### Ubuntu Desktop 配置 Go 开发环境

1. [下载](https://golang.org/dl/)对应版本的 Go 二进制包
1. 安装（解压）二进制包到自定义路径（推荐 `/usr/local`）

    ```sh
    sudo tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
    ```

1. 配置环境变量，将以下内容追加到 `~/.bashrc` 中 (`GOROOT` 为上一步的安装路径，`GOPATH` 可自定义)

    ```sh
    export GOROOT=/usr/local/go
    export GOPATH=$HOME/Documents/Go
    export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
    ```

1. 通过 `go env` 检查环境配置

    ```sh
    lynn@192.168.127.100 ~ $ go env
    GOARCH="amd64"
    GOBIN=""
    GOCACHE="/home/lynn/.cache/go-build"
    GOEXE=""
    GOHOSTARCH="amd64"
    GOHOSTOS="linux"
    GOOS="linux"
    GOPATH="/home/lynn/Documents/Go"
    GORACE=""
    GOROOT="/usr/local/go"
    GOTMPDIR=""
    GOTOOLDIR="/usr/local/go/pkg/tool/linux_amd64"
    GCCGO="gccgo"
    CC="gcc"
    CXX="g++"
    CGO_ENABLED="1"
    CGO_CFLAGS="-g -O2"
    CGO_CPPFLAGS=""
    CGO_CXXFLAGS="-g -O2"
    CGO_FFLAGS="-g -O2"
    CGO_LDFLAGS="-g -O2"
    PKG_CONFIG="pkg-config"
    GOGCCFLAGS="-fPIC -m64 -pthread -fmessage-length=0 -fdebug-prefix-map=/tmp/go-build200491170=/tmp/go-build -gno-record-gcc-switches"
    ```

> 参考 [Install the Go tools](https://golang.org/doc/install#install)

#### GoLand 配置调试

Sia 源码可生成 *siad* 和 *siac* 两个可执行二进制文件，在使用 GoLand 进行开发时，可将它们所属的包作为入口启动调试，调试的配置信息可根据 *Makefile* 文件进行设置

1. 通过顶部菜单 `Run -> Edit Configurations...` 启动 `Run/Debug Configurations`
1. 通过 `Add New Configuration -> Go Build` 添加新调试配置
1. 在 `Configuration` 选项卡中将 `Go tool arguments` 设置为

    `-tags "dev debug profile netgo" -ldflags "-X github.com/NebulousLabs/Sia/build.GitRevision=DEBUG_COMMIT -X github.com/NebulousLabs/Sia/build.BuildTime=DEBUG_TIME"`

    其他设置如下表（以 *siad* 为例）

    | Item              | Configuration                                                                          |
    | ----------------- | -------------------------------------------------------------------------------------- |
    | Run kind          | Package                                                                                |
    | Package path      | github.com/NebulousLabs/Sia/cmd/siad                                                   |
    | Run after build   | Checked                                                                                |
    | Program arguments | --modules=g --sia-directory=/home/lynn/sia --profile-directory=/home/lynn/sia/profiles |
    | Module            | Sia                                                                                    |

    > - 通过上述配置启动调试，等价于在编译出 *siad* 后，运行 `siad --modules=g --sia-directory=/home/lynn/sia --profile-directory=/home/lynn/sia/profiles` 时进行调试
    > - `Go tool arguments` 的配置与 *Makefile* 保持一致， `DEBUG_COMMIT` 和 `DEBUG_TIME` 可根据 *Makefile* 中的设置替换为实际值
    > - `Package path` 和 `Program arguments` 可根据实际要编译运行的 `main` 函数所在的包（如调试 *siac* 时设置为 `github.com/NebulousLabs/Sia/cmd/siac`）和启动相应程序时的实际运行参数进行设置

### 测试平台

- 最基本测试要求
  - 任意 Go 语言支持的操作系统
  - 一台主机
- 建议测试环境
  - 虚拟化平台：[VMware Workstation Pro 14](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)
  - 虚拟机操作系统：[Ubuntu Server 18.04](https://www.ubuntu.com/download/server) （无需安装 Go）
  - 虚拟机配置：1GB 内存、8GB 存储
  - 多台虚拟主机组成的集群

#### Ubuntu Server 配置静态 IP

假设虚拟机的网络适配器设置为 NAT，NAT 适配器的网关已设置为 *192.168.127.2*，欲设置虚拟机的静态 IP 为 *192.168.127.21*，子网掩码为 *255.255.255.0*

1. 修改 `/etc/netplan/` 路径下的配置文件（默认为 `50-cloud-init.yaml`）

    ```yaml
    # This file is generated from information provided by
    # the datasource.  Changes to it will not persist across an instance.
    # To disable cloud-init's network configuration capabilities, write a file
    # /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
    # network: {config: disabled}
    network:
        ethernets:
            ens33:
                addresses: [192.168.127.21/24]
                dhcp4: no
                optional: true
                gateway4: 192.168.127.2
        version: 2
    ```

1. 应用配置项

    ```sh
    sudo netplan apply
    ```

> 参考 [Configure Static IP Addresses On Ubuntu 18.04 LTS Server](https://websiteforstudents.com/configure-static-ip-addresses-on-ubuntu-18-04-beta/)、[配置脚本](https://github.com/lynn9388/script-tools/blob/master/ubuntu/config-ip.sh)

## 编译源码

*Makefile* 文件中默认包含 `release` （标准版）、`dev`（开发版）、`debug`（调试版） 三种常规编译目标及各自包含数据竞争检测的编译目标，开发过程中主要编译 `dev` 版本

> 参考 [Build Sia](https://github.com/NebulousLabs/Sia/blob/master/doc/Guide%20to%20Contributing%20to%20Sia.md#build-sia)

### 本地编译

在 *Sia* 根目录下执行 `make dev`，在 `$GOPATH/bin` 中生成可执行文件 *siac* 和 *siad*

> *Makefile* 文件中申明的第一个编译目标为默认编译目标（一般命名为 `all`），本项目中的设置为
>
> ```makefile
> # all will build and install release binaries
> all: release
> ```
>
> 即默认使用 `all` 作为编译目标（`all` 调用编译目标 `release`），所以使用 `make`、`make all`、`make release` 的效果相同
>
> 若要修改默认的编译目标为 `dev`，可将上述设置修改为
>
> ```makefile
> # all will build and install dev binaries
> all: dev
> ```

### 交叉编译

1. 针对交叉编译的目的平台（Windows、Linux、OpenBSD 等）和编译目标（release、dev、debug 等），修改 *Makefile* 文件中对应的编译指令，指定目的操作系统（`GOOS`）和系统架构（`GOARCH`）
1. 在 *Sia* 根目录下执行 `make dev`, 在 `$GOPATH/bin/$GOOS_$GOARCH` 中生成对应的可执行文件

如若要在 Linux 下交叉编译 Windows 版本，可在 *Makefile* 文件中修改

```makefile
# dev builds and installs developer binaries.
dev:
    go install -tags='dev debug profile netgo' -ldflags='$(ldflags)' $(pkgs)
```

为

```makefile
# dev builds and installs developer binaries.
dev:
    GOOS=windows GOARCH=amd64 go install -tags='dev debug profile netgo' -ldflags='$(ldflags)' $(pkgs)
```

> 参考 [Building windows go programs on linux](https://github.com/golang/go/wiki/WindowsCrossCompiling)、[Optional environment variables](https://golang.org/doc/install/source#environment)

## 搭建私有测试网络

搭建私有测试网络时，网络的组网过程主要包括[手动设置连接节点](#手动设置连接节点)和[自动发现网络节点](#自动发现网络节点)

现假设已具有符合最基本开发要求的开发平台，并已搭建多节点的虚拟机集群，各节点间可通过网络互访。（以下配置假设虚拟机处于同一内部网络，子网掩码为 *255.255.255.0*，网关为 *192.168.127.2*）

集群中各类型节点配置如下

| Node   | IP             | Type      | api-addr       | rpc-addr | host-addr | sia-directory   |
| ------ | -------------- | --------- | -------------- | -------- | --------- | --------------- |
| Node 0 | 192.168.127.21 | Bootstrap | localhost:9980 | :9981    |           | ~/sia/bootstrap |
| Node 1 | 192.168.127.21 | Miner     | localhost:9990 | :9991    |           | ~/sia/miner     |
| Node 2 | 192.168.127.22 | Host      | localhost:9980 | :9981    | :9982     | ~/sia/host      |
| Node 3 | 192.168.127.23 | Renter    | localhost:9980 | :9981    |           | ~/sia/renter    |

> 单个节点可通过组合多个模块，作为多用途节点
>
> 单个主机可通过为 *siad* 设置不同的端口和数据存储路径，运行多个 Sia 实例 （如 *192.168.127.21* 中的 *Node 0* 和 *Node 1*）
>
> 可通过 `./siad --help` 查看相关端口描述信息
>
> - `api-addr` 用于访问 API，*siac* 通过该主机、端口与 *siad* 交互（默认为 `localhost:9980`）
> - `rpc-addr` 用于节点间的通信， *gateway* 模块通过该端口与其他 Sia 实例进行连接 （默认为 `:9981`）
> - `host-addr` 用于 Host 节点监听文件上传下载请求（默认为 `:9982`）

### 手动设置连接节点

一个节点加入网络时，需要手动指定要进行连接的 Sia 实例，然后该节点可从所连接的节点获得网络中其他节点的信息，并与其他节点建立连接

由于不使用硬编码的 Bootstrap 节点，所以集群中的 *Node 0* 节点可优先启动，承担 Bootstrap 的功能，并作为新节点加入网络和新节点获取网络中其他节点信息的入口；新节点在[启动后台服务进程](#启动后台服务进程sia-实例)时，**需要设置 `--no-bootstrap` 参数**，并在[加入网络](#加入网络)时，将 *Node 0* 作为连接的对象，最终形成 P2P 网络

#### 设置运行环境

1. 创建用于构建集群的虚拟机，[配置各虚拟机的 IP](#ubuntu-server-配置静态-ip)
1. 创建保存节点数据的文件夹
1. 将[编译](#编译源码)的 `dev` 版 *siad* 和 *siac* 上传到各虚拟机 （可使用 [scp](https://linux.die.net/man/1/scp) 命令）

    ```sh
    lynn@192.168.127.100 bin $ scp siac siad lynn@192.168.127.21:~/sia
    siac                                          100%   12MB  54.5MB/s   00:00
    siad                                          100%   18MB  65.7MB/s   00:00
    ```

    > 参考 [Transferring files over SSH](https://stackoverflow.com/questions/343711/transferring-files-over-ssh)

#### 启动后台服务进程（Sia 实例）

通过 *siad* 运行 Sia 实例，启动各节点的后台服务进程，装载各节点所需模块

1. 新建一个终端，通过 *ssh* 登录相应节点
1. 进入保存 *siad* 的目录
1. 通过 `./siad --help` 查看相关命令和参数设置信息

    ```sh
    lynn@192.168.127.21 sia $ ./siad --help
    Running with debugging enabled
    Sia Daemon v1.3.3

    Usage:
    ./siad [flags]
    ./siad [command]

    Available Commands:
    help        Help about any command
    modules     List available modules for use with -M, --modules flag
    version     Print version information

    Flags:
        --agent string               required substring for the user agent (default "Sia-Agent")
        --api-addr string            which host:port the API server listens on (default "localhost:9980")
        --authenticate-api           enable API password protection
        --disable-api-security       allow siad to listen on a non-localhost address (DANGEROUS)
    -h, --help                       help for ./siad
        --host-addr string           which port the host listens on (default ":9982")
    -M, --modules string             enabled modules, see 'siad modules' for more info (default "cghrtw")
        --no-bootstrap               disable bootstrapping on this run
        --profile string             enable profiling with flags 'cmt' for CPU, memory, trace
        --profile-directory string   location of the profiling directory (default "profiles")
        --rpc-addr string            which port the gateway listens on (default ":9981")
    -d, --sia-directory string       location of the sia directory

    Use "./siad [command] --help" for more information about a command.
    ```

1. 通过 `./siad version` 检查编译的版本是否为 `dev` 版本

    ```sh
    lynn@192.168.127.21 sia $ ./siad version
    Running with debugging enabled
    Sia Daemon v1.3.3-dev
    ```

1. 通过 `./siad --modules gctwrhm` 启动后台服务进程（可需根据节点类型，仅选择启动部分模块）(**需要设置参数 `--no-bootstrap`**)
    - Node 0 - Bootstrap

        ```sh
        lynn@192.168.127.21 sia $ ./siad --modules=g --no-bootstrap --sia-directory=bootstrap
        Running with debugging enabled
        Sia Daemon v1.3.3
        Git Revision 4c73a7dd
        Loading...
        (0/1) Loading siad...
        (1/1) Loading gateway...
        Finished loading in 0.066843548 seconds
        ```

    - Node 1 - Miner

        ```sh
        lynn@192.168.127.21 sia $ ./siad --modules=gctwm --no-bootstrap --api-addr=localhost:9990 --rpc-addr=:9991 --sia-directory=miner
        Running with debugging enabled
        Sia Daemon v1.3.3
        Git Revision 4c73a7dd
        Loading...
        (0/5) Loading siad...
        (1/5) Loading gateway...
        (2/5) Loading consensus...
        (3/5) Loading transaction pool...
        (4/5) Loading wallet...
        (5/5) Loading miner...
        Finished loading in 0.163977087 seconds
        ```

    - Node 2 - Host

        ```sh
        lynn@192.168.127.22 sia $ ./siad --modules=gctwh --no-bootstrap --sia-directory=host
        ...
        (5/5) Loading host...
        Finished loading in 0.013928993 seconds
        ```

    - Node 3 - Renter

        ```sh
        lynn@192.168.127.23 sia $ ./siad --modules=gctwr --no-bootstrap --sia-directory=renter
        ...
        (5/5) Loading renter...
        Finished loading in 0.021479162 seconds
        ```

#### 加入网络

通过 *siac* 所提供的命令，与后台服务进程进行交互，调用其 *gateway* 模块提供的 API，加入 P2P 网络

1. 新建一个终端，通过 `ssh` 登陆相应节点
1. 进入保存 *siac* 的目录
1. 通过 `./siac --help` 查看相关帮助信息（可通过相关命令调用 *siad* 提供的 API）

    ```sh
    lynn@192.168.127.21 sia $ ./siac --help
    Sia Client v1.3.3

    Usage:
    ./siac [flags]
    ./siac [command]

    Available Commands:
    bash-completion Creates bash completion file.
    consensus       Print the current state of consensus
    gateway         Perform gateway actions
    help            Help about any command
    host            Perform host actions
    hostdb          Interact with the renter's host database.
    man-generation  Creates unix style manpages.
    miner           Perform miner actions
    renter          Perform renter actions
    stop            Stop the Sia daemon
    update          Update Sia
    version         Print version information
    wallet          Perform wallet actions

    Flags:
    -a, --addr string          which host/port to communicate with (i.e. the host/port siad is listening on) (default "localhost:9980")
        --apipassword string   the password for the API's http authentication
    -h, --help                 help for ./siac
        --useragent string     the useragent used by siac to connect to the daemon's API (default "Sia-Agent")

    Use "./siac [command] --help" for more information about a command.
    ```

1. 对于非 Bootstrap 节点（Miner、Host、Renter），通过 `./siac gateway connect 192.168.127.21:9981` 与 Bootstrap 节点建立连接，加入 P2P 网络
    - Node 1 - Miner

        ```sh
        lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 gateway connect 192.168.127.21:9981
        Added 192.168.127.21:9981 to peer list.
        lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 gateway list
        1 active peers:
        Version  Outbound  Address
        1.3.3    Yes       192.168.127.21:9981
        ```

    - Node 2 - Host

        ```sh
        lynn@192.168.127.22 sia $ ./siac gateway connect 192.168.127.21:9981
        Added 192.168.127.21:9981 to peer list.
        lynn@192.168.127.22 sia $ ./siac gateway list
        2 active peers:
        Version  Outbound  Address
        1.3.3    Yes       192.168.127.21:9981
        1.3.3    Yes       192.168.127.21:9991
        ```

    - Node 3 - Renter

        ```sh
        lynn@192.168.127.23 sia $ ./siac gateway connect 192.168.127.21:9981
        Added 192.168.127.21:9981 to peer list.
        lynn@192.168.127.23 sia $ ./siac gateway list
        3 active peers:
        Version  Outbound  Address
        1.3.3    Yes       192.168.127.21:9981
        1.3.3    Yes       192.168.127.21:9991
        1.3.3    No        192.168.127.22:9981
        ```

    > 依次将 miner、host、renter 与 Bootstrap 进行连接，新节点除连接 Bootstrap 节点外，也会连接网络中的其他节点

#### 钱包操作

##### 初始化钱包

对于使用了 *wallet* 模块的节点（Miner 保存挖矿的奖励、Host 开展共享存储服务前需提供抵押、Renter 使用共享存储前需支付预付款），通过 `./siac wallet init --password` 初始化钱包并设置密码

```sh
lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 wallet init --password
Wallet password:
Confirm:
Recovery seed:
awning myth blip hexagon identity goodbye dyslexic debut mammal actress pulp upstairs lobster autumn adult vehicle twofold stellar batch jittery essential pause onslaught deftly saxophone ringing remedy reef aggravate

Wallet encrypted with given password
```

> `--addr` 参数用于指定通信的 *siad* 后台服务进程（默认为 `localhost:9980`）

##### 解锁钱包

节点的相关操作要使用钱包，或对钱包进行任何操作前需要通过 `./siac wallet unlock` 解锁钱包，否则相关操作将无法正常工作，如 Miner 节点在开始首次挖矿前未解锁钱包,将无法收到挖矿奖励

```sh
lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 wallet unlock
Wallet password:
```

##### 创建账户地址

在需要进行[转账](#转账-miner---host)操作时，转账的接收方需要预先通过 `./siac wallet address` 为钱包创建新账户地址，并可通过 `./siac wallet addresses` 查看钱包中所有的账户地址

```sh
lynn@192.168.127.22 sia $ ./siac wallet address
Created new address: 17d05e536d6cd6ccfcfeb0688325464f69b75b960df6dd1a5caded822ee80f98e47654dd8181
lynn@192.168.127.22 sia $ ./siac wallet addresses
17d05e536d6cd6ccfcfeb0688325464f69b75b960df6dd1a5caded822ee80f98e47654dd8181
```

> 后续操作中由于只有 Host 节点和 Renter 节点为提供或使用共享存储支付 Siacoin，所以只有这两类节点需要手动创建账户地址；由于 Miner 节点可在钱包解锁后，在挖矿的同时自动生成接收奖励的账户地址，所以不需要手动创建账户地址

##### 查看余额

对于使用了 *wallet* 模块的节点，可通过 `./siac wallet balance` 查询钱包中的余额

```sh
lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 wallet balance
Wallet status:
Encrypted, Unlocked
Height:              0
Confirmed Balance:   0 H
Unconfirmed Delta:  +0 H
Exact:               0 H
Siafunds:            0 SF
Siafund Claims:      0 H

Estimated Fee:       30 mS / KB
```

#### 测试搭建的网络

##### 挖矿 Miner

Miner 节点通过挖矿获得 Siacoin，并向 Host 和 Renter 节点进行[转账](#转账-miner---host)，为[共享存储](#共享存储-host)和[文件上传下载](#文件上传下载-renter---host)提供交易支持，并对所产生的交易进行确认

1. Miner 节点通过 `./siac miner start` 运行 *miner* 模块，开始挖矿

    ```sh
    lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 miner start
    CPU Miner is now running.
    ```

1. Miner 节点通过 `./siac miner stop` 结束挖矿

    ```sh
    lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 miner stop
    Stopped mining.
    ```

1. 各节点（（Miner、Host、Renter）通过 `./siac consensus` 查看区块链同步情况（由于 Bootstrap 节点未启动 *consensus* 模块，所以不进行区块链同步，而其他节点的区块链完全同步，查询结果完全相同）

    ```sh
    lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 consensus
    Synced: Yes
    Block:      0000007573fc48b79136a60f5597fc34f34603c5e816a241d9b2390db18b0219
    Height:     500
    Target:     [0 0 1 5 17 132 33 158 236 175 6 108 67 120 9 5 174 115 191 213 7 247 6 193 127 240 191 222 39 32 139 209]
    Difficulty: 16451500
    ```

1. Miner 节点[查看钱包中的余额](#查看余额)

    ```sh
    lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 wallet balance
    Wallet status:
    Encrypted, Unlocked
    Height:              500
    Confirmed Balance:   146.9 MS
    Unconfirmed Delta:  +0 H
    Exact:               146879705000000000000000000000000 H
    Siafunds:            0 SF
    Siafund Claims:      0 H

    Estimated Fee:       30 mS / KB
    ```

##### 转账 Miner -> Host

Miner 节点将[挖矿](#挖矿-miner)获得的 Siacoin 转给 Host 节点，Host 节点将获得的 Siacoin 作为抵押，用于提供共享存储服务

1. Host 节点[创建账户地址](#创建账户地址)
1. Miner 节点通过 `./siac wallet send siacoins [amount] [dest]` 向 Host节点发送 Siacoin

    ```sh
    lynn@192.168.127.21 sia $ ./siac --addr=localhost:9990 wallet send siacoins 50MS 17d05e536d6cd6ccfcfeb0688325464f69b75b960df6dd1a5caded822ee80f98e47654dd8181
    Sent 50000000000000000000000000000000 hastings to 17d05e536d6cd6ccfcfeb0688325464f69b75b960df6dd1a5caded822ee80f98e47654dd8181
    ```

1. Host 节点通过 `./siac wallet balance` 查看钱包的余额，通过 `./siac wallet transactions` 查看相关转账交易信息
    - Miner 节点未开启挖矿时，交易未被记录到区块链中，转账未完成

        ```sh
        lynn@192.168.127.22 sia $ ./siac wallet balance
        Wallet status:
        Encrypted, Unlocked
        Height:              500
        Confirmed Balance:   0 H
        Unconfirmed Delta:  +50 MS
        Exact:               0 H
        Siafunds:            0 SF
        Siafund Claims:      0 H

        Estimated Fee:       30 mS / KB
        lynn@192.168.127.22 sia $ ./siac wallet transactions
                    [timestamp]    [height]                                                   [transaction id]    [net siacoins]   [net siafunds]
                    unconfirmed unconfirmed   b7b37e7293b9e17867b076f11441e3fbc2dbd5195da05f64e8dc40d235a0c06c    50000000.00 SC             0 SF
        ```

    - Miner 节点开启挖矿后，交易被确认，转账完成

        ```sh
        lynn@192.168.127.22 sia $ ./siac wallet balance
        Wallet status:
        Encrypted, Unlocked
        Height:              504
        Confirmed Balance:   50 MS
        Unconfirmed Delta:  +0 H
        Exact:               50000000000000000000000000000000 H
        Siafunds:            0 SF
        Siafund Claims:      0 H

        Estimated Fee:       30 mS / KB
        lynn@192.168.127.22 sia $ ./siac wallet transactions
                    [timestamp]    [height]                                                   [transaction id]    [net siacoins]   [net siafunds]
        2018-07-05 04:43:45+0800         501   b7b37e7293b9e17867b076f11441e3fbc2dbd5195da05f64e8dc40d235a0c06c    50000000.00 SC             0 SF
        ```

##### 共享存储 Host

Host 节点通过设置共享存储路径和大小，向网络宣布共享存储并接受合约，从而提供共享存储服务

1. Host 节点新建一个文件夹，作为共享存储时的数据存放路径

    ```sh
    lynn@192.168.127.22 sia $ mkdir share_storage
    ```

1. Host 节点通过 `./siac host folder add [path] [size]` 增加一个共享存储数据的存放路径，并设置共享空间大小

    ```sh
    lynn@192.168.127.22 sia $ ./siac host folder add ~/sia/share_storage 512MB
    Added folder /home/lynn/sia/share_storage
    ```

1. Host 节点通过 `./siac host announce [address]` 向网络宣布开始共享存储服务

    ```sh
    lynn@192.168.127.22 sia $ ./siac host announce 192.168.127.22:9982
    Host announcement submitted to network.
    The host has also been configured to accept contracts.
    To revert this, run:
        siac host config acceptingcontracts false
    ```

    > 通过检查 `Sia/modules/host/announce.go` 可知
    >
    > 1. 在内网测试中，执行 `./siac host announce` 最终将在 Siad 实例中调用上述源码文件中的 `func (h *Host) Announce() error` 方法，由于 `h.settings.NetAddress` 和 `h.autoAddress` 均返回 `""`，所以不会执行 `h.managedAnnounce(annAddr)`，无法完成宣布过程（不会报错)
    >
    > 2. 在内网测试中，使用官方提供的源码直接编译的版本，执行 `./siac host announce [address]` 最终将在 Siad 实例中调用上述源码文件中的 `func (h *Host) AnnounceAddress(addr modules.NetAddress) error` 方法，由于测试时提供的是内网地址，执行判断语句 `if addr.IsLocal()` 时无法通过，所以需要注释相关语句，然后通过修改后的源码重新[编译 *siad*](#编译源码)，并上传到相应 Host 节点后，执行 `./siac host announce [address]`
    >
    >     ```go
    >     //if addr.IsLocal() {
    >     //    return errors.New("announcement requested with local net address")
    >     //}
    >     ```

1. Host 节点通过 `./siac host` 查看共享存储的当前设置

    ```sh
    lynn@192.168.127.22 sia $ ./siac host
    Host info:
        Connectability Status: Host is not connectable (re-checks every few minutes).

        Storage:      503.32 MB (0 B used)
        Price:        50 SC / TB / Month
        Max Duration: 25 Weeks

        Accepting Contracts:  Yes
        Anticipated Revenue:  0 H
        Locked Collateral:    0 H
        Revenue:              0 H

    Storage Folders:
        Used    Capacity     % Used    Path
        0 B     503.32 MB    0.00      /home/lynn/sia/share_storage
    ```

1. Renter 节点通过 `./siac hostdb` 查看当前网络中宣布提供共享存储服务的 Host 节点信息

    ```sh
    lynn@192.168.127.23 sia $ ./siac hostdb
    1 Active Hosts:
        Address              Price (per TB per Mo)
    1:  192.168.127.22:9982  50 SC
    ```

##### 文件上传下载 Renter <-> Host

Renter 节点通过预先设置在文件上传下载过程中要支付给 Host 节点的津贴（预付款），从而获得共享存储服务

1. Renter 节点通过类似 [Miner 转账给 Host](#转账-miner---host) 的过程，从 Miner 节点获得 Siacoin
1. Renter 节点通过 `./siac renter prices` 查看网络中的共享存储价格

    ```sh
    lynn@192.168.127.23 sia $ ./siac renter prices
    Renter Prices (estimated):
    Fees for Creating a Set of Contracts:   3.96 SC
    Download 1 TB:                          25 SC
    Store 1 TB for 1 Month:                 150 SC
    Upload 1 TB:                            3 SC
    ```

1. Renter 节点通过 `./siac renter setallowance [amount] [period] [hosts] [renew window]` 设置使用 Host 节点提供的共享存储服务时，在一个给定周期内所能花费的 Siacoin 总数，即根据网络中的共享存储价格，在使用共享存储服务前所需支付的预付款

    ```sh
    lynn@192.168.127.23 sia $ ./siac renter setallowance 100SC 100b 1 10b
    Allowance updated.
    ```

    > 可通过 `./siac renter setallowance --help` 查看具体参数设置说明
    >
    > - `amount` -> `100SC` 设置在一个周期内最多能能花费100SC，即在一个共享存储服务使用周期内的预付款为100SC
    > - `period` -> `100b` 设置一个周期长度为100个区块
    > - `hosts` -> `1` 设置 Renter 节点的数据将分布在1个 Host 节点（由于测试网络中仅有一个 Host 节点）
    > - `renew windows` -> `10b` 设置自动更新津贴（预付款）的时间窗口为10个区块， Renter 和 Host 签订的合约在时间窗口达到后将自动更新
1. Renter 节点通过 `./siac renter upload [source] [path]` 上传文件，通过 `./siac renter list` 查看所有文件状态

    ```sh
    lynn@192.168.127.23 sia $ ./siac renter upload test_file test
    Uploaded '/home/lynn/sia/test_file' as test.
    lynn@192.168.127.23 sia $ ./siac renter list
    Tracking 1 files:
    Total uploaded:  12.09 MB
    12.09 MB  test
    ```

1. Host 节点通过 `./siac host` 查看共享存储空间使用情况

    ```sh
    lynn@192.168.127.22 sia $ ./siac host
    ...
    Storage Folders:
        Used        Capacity     % Used    Path
        12.32 MB    503.32 MB    2.45      /home/lynn/sia/share_storage
    ```

1. Renter 节点通过 `./siac renter download [path] [destination]` 下载文件

    ```sh
    lynn@192.168.127.23 sia $ ./siac renter download test test_file2
    Downloading...  95.4% of 12.09 MB, 1s elapsed, 61.42 Mbps
    Downloaded 'test' to /home/lynn/sia/test_file2.
    ```
