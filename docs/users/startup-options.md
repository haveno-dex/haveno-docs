# Startup Options

Haveno Desktop and the Haveno daemon accept the same startup options. They configure the data directory, Tor, the Monero node, the API and other settings needed before the app starts.

Most users never need them. Settings available in the app should be changed there instead.

## Setting options

### On the command line

Pass options as `--name=value` when launching Haveno:

=== "Linux"

    ```bash
    /opt/haveno/bin/Haveno --logLevel=DEBUG
    ```

=== "macOS"

    ```bash
    /Applications/Haveno.app/Contents/MacOS/Haveno --logLevel=DEBUG
    ```

=== "Windows"

    ```
    %LOCALAPPDATA%\Haveno\Haveno.exe --logLevel=DEBUG
    ```

    To use options every time, add them to the end of the **Target** field in the Haveno shortcut's **Properties**.

=== "Daemon"

    ```bash
    ./haveno-daemon --logLevel=DEBUG
    ```

Run with `--help` to print every option the installed version accepts.

### In a config file

Options can also be set in `haveno.properties`, inside the [application data directory](logs.md). Use one `name=value` per line, without the leading dashes or quotes. Lines starting with `#` are ignored.

```properties
# use a local Monero node
xmrNode=http://127.0.0.1:18081
logLevel=DEBUG
```

Command line values take precedence over the config file. `--help` and `--configFile` are accepted only on the command line, and `appName`, `userDataDir` and `appDataDir` are ignored in the default config file, since it lives inside the data directory they select. Haveno may also write entries to this file itself, such as banned nodes received from the network.

## Application

| Option | Default | Description |
|--------|---------|-------------|
| `--help` | | Print all options and exit. |
| `--configFile` | `haveno.properties` | Config file to read. Relative paths are resolved against `appDataDir`. |
| `--appName` | `Haveno` | Application name, which also names the data directory. Use a different name to run separate instances. |
| `--userDataDir` | OS user data directory | Parent directory of the application data directory. |
| `--appDataDir` | `userDataDir/appName` | Directory for wallets, keys, logs and settings. |
| `--baseCurrencyNetwork` | `XMR_MAINNET` | Network to use: `XMR_MAINNET`, `XMR_STAGENET` or `XMR_LOCAL`. |
| `--logLevel` | `INFO` | `OFF`, `ERROR`, `WARN`, `INFO`, `DEBUG`, `TRACE` or `ALL`. |
| `--passwordRequired` | `false` | Require a password when creating an account. |
| `--dumpHeapOnOutOfMemoryError` | `false` | Write a Java heap dump to the data directory on out of memory errors. Dumps may contain sensitive data and use substantial disk space. |
| `--maxMemory` | `1200` | Memory limit in MB for headless nodes such as seed nodes, which shut down when it is exceeded. |

## Monero

| Option | Default | Description |
|--------|---------|-------------|
| `--xmrNode` | | Monero node to connect to, e.g. `http://127.0.0.1:18081`. Locks node settings in the app. |
| `--xmrNodeUsername` | | Username for `--xmrNode`. |
| `--xmrNodePassword` | | Password for `--xmrNode`. |
| `--xmrNodes` | | Custom Monero nodes as `host[:port][,...]`. Locks node settings in the app. |
| `--ignoreLocalXmrNode` | `false` | Ignore a Monero node running on this machine. |
| `--useTorForXmr` | `after_sync` | Use Tor for Monero connections: `after_sync`, `on` or `off`. Overrides the setting in the app. |
| `--xmrStreamIsolation` | `false` | Sync each Monero wallet over its own Tor circuit. |
| `--socks5ProxyXmrAddress` | | SOCKS5 proxy (`host:port`) for Monero connections instead of Haveno's Tor. Also used for HTTP requests unless `--socks5ProxyHttpAddress` is set. |
| `--useNativeXmrWallet` | `false` | Use the native Monero wallet library instead of `monero-wallet-rpc`. Overrides the setting in the app. |
| `--walletRpcBindPort` | automatic | Port for the main wallet's `monero-wallet-rpc` process. |
| `--xmrBlockchainPath` | | Blockchain directory for a local Monero node started by Haveno. Overrides the setting in the app. |
| `--updateXmrBinaries` | `true` | Replace the Monero binaries in the data directory when they differ from the bundled ones. |

## Haveno network

| Option | Default | Description |
|--------|---------|-------------|
| `--nodePort` | `9999` | Port this node listens on for peer connections. |
| `--hiddenServiceAddress` | | Existing onion address to bind to directly. See [Hardened Tor Setup](connectivity/hardened-tor.md). |
| `--seedNodes` | | Seed nodes (`host:port[,...]`) to use instead of the built in ones. |
| `--bannedSeedNodes` | | Seed nodes (`host:port[,...]`) to avoid. |
| `--banList` | | Peers (`host:port[,...]`) to refuse connections with. |
| `--maxConnections` | `12` | Number of peer connections to maintain. |
| `--providers` | | Price relay nodes (`host:port[,...]`) to use instead of the built in ones. |
| `--bannedPriceRelayNodes` | | Price relay nodes (`host:port[,...]`) to avoid. |
| `--socks5ProxyHttpAddress` | | SOCKS5 proxy (`host:port`) for HTTP requests such as price data. Should not be Tor. |
| `--republishMailboxEntries` | `false` | Republish stored mailbox messages at startup. |
| `--useLocalhostForP2P` | `false` | Connect to peers over localhost instead of Tor. Not available on mainnet. |

## Tor

See [Tor Overview](connectivity/tor-overview.md) for how these fit together.

| Option | Default | Description |
|--------|---------|-------------|
| `--torrcFile` | | Existing torrc file for the bundled Tor. Entries Haveno depends on cannot be overridden. |
| `--torrcOptions` | | Extra torrc entries for the bundled Tor, e.g. `"NumCPUs 0, Log notice file tor.log"`. |
| `--hiddenServiceFlags` | | Flags for the onion service added with `ADD_ONION`. |
| `--hiddenServiceParams` | | Parameters for the onion service added with `ADD_ONION`, e.g. PoW defenses. |
| `--torControlHost` | `127.0.0.1` | Control host of an external Tor. |
| `--torControlPort` | | Control port of an external Tor, used instead of the bundled one. Cannot be combined with `--torrcFile` or `--torrcOptions`. |
| `--torControlPassword` | | Password for the external Tor's control port. |
| `--torControlCookieFile` | | Cookie file for the external Tor's control port. Cannot be combined with `--torControlPassword`. |
| `--torControlUseSafeCookieAuth` | | Flag without a value. Use SafeCookie authentication with `--torControlCookieFile`. |
| `--torStreamIsolation` | | Flag without a value. Use a separate Tor circuit per outbound peer connection. Experimental. |

## API

These apply to the [Haveno daemon](haveno-api.md).

| Option | Default | Description |
|--------|---------|-------------|
| `--apiPassword` | | Password required by API clients. |
| `--apiPort` | `9998` | API port. |
| `--apiBindAddress` | `127.0.0.1` | Interface the API listens on. Anything but loopback exposes full wallet access to that network. |
| `--apiHiddenService` | `false` | Also publish the API as a Tor onion service. With the bundled Tor, it starts after account login. |
| `--apiHiddenServiceBeforeLogin` | `false` | Publish the API onion service before login, using the startup Tor configuration. Bridge settings saved in the app are not applied. |
| `--apiHiddenServicePort` | `--apiPort` | Port of the API onion service. |
| `--disableRateLimits` | `false` | Disable API rate limits. |

## Rate limits

See [DoS Protection](connectivity/dos-protection.md) for the envelope limit formats and defaults.

| Option | Default | Description |
|--------|---------|-------------|
| `--envelopeLimitsGlobalDefault` | `0` | Global limit for all message types. |
| `--envelopeLimitsGlobalUnknownPeers` | `50,10000,0` | Global limit for messages from unknown peers. |
| `--envelopeLimitsGlobalOverrides` | | Global limits for specific message types. |
| `--envelopeLimitsConnectionDefault` | `0` | Limit per peer connection. |
| `--envelopeLimitsConnectionOverrides` | | Per connection limits for specific message types. |
| `--msgThrottlePerSec` | `200` | Messages accepted per second from a connection. |
| `--msgThrottlePer10Sec` | `1000` | Messages accepted per 10 seconds from a connection. |
| `--sendMsgThrottleTrigger` | `20` | Interval in ms between two sent messages below which sending pauses. |
| `--sendMsgThrottleSleep` | `50` | Pause in ms when sending too fast. |

## Development and operators

| Option | Default | Description |
|--------|---------|-------------|
| `--useDevMode` | `false` | Enable conveniences for developer testing. |
| `--useDevModeHeader` | `false` | Use a distinct color scheme to tell development instances apart. |
| `--useDevPrivilegeKeys` | `false` | Use development keys for privileged features such as filters and alerts. For development networks only. |
| `--ignoreDevMsg` | `false` | Ignore signed alerts, update notices and filters. |
| `--dumpStatistics` | `false` | Write trade statistics and offers as JSON files to the data directory. |
| `--preventPeriodicShutdownAtSeedNode` | `false` | Disable the periodic restart of seed nodes. |

Some options listed by `--help`, such as `--referralId`, `--numConnectionsForBtc` and `--socks5DiscoverMode`, are inherited from Bisq and have no effect in Haveno.
