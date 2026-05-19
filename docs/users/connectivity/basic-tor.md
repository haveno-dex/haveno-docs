# Basic Tor Setup

By default, Haveno manages a dynamic onion service automatically.

Basic configuration is supported if using the default Tor setup, or if connecting via a control port (e.g. `--torControlPort=9051`).

To use more advanced features in `torrc`, or if your node is experiencing a sustained Denial of Service (DoS) attack, connect to a [hardened Tor](./hardened-tor.md) instance instead.

## Basic configuration

### `--hiddenServiceParams`
**Default:** `PoWDefensesEnabled=1,PoWQueueRate=10,PoWQueueBurst=100`  
Passes configuration directly to the `ADD_ONION` dynamic service to enable and tune Proof of Work defense.

### `--hiddenServiceFlags`
**Default:** `""`  
Passes behavior modifiers directly to the ADD_ONION command. These are binary or state-based toggles rather than key-value pairs.

### `--torrcOptions`
**Default:** `NumCPUs 0` (use all available CPUs)   
A list of entries to amend to Haveno's internal `torrc`. Note that critical system entries cannot be overwritten.  
*Format: Option Value[; ...]*

## Example configuration

```
--hiddenServiceParams="PoWDefensesEnabled=1,PoWQueueRate=10,PoWQueueBurst=100" \
--hiddenServiceFlags="" \
--torrcOptions="NumCPUs 0" \
```
