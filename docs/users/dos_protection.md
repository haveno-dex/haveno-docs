# Denial of Service (DoS) Protection

To protect against Denial of Service (DoS) attacks, Haveno uses adaptive Proof of Work (PoW) challenges and granular peer throttling to maintain network reliability.

## Configuration Parameters

The following startup flags allow users to harden their client against resource exhaustion while maintaining connectivity for legitimate peers.

Most limits use a three-part configuration: `ratePerSec`, `burstCapacity`, and `numStrikes` (the number of violations allowed before a peer is disconnected).

!!! info "The Leaky Bucket Analogy"
    Imagine a bucket that can hold 50 units of water (Burst Capacity). The bucket has a small hole in the bottom that leaks at 1 unit per second (Rate per Second).
    
    Incoming messages are like pouring water into the bucket. If the water ever overflows the 50-unit capacity, the node records a Strike. This system allows for short bursts of activity while enforcing a steady, sustainable pace over time.

### `hiddenServiceParams`
**Default:** `PoWDefensesEnabled=1,PoWQueueRate=10,PoWQueueBurst=100`  
Passes configuration directly to the `ADD_ONION` dynamic service to enable and tune Proof of Work defense.

### `envelopeLimitsGlobalDefault`
**Default:** `0`  
Default global rate limits for all envelope types. Set to `0` for no limit.  
*Format: ratePerSec, burstCapacity, numStrikes*

### `envelopeLimitsGlobalUnknownPeers`
**Default:** `50,10000,0`  
Global rate limits applied only to messages from unknown peers.  
*Format: ratePerSec, burstCapacity, numStrikes*

### `envelopeLimitsGlobalOverrides`
**Default:** `None`  
Global rate limit overrides for specific envelope types.  
*Format: EnvelopeName=ratePerSec, burstCapacity, numStrikes[;...]*

### `envelopeLimitsConnectionDefault`
**Default:** `0`  
Default rate limits applied per individual peer connection. Set to `0` for no limit.  
*Format: ratePerSec, burstCapacity, numStrikes*

### `envelopeLimitsConnectionOverrides`
**Default:** `None`  
Per-connection rate limit overrides for specific envelope types.  
*Format: EnvelopeName=ratePerSec, burstCapacity, numStrikes[;...]*

### `torrcOptions`
**Default:** `NumCPUs 0` (use all available CPUs)
A list of entries to amend to Haveno's internal `torrc`. Note that critical system entries cannot be overwritten.  
*Format: Option Value[; ...]*

---

## Configuration Example

The following baseline configuration mitigates known attack vectors and provides a resilient starting point for users:

```bash
--hiddenServiceParams="PoWDefensesEnabled=1,PoWQueueRate=10,PoWQueueBurst=100" \
--torrcOptions="NumCPUs 0" \
--envelopeLimitsGlobalDefault="0" \
--envelopeLimitsGlobalUnknownPeers="50,10000,0" \
--envelopeLimitsGlobalOverrides="GetPeersRequest=0.5,5,0;Ping=2,5,0" \
--envelopeLimitsConnectionDefault="0" \
--envelopeLimitsConnectionOverrides="GetPeersRequest=0.0167,2,0;Ping=0.0333,2,0"
```
