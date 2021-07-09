# Nano Quick Start

1. Install Docker: https://docs.docker.com/engine/install/

2. Pull nano docker image:

```
docker pull nanocurrency/nano:V22.0
```

4. Set environment variables `NANO_HOST_DIR`, `NANO_NAME`, and `NANO_TAG` in `~/.profile`. For example:

```
export NANO_HOST_DIR=~/Desktop/nano
export NANO_NAME=nano-test
export NANO_TAG=V22.0
```

5. Many RPC commands require `enable_control` to be set to `true`. Reference: [Running a Node/Configuration](https://docs.nano.org/running-a-node/configuration/). Set this setting to `true` in the `Nano/config-rpc.toml` file:

```
root@ubuntu:/home/z/Desktop/nano/Nano# cat config-rpc.toml 

# Bind address for the RPC server
# type:string,ip
address = "::ffff:0.0.0.0"

# Enable or disable control-level requests
# type:bool
enable_control = true
```

6. Run nano docker image:

```
docker run --restart=unless-stopped -d \
  -p 7075:7075/udp \
  -p 7075:7075 \
  -p [::1]:7076:7076 \
  -p [::1]:7078:7078 \
  -v ${NANO_HOST_DIR}:/root \
  --name ${NANO_NAME} \
  nanocurrency/nano:${NANO_TAG}
```

7. Run a command, like get the node version:
```
curl -d '{ "action" : "version" }' [::1]:7076
```

# RPC Commands

List of Commands: https://docs.nano.org/commands/rpc-protocol/

## Block Count
```
curl -d '{ "action" : "block_count" }' [::1]:7076
```
Response
```
{
    "count": "7800",
    "unchecked": "9264e113",
    "cemented": "4727"
}
```

## Wallet Create

```
{
  "action": "wallet_create"
}
```

Response:
```
{
    "wallet": "0B580F6660304000188FDCDD395BD40AAD9FA666DDE5A0EEFF874ED82E263F24"
}
```

## Account Create

```
{ 
  "action": "account_create",
  "wallet": "0B580F6660304000188FDCDD395BD40AAD9FA666DDE5A0EEFF874ED82E263F24"
}
```

Response:
```
{
    "account": "nano_187cgr9yqbh67mytbu3n7opx7wx9qtty1qx9xfeinoc654z7iefiyjp7m6h9"
}
```

## Account Balance
```
{
  "action": "account_balance",
  "account": "nano_3e3j5tkog48pnny9dmfzj1r16pg8t1e76dz5tmac6iq689wyjfpi00000000"
}
```

Response:
```
{
    "balance": "0",
    "pending": "0"
}
```

## Send
```
{
  "action": "send",
  "wallet": "000D1BAEC8EC208142C99059B393051BAC8380F9B5A2E6B2489A277D81789F3F",
  "source": "nano_3e3j5tkog48pnny9dmfzj1r16pg8t1e76dz5tmac6iq689wyjfpi00000000",
  "destination": "nano_3e3j5tkog48pnny9dmfzj1r16pg8t1e76dz5tmac6iq689wyjfpi00000000",
  "amount": "1000000",
  "id": "your-unique-id"
}
```

# CLI Commands

List of Commands: https://docs.nano.org/commands/command-line-interface/

To drop into a shell use command:
```
docker exec -ti nano-test bash
```
where `nano-test` is the name of the docker container.

Then run any of the CLI commands listed in the docs. For example:
```
nano_node --help
nano_node --version
nano_node --validate-blocks
```