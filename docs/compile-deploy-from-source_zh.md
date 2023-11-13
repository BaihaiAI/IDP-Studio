<div align="right">

  [English](compile-deploy-from-source.md) | [简体中文](compile-deploy-from-source_zh.md)

</div>


# 通过源代码编译和部署IDP

## 环境要求

需要Git、Rustup(1.17.0及以上)、Python 3（v3.8及以上）、Node.js active LTS（v16.17.0）、yarn和npm（v8及以上）。

### Linux操作系统所需额外构建依赖项

如果你使用的是Ubuntu/Debian操作系统，请另外安装：

```shell
> apt install build-essential python3-dev python3-pip openssl git libgit2-dev
```

如果你使用的是Fedora/Centos操作系统，请另外安装：

```shell
> dnf install base-devel python3-devel python3-pip openssl-devel git libgit2-devel
```

### Windows操作系统所需额外构建依赖项

一定要使用x86_64-pc-windows-msvc target，使用windows-gnu target进行编译不会成功。

按照[Visual Studio guide to install msvc](https://learn.microsoft.com/en-us/windows/dev-environment/rust/setup#install-visual-studio-recommended-or-the-microsoft-c-build-tools)进行操作。

### macOS操作系统操作指南

如果你使用macOS操作系统，请不要使用操作系统自带的python3，因为它是静态链接，没有dylib。可以从conda/miniconda3/brew安装带有dylib的python3。

复制.cargo/config.toml，并将其放置到python安装目录。

```shell
> cp .cargo/config.toml.example .cargo/config.toml
```

#### 安装Rust

```shell
> curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
> source "$HOME/.cargo/env"
```

#### Homebrew环境下Python的配置

编辑.cargo/config.toml文件：

```toml
[target.x86_64-apple-darwin]
rustflags = ["-L", "/opt/homebrew/lib/", "-C", "link-arg=-undefined", "-C", "link-arg=dynamic_lookup"]
[env]
PYO3_PYTHON="/opt/homebrew/bin/python3"
```

#### macOS conda环境下Python的配置

按照如下所示修改.cargo/config.toml文件：
（注意，需要用终端的 "echo $CONDA_PREFIX "的输出来替换 "CONDA_PREFIX"。）

```toml
[target.x86_64-apple-darwin]
rustflags = ["-C", "link-arg=-undefined", "-C", "link-arg=dynamic_lookup", "-C", "link-arg=-Wl,-rpath,`CONDA_PREFIX`/lib"]
```

## 配置rust-toolchain.toml

### 查看rust版本

```shell
> cd ~&rustc -V
> rustc 1.73.0 (cc66ad468 2023-10-03)
```

修改channel为1.73.0
```toml
[toolchain]
channel = "1.73.0"
components = ["rustfmt", "clippy"]
targets = ["x86_64-unknown-linux-musl"]
```

## 编译和运行

```shell
> ./scripts/build.sh
> ./target/release/idp
```

---

## 构建Docker镜像
```shell
> docker build -t note -f Dockerfile --target release .
```
