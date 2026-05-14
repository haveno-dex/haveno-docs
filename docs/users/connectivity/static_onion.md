# Static Tor Setup

Haveno can bind directly to a static onion service running on your system using the startup flag `--hiddenServiceAddress=your_onion_address.onion [--nodePort=9999]`.

This mode allows advanced features to be used in your `torrc` configuration file and is preferred for users who require DDoS hardening, utilize [Whonix or Qubes OS](./operating_systems.md), or prefer managing Tor as a system service.

??? Info "Optionally preserve your onion address"

    Using a static Tor instance will change the onion address used by Haveno, so peers will not recognize previous trades with your node.

    You can optionally preserve your onion address, by converting the private keys to have the same onion identity on both external and internal tor instances, using the following conversion script:

    ```
    { printf '== ed25519v1-secret: type0 ==\0\0\0'; sed '/^-----BEGIN OPENSSH PRIVATE KEY-----$/d;/^-----END OPENSSH PRIVATE KEY-----$/d' private_key | tr -d '\r\n' | base64 -d; } > hs_ed25519_secret_key && chmod 600 hs_ed25519_secret_key
    ```

## Configure `torrc`

The following `torrc` configuration is recommended for hardening against Denial of Servce (DoS) attacks.

This template includes aggressive rate limiting, PoW tuning, and restricted SOCKS policies.

??? note "Hardened `torrc` template"

    !!! Warning "Note"
        Modify `HiddenServiceDir` to a valid path on your system.

    ```text
    ## Configuration file for Haveno
    ##
    ## See 'man tor', for more options you can use in this file.

    ## NOTE: In order to use the ControlPort, the <user> must belong to the tor group.
    ##  sudo usermod -aG debian-tor <user>
    ##
    ## The port on which Tor will listen for local connections from Tor
    ## controller applications, as documented in control-spec.txt.
    #ControlPort 9051
    ## If you enable the controlport, be sure to enable one of these
    ## authentication methods, to prevent attackers from accessing it.
    ##
    ## Compute the hash of a password with "tor --hash-password password".
    #HashedControlPassword 16:872860B76453A77D60CA2BB8C1A7042072093276A3D701AD684053EC4C
    CookieAuthentication 0       # (Default: 1)

    ## Tor opens a socks proxy on port 9050 by default -- even if you don't
    ## configure one below. Set "SocksPort 0" if you plan to run Tor only
    ## as a relay, and not make any local application connections yourself.
    SocksPort 9050 OnionTrafficOnly ExtendedErrors

    ## Entry policies to allow/deny SOCKS requests based on IP address.
    ## First entry that matches wins. If no SocksPolicy is set, we accept
    ## all (and only) requests that reach a SocksPort. Untrusted users who
    ## can access your SocksPort may be able to learn about the connections
    ## you make.
    SocksPolicy accept 127.0.0.1
    SocksPolicy accept6 [::1]
    SocksPolicy reject *

    ## Tor will reject application connections that use unsafe variants of the socks protocol
    ## — ones that only provide an IP address, meaning the application is doing a DNS resolve first.
    ## Specifically, these are socks4 and socks5 when not doing remote DNS. (Default: 0)
    #SafeSocks 1

    ## Tor will make a notice-level log entry for each connection to the Socks port indicating
    ## whether the request used a safe socks protocol or an unsafe one (see above entry on SafeSocks).
    ## This helps to determine whether an application using Tor is possibly leaking DNS requests. (Default: 0)
    #TestSocks 1

    ## Logs go to stdout at level "notice" unless redirected by something
    ## else, like one of the below lines. You can have as many Log lines as
    ## you want.
    ##
    ## We advise using "notice" in most cases, since anything more verbose
    ## may provide sensitive information to an attacker who obtains the logs.
    ##
    ## Send all messages of level 'notice' or higher to /var/log/tor/notices.log
    #Log notice file /var/log/tor/notices.log
    ## Send every possible message to /var/log/tor/debug.log
    #Log debug file /var/log/tor/debug.log
    ## Use the system log instead of Tor's logfiles (This is default)
    #Log notice syslog
    ## To send all messages to stderr:
    #Log debug stderr

    # Try to write to disk less frequently than we would otherwise. This is useful when running on flash memory.
    AvoidDiskWrites 1

    #### Haveno incoming anonymity connections ###
    HiddenServiceDir /path/to/haveno_service
    HiddenServicePort 9999 127.0.0.1:9999
    HiddenServicePort 9999 [::1]:9999

    ## NOTE: HiddenService options are per onion service
    ## https://community.torproject.org/onion-services/advanced/dos/
    ##
    ## Rate limiting at the Introduction Points
    ## Intropoint protections prevents onion service DoS from becoming a DoS for the entire machine and its guard.
    HiddenServiceEnableIntroDoSDefense 1
    HiddenServiceEnableIntroDoSRatePerSec 25       # (Default: 25)
    HiddenServiceEnableIntroDoSBurstPerSec 200     # (Default: 200)

    # Number of introduction points the hidden service will have. You can’t have more than 20.
    #HiddenServiceNumIntroductionPoints 3           # (Default: 3)

    ## https://tpo.pages.torproject.net/onion-services/ecosystem/technology/pow/#configuring-an-onion-service-with-the-pow-protection
    ## Proof of Work (PoW) before establishing Rendezvous Circuits
    ## The lower the queue and burst rates, the higher the puzzle effort tends to be for users.
    HiddenServicePoWDefensesEnabled 1
    HiddenServicePoWQueueRate 50          # (Default: 250)
    HiddenServicePoWQueueBurst 250        # (Default: 2500)

    ## Stream limits in the established Rendezvous Circuits
    ## The maximum number of simultaneous streams (connections) per rendezvous circuit. The max value allowed is 65535. (0 = unlimited)
    HiddenServiceMaxStreams 25
    #HiddenServiceMaxStreamsCloseCircuit 1

    ## Optionally limit tor bandwidth
    #BandwidthRate 1000 KBytes
    #BandwidthBurst 2000 KBytes

    LongLivedPorts 9999
    ```

!!! Info "Note"
    Do not modify the internal `torrc` file inside the /xmr_mainnet/tor/ subdirectory. That file is used internally by Haveno and will be overwritten on every application startup.

Restart `tor` to apply your changes.

## Start Haveno

Start haveno with `--hiddenServiceAddress=your_address.onion [--nodePort=9999]` to bind directly to the static onion service.
