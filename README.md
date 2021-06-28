# Nano Quick Start

## Steps

1. Install Docker: https://docs.docker.com/engine/install/

2. Pull nano docker image:

```
docker pull nanocurrency/nano:V22.0
```

4. Set environment variables `NANO_HOST_DIR`, `NANO_NAME`, and `NANO_TAG` in ~/.profile

5. Run nano docker image:

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

6. Run a command, like get the node version:
```
$ curl -d '{ "action" : "version" }' [::1]:7076
```

## Commands
Block count:
```
$ curl -d '{ "action" : "block_count" }' [::1]:7076
```

Wallet Create
```
{
  "action": "wallet_create"
}
```

Account Create
```
{ 
  "action": "account_create",
  "wallet": "000D1BAEC8EC208142C99059B393051BAC8380F9B5A2E6B2489A277D81789F3F"
}
```