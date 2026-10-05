# Haveno CLI

The Haveno CLI (`haveno-cli`) is a command-line interface to a Haveno daemon. Each command calls the daemon's API, such as checking balances, browsing offers, or managing trades, and prints the result.

!!! info "Just want to trade?"
    If you only want to buy and sell XMR, see [Getting Started](getting-started.md). To build programs on top of Haveno, see [Haveno API](haveno-api.md).

## Requirements

- A running Haveno daemon (see [Start a daemon](haveno-api.md#start-a-daemon))

Building Haveno also builds the CLI, available as `./haveno-cli` in the project directory. It connects to the daemon directly, so no proxy is needed. On Windows, run `haveno-cli.bat` instead of `./haveno-cli`.

## Usage

```bash
./haveno-cli [options] <method> [params]
```

| Option | Description | Default |
| ------ | ----------- | ------- |
| `--host` | daemon hostname or IP | `localhost` |
| `--port` | daemon API port | `9998` |
| `--password` | daemon API password (required) | |

The password and port must match the daemon's `--apiPassword` and `--apiPort`.

## Examples

Start a daemon in one terminal, replacing `<your-api-password>` with a strong secret (see [Secure your daemon](haveno-api.md#start-a-daemon)):

```bash
./haveno-daemon --baseCurrencyNetwork=XMR_MAINNET --useLocalhostForP2P=false --useDevPrivilegeKeys=false \
  --nodePort=9999 --appName=Haveno --apiPassword=<your-api-password> --apiPort=1201 \
  --useNativeXmrWallet=false --ignoreLocalXmrNode=false
```

Enter a password when prompted to create or unlock your account. Without a console, use the `createaccount` or `openaccount` method instead.

Then use the CLI from another terminal:

```bash
# check the connection
./haveno-cli --port=1201 --password=<your-api-password> getversion

# get wallet balances
./haveno-cli --port=1201 --password=<your-api-password> getbalance

# get an address to deposit XMR
./haveno-cli --port=1201 --password=<your-api-password> getxmrprimaryaddress

# see open offers to sell XMR for USD
./haveno-cli --port=1201 --password=<your-api-password> getoffers --direction=sell --currency-code=USD
```

!!! note
    To use test funds instead, run `make haveno-daemon-stagenet` and connect with `--port=3204 --password=apitest`.

## Getting help

List all methods and options:

```bash
./haveno-cli --help
```

Get help for a specific method:

```bash
./haveno-cli --port=1201 --password=<your-api-password> takeoffer --help
```
