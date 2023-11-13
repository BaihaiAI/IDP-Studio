<div align="right">

  [English](compile-deploy-from-source.md) | [简体中文](compile-deploy-from-source_zh.md)

</div>


# Compile and Deploy IDP From Source

## System Requirements

You will need Git, Rustup(1.70.0 or newer), Python 3(v3.8+), the Node.js active LTS (v16.17.0), yarn, and npm (v8+). 

### Linux additional build dependencies

If you are using Ubuntu/Debian, please additionally install:

```shell
> apt install build-essential python3-dev python3-pip openssl git libgit2-dev
```
If you are using Fedora/Centos, please additionally install:

```shell
> dnf install base-devel python3-devel python3-pip openssl-devel git libgit2-devel
```

### Windows additional build dependencies

x86_64-pc-windows-msvc target is required. Compiling with windows-gnu target will not be successful.

Follow [Visual Studio guide to install msvc](https://learn.microsoft.com/en-us/windows/dev-environment/rust/setup#install-visual-studio-recommended-or-the-microsoft-c-build-tools)

### macOS

For macOS，you can't use the python3 that comes with the system because it's static linked and no dylib. You can install python3 with dylib from conda/miniconda3/brew.

copy .cargo/config.toml and edit it to where your python installed
```shell
> cp .cargo/config.toml.example .cargo/config.toml
```

#### Install Rust

```shell
> curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
> source "$HOME/.cargo/env"
```

#### Homebrew's Python

edit .cargo/config.toml

```toml
[target.x86_64-apple-darwin]
rustflags = ["-L", "/opt/homebrew/lib/", "-C", "link-arg=-undefined", "-C", "link-arg=dynamic_lookup"]
[env]
PYO3_PYTHON="/opt/homebrew/bin/python3"
```

#### macOS conda's python

Modify .cargo/config.toml as follows:
(note that you need to _replace_ `CONDA_PREFIX` with
the output of `echo $CONDA_PREFIX` from your terminal.)

```toml
[target.x86_64-apple-darwin]
rustflags = ["-C", "link-arg=-undefined", "-C", "link-arg=dynamic_lookup", "-C", "link-arg=-Wl,-rpath,`CONDA_PREFIX`/lib"]
```

## rust-toolchain.toml configuration

### check rust version

```shell
> cd ~&rustc -V
> rustc 1.73.0 (cc66ad468 2023-10-03)
```

change channel to your's rust version, eg: 1.73.0
```toml
[toolchain]
channel = "1.73.0"
components = ["rustfmt", "clippy"]
targets = ["x86_64-unknown-linux-musl"]
```

## Compile and Run

```shell
> ./scripts/build.sh
> ./target/release/idp
```

---

## Build Docker Image
```shell
> docker build -t note -f Dockerfile --target release .
```
