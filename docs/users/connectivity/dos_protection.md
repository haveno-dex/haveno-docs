# Denial of Service (DoS) Protection

If your application is under sustained Denial of Service (DoS) attack, binding to a [hardened Tor](./hardened_tor.md) instance is highly recommended to resolve most issues.

## Envelope Throttling

While Tor handles network-layer security, Haveno also uses granular envelope throttling to protect the application layer.

If a peer sends messages faster than your defined rates, Haveno will drop the excess traffic to ensure responsiveness. This prevents your node from being overwhelmed by a flood of encrypted messaging data ("envelopes").

Rate limiting uses a three-part configuration: `ratePerSec`, `burstCapacity`, and `numStrikes` (the number of violations allowed before a peer is disconnected).

!!! info "The Leaky Bucket Analogy"
    Imagine a bucket that can hold 50 units of water (Burst Capacity). The bucket has a small hole in the bottom that leaks at 1 unit per second (Rate per Second).
    
    Incoming messages are like pouring water into the bucket. If the water ever overflows the 50-unit capacity, the node records a Strike. This system allows for short bursts of activity while enforcing a steady, sustainable pace over time.

### `--envelopeLimitsGlobalDefault`
**Default:** `0`  
Default global rate limits for all envelope types. Set to `0` for no limit.  
*Format: ratePerSec, burstCapacity, numStrikes*

### `--envelopeLimitsGlobalUnknownPeers`
**Default:** `50,10000,0`  
Global rate limits applied only to messages from unknown peers.  
*Format: ratePerSec, burstCapacity, numStrikes*

### `--envelopeLimitsGlobalOverrides`
**Default:** `None`  
Global rate limit overrides for specific envelope types.  
*Format: EnvelopeName=ratePerSec, burstCapacity, numStrikes[;...]*

### `--envelopeLimitsConnectionDefault`
**Default:** `0`  
Default rate limits applied per individual peer connection. Set to `0` for no limit.  
*Format: ratePerSec, burstCapacity, numStrikes*

### `--envelopeLimitsConnectionOverrides`
**Default:** `None`  
Per-connection rate limit overrides for specific envelope types.  
*Format: EnvelopeName=ratePerSec, burstCapacity, numStrikes[;...]*

---

## Default Configuration

Haveno is pre-configured with the following baseline settings to mitigate known attack vectors. These values are applied by default and only need to be specified if you wish to override them:

```bash
--envelopeLimitsGlobalDefault="0" \
--envelopeLimitsGlobalUnknownPeers="50,10000,0" \
--envelopeLimitsGlobalOverrides="GetPeersRequest=0.5,5,0;Ping=2,5,0" \
--envelopeLimitsConnectionDefault="0" \
--envelopeLimitsConnectionOverrides="GetPeersRequest=0.0167,2,0;Ping=0.0333,2,0"
```
