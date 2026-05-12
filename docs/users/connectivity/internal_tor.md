# Using Haveno with internal Tor

By default, Haveno includes an integrated Tor binary. This provides a seamless experience that requires no external dependencies or manual configuration.

!!! warning "Advanced DDoS Configuration"
    The internal configuration is optimized for standard use. If your node experiences sustained DDoS attacks, we recommend transitioning to an [External Tor](./external_tor.md) setup for more robust hardening.

!!! note "Note"
    Editing the internal torrc in /xmr_mainnet/tor has no effect, as it will be overwritten. All configuration for internal tor must be specified via Haveno command line parameters.

## Supported configuration

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
