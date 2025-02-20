# eunomia-bpf: 一个基于 WASM 的 CO-RE eBPF 程序动态加载框架

[![Actions Status](https://github.com/eunomia-bpf/eunomia-bpf/workflows/Ubuntu/badge.svg)](https://github.com/eunomia-bpf/eunomia-bpf/actions)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/eunomia-bpf/eunomia-bpf)](https://github.com/eunomia-bpf/eunomia-bpf/releases)
<!-- [![codecov](https://codecov.io/gh/eunomia-bpf/eunomia-bpf/branch/master/graph/badge.svg)](https://codecov.io/gh/filipdutescu/modern-cpp-template) -->

## 概况

在 `eunomia-bpf` 中，您可以:

- 在没有提供和管理框架的情况下运行 `CO-RE` eBPF代码
- 只编写eBPF内核代码并且将他编译为JSON，动态加载到另一台机器上而不需要重新编译
- 将eBPF程序编译为 `WASM` 模块，就可以在用户空间 `WASM` 运行时时控制eBPF程序或处理数据
- 拥有非常小和简单的可执行程序, 库本身小于 `1MB` 且不依赖 `LLVM/Clang`，可以轻松嵌入到您的项目中
- 以小于 `100ms` 的时间动态加载和运行任何eBPF程序，比 `bcc` 更迅速

## 项目架构

我们有一个加载器库，一个编译工具链，以及一些额外的工具，如cli和一个自定义指标导出器。

### 一个eunomia-bpf库

这个库包含了 `libbpf` 的主要函数，提供了将 `eBPF` 代码动态加载到内核的能力，并使用 `JSON` 和简单的 `API` 运行它。

查看 [eunomia-bpf](https://github.com/eunomia-bpf/eunomia-bpf/tree/master/ecli) 以获得更多细节信息

我们提供了[一个简单的cli接口](https://github.com/eunomia-bpf/eunomia-bpf/blob/master/ecli), 使得您可以通过在命令行输入URL的方式启动任何eBPF程序。您可以从[release](https://github.com/eunomia-bpf/eunomia-bpf/releases/)版本中下载样例:

```bash
# download the release from https://github.com/eunomia-bpf/eunomia-bpf/releases/latest/download/ecli
$ wget https://aka.pw/bpf-ecli -O ecli && chmod +x ./ecli
$ sudo ./ecli run https://eunomia-bpf.github.io/ebpm-template/package.json # simply run a pre-compiled ebpf code from a url
```

### 一个从WASM模块加载eBPF程序的库

使用 `eunomia-bpf` 库从`WASM`模块加载eBPF程序，您可以编写`WASM`模块来操作eBPF程序或在用户空间`WASM`运行时处理数据。这个想法很简单:

1. 使用 `eunomia-cc` 工具链将 `eBPF` 代码骨架编译成 `JSON` 格式
2. 在 `WASM` 模块中嵌入 `JSON` 数据，并为操作eBPF程序框架提供一些API
3. 从 `WASM` 模块加载 `JSON` 数据，并使用 `eunomia-bpf` 库运行eBPF程序框架

在单个 `WASM` 模块中可以有多个 `eBPF` 程序。

您可以在 [ewasm](https://github.com/eunomia-bpf/eunomia-bpf/blob/master/ewasm) 中看到更多细节

### 一个帮助您生成预编译eBPF数据的编译工具链

该工具链可以和docker一样使用，在一个命令中生成预编译的eBPF数据:
详细信息请参见 [eunomia-cc](eunomia-cc)。
您也可以简单地使用 [ebpm-template](https://github.com/eunomia-bpf/ebpm-template) 作为一个模板，将修改推送到这里后使用github action可以帮助您编译CO-RE ebpf代码!

### 一个可观测性工具

我们提供了一个 `prometheus` 和 `OpenTelemetry` 的输出工具用于定制eBPF指标，它用异步rust编写:[eunomia-exporter](https://github.com/eunomia-bpf/eunomia-bpf/blob/master/eunomia-exporter)

您可以编译它或从[release](https://github.com/eunomia-bpf/eunomia-bpf/releases/)版本下载它

### 其他相关项目

- LMP eBPF Hub: [github.com/linuxkerneltravel/lmp](github.com/linuxkerneltravel/lmp)
- bolipi online compiler & runner: [https://bolipi.com/ebpf/home/online](https://bolipi.com/ebpf/home/online)

## 工程构建

查看[build](https://github.com/eunomia-bpf/eunomia-bpf/blob/master/documents/build.md)以获得更多细节

## 计划路线图

- [X] 重构 `Eunomia` 项目中的代码并提供快速示例
- [X] 支持用户空间中的`tracepoints`、`fentry`、`kprobe`、`lsm`和`ring buffer`/`perf event`输出。
- [X] 使编译更容易使用，更灵活，完全兼容其他 libbpf 程序；
- [X] 添加可配置的可观测性导出器
- [X] 使用 WASM 进行 ebpf 包加载配置，并添加更多 ebpf 程序类型支持
- [ ] 支持更多的 ebpf 程序类型：
- [X] 为 eunomia-bpf 添加简单的包管理器: LMP
- [ ] 从 `libbpf` 添加更多可能性
- [ ] 提供 python、go 等 sdk
- [ ] 添加对 `etcd` 的支持并增强服务器
- [ ] 修复 ci 和 docs

## 证书

MIT LICENSE

