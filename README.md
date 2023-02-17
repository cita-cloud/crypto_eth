# crypto_eth
`CITA-Cloud`中[crypto微服务](https://github.com/cita-cloud/cita_cloud_proto/blob/master/protos/crypto.proto)的实现，采用跟以太坊一样的签名算法（`secp256k1`）和哈希算法(`keccak`)组合。
## 编译docker镜像
```
docker build -t citacloud/crypto_eth .
```
## 使用方法

```
$ crypto -h
crypto 6.6.3
Rivtower Technologies <contact@rivtower.com>

Usage: crypto <COMMAND>

Commands:
  run   run this service
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

### crypto-run

运行`crypto`服务。

```
$ crypto run -h
run this service

Usage: crypto run [OPTIONS]

Options:
  -c, --config <CONFIG_PATH>                 Chain config path [default: config.toml]
  -p, --private_key_path <PRIVATE_KEY_PATH>  private key path [default: private_key]
  -h, --help                                 Print help
```

参数：
1. 微服务配置文件。

    参见示例`example/config.toml`。

    其中`[crypto_eth]`段为微服务的配置：
    * `crypto_port` 为该服务监听的端口号。
    * `domain` 节点的域名

    其中`[crypto_eth.log_config]`段为微服务日志的配置：
    * `max_level` 日志等级
    * `filter` 日志过滤配置
    * `service_name` 服务名称，用作日志文件名与日志采集的服务名称
    * `rolling_file_path` 日志文件路径
    * `agent_endpoint` jaeger 采集端地址

2. 私钥文件路径。
    文件内容参见示例`example/private_key`。

```
$ crypto run -c example/config.toml -p example/private_key
2023-02-09T05:44:39.471368Z  INFO crypto: grpc port of crypto_eth: 60005
2023-02-09T05:44:39.471418Z  INFO crypto: start crypto_sm grpc server
```

## 设计

只是对签名算法（`secp256k1`）和哈希算法(`keccak`)的简单封装。
