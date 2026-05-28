# Configure Tor for Your Operating System

This guide describes how to use a [static onion service](./hardened-tor.md) for specific operating systems.

??? Info "Optionally preserve your onion address"

    Using a static Tor instance will change the onion address used by Haveno, so peers will not recognize previous trades with your node.

    You can optionally preserve your onion address, by converting the private keys to have the same onion identity on both external and internal tor instances, using the following conversion script:

    ```
    { printf '== ed25519v1-secret: type0 ==\0\0\0'; sed '/^-----BEGIN OPENSSH PRIVATE KEY-----$/d;/^-----END OPENSSH PRIVATE KEY-----$/d' private_key | tr -d '\r\n' | base64 -d; } > hs_ed25519_secret_key && chmod 600 hs_ed25519_secret_key
    ```

## Debian

Ubuntu and other Debian based as well as almost any OS with systemd

#### 1. Configure a HiddenService in: `/etc/tor/torrc`

```
# Haveno incoming anonymity connections
HiddenServiceDir /var/lib/tor/haveno_service/
HiddenServicePort 9999 127.0.0.1:9999
HiddenServicePort 9999 [::1]:9999
# Rate limiting at the Introduction Points (protects the entire Tor network)
HiddenServiceEnableIntroDoSDefense 1
```

??? info "HiddenService options"
    Not needed for Haveno-desktop daemon. Useful for seednodes or your own monerod.<br>
    ```
    # HiddenService options are per onion service:
    ##
    ## Rate limiting at the Introduction Points
    HiddenServiceEnableIntroDoSDefense 1
    #HiddenServiceEnableIntroDoSRatePerSec 25       # (Default: 25)
    #HiddenServiceEnableIntroDoSBurstPerSec 200     # (Default: 200)

    # Number of introduction points the hidden service will have. You can’t have more than 20.
    #HiddenServiceNumIntroductionPoints 3           # (Default: 3)

    ## https://onionservices.torproject.org/technology/pow/#configuring-an-onion-service-with-the-pow-protection
    ## Proof of Work (PoW) before establishing Rendezvous Circuits
    ## The lower the queue and burst rates, the higher the puzzle effort tends to be for users.
    HiddenServicePoWDefensesEnabled 1
    #HiddenServicePoWQueueRate 200          # (Default: 250)
    #HiddenServicePoWQueueBurst 1000        # (Default: 2500)
    #CompiledProofOfWorkHash auto           # (Default: auto)

    ## Stream limits in the established Rendezvous Circuits
    HiddenServiceMaxStreams 10
    HiddenServiceMaxStreamsCloseCircuit 1
    ```

- Reload Tor config to create the HiddenService with: `sudo systemctl reload tor`
- Get *Your_HiddenService_address*: `sudo cat /var/lib/tor/haveno_service/hostname`

#### 2. Start Haveno with *Your_HiddenService_address*

`/opt/haveno/bin/Haveno --hiddenServiceAddress=Your_HiddenService_address.onion [--nodePort=9999]`

## Whonix

On Whonix systems we need to configure 2 files. In the different Whonix types, the two files to be edited are in different places. Further details please see the two Whonix Wiki links.<br>
If you use Qubes-Whonix, read there how to get your `TARGET` IP! `qubesdb-read /qubes-ip`

1. [Create a HiddenService on Whonix-Gateway](https://www.whonix.org/wiki/Onion_Services#Step_2:_Edit_Tor_Configuration)
2. [Open Whonix-Workstation Firewall Port 9999](https://www.whonix.org/wiki/Onion_Services#Step_2:_Open_Whonix-Workstation_Firewall_Port)

### 1. Create a HiddenService on the Whonix-Gateway

File paths are of non-Qubes Whonix running in VirtualBox or KVM - Whonix with Xfce graphical user interface (GUI)

!!! info
    There is a provided `Tor Examples` Button for *torrc.examples* &<br>
    `Tor User Config` in the Whisker Menu `Application` -> `System`<br>
    Please open *torrc.examples* in your Whonix VM and check the IP in web server example!<br>
    You may need to adjust the TARGET IP<br>
    You can use the `Tor User Config` Button or `sudoedit` in Terminal to edit `50_user.conf`

`sudoedit /usr/local/etc/torrc.d/50_user.conf`

```
# Haveno incoming anonymity connections
HiddenServiceDir /var/lib/tor/haveno_service/
HiddenServicePort 9999 10.152.152.11:9999
# Rate limiting at the Introduction Points (protects the entire Tor network)
HiddenServiceEnableIntroDoSDefense 1
```
and save the file.

??? info
    `HiddenServiceVersion 3` as in the examples of the Whonix wiki is **not** required, this is the Tor default.<br>
    Hidden (Onion) services version 2 is deprecated and is no longer supported since the 0.4.6.1-alpha Tor release, in 2021!

- Reload Tor config to create the HiddenService with: `sudo systemctl reload tor`<br>
Alternatively, there's even a GUI button for: `Reload Tor`
- Get *Your_HiddenService_address* with: `sudo cat /var/lib/tor/haveno_service/hostname`
- **Copy it for your Whonix-Workstation.**

Whonix-Gateway is ready, switch to Whonix-Workstation.

### 2. Edit Whonix-Workstation firewall configuration to open port 9999

!!! info
    There is `Global Firewall Settings` in the Whisker Menu `Application` -> `System` whith examples & notes.<br>
    You can use the `User Firewall Settings` Button or `sudoedit` in Terminal to edit `50_user.conf`

`sudoedit /etc/whonix_firewall.d/50_user.conf`

```
# Open TCP port on all network interfaces,
# gateway as well as (if any) tunnel (VPN) interfaces.
EXTERNAL_OPEN_PORTS+=" 9999 "
```
and save the file.

Reload Whonix Firewall using: `sudo /usr/bin/whonix_firewall`<br>
There's even a GUI button for: `Reload Firewall` ;-)


That was all to configure a HiddenService for our Haveno app in Whonix.

### 3. Download & Run Haveno on Whonix-Workstation

!!! warning
    The official Haveno repository does not operate or endorse any mainnet network.<br>
    To make real trades with Haveno, first find a third party network, and then use their installer or build their repository. We do not endorse any networks at this time.

There is an installation guide in the Whonix Wiki for a Haveno main network:
[Downloading_&_Installing_RetoSwap](https://www.whonix.org/wiki/RetoSwap#Downloading_&_Installing_RetoSwap)

Haveno Launcher should be in `Applications` -> `Internet` You must edit it to:<br>
`/opt/haveno/bin/Haveno --hiddenServiceAddress=Your_HiddenService_address.onion [--nodePort=9999]`

??? info "Reminder"
    **Your_HiddenService_address** is the saved output from Whonix-Gateway<br>
    `sudo cat /var/lib/tor/haveno_service/hostname`

If not create a desktop shortcut: copy (or drag) `/opt/haveno/lib/haveno-Haveno.desktop` to your desktop and add the cmdline options like in the launcher above.

You can list all available haveno-desktop options for cmdline:<br>
`/opt/haveno/bin/Haveno -h`<br>
or to use in `~/.local/share/Haveno[-.*]/haveno.properties`

## Qubes OS

For Qubes/Whonix there are installation scripts for using Haveno in 2 ways:<br>
with [direct bind](hardened-tor.md) or via [control port](basic-tor.md) with the help of Netlayer/jtorctl.

- [Haveno on Qubes/Whonix](https://github.com/haveno-dex/haveno/tree/master/scripts/install_whonix_qubes)
- [Install Haveno on Qubes/Whonix](https://github.com/haveno-dex/haveno/blob/master/scripts/install_whonix_qubes/README.md)
- [Documentation Haveno on Qubes/Whonix](https://github.com/haveno-dex/haveno/blob/master/scripts/install_whonix_qubes/INSTALL.md)

## Windows

1. Download and install [Tor Browser](https://www.torproject.org/download/).
2. Create a new folder at `C:\TorData` (change as desired throughout these instructions)
3. Create a subfolder at `C:\TorData\haveno_onion_service`
4. Open **Notepad** and paste the following configuration:

    ??? note "Hardened `torrc` template for Windows"

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
        SocksPort 9050 ExtendedErrors

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
        HiddenServiceDir C:\TorData\haveno_onion_service\ 
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

5. Save the file inside `C:\TorData` as **All Files (*.*)** and name it exactly `torrc` (make sure it does not end in .txt).
6. Open **Command Prompt**.
7. Run this command to start your custom service (replace <YourUsername> with your real Windows profile name):
   ```
   "C:\Users\<YourUsername>\Desktop\Tor Browser\Browser\TorBrowser\Tor\tor.exe" -f C:\TorData\torrc
   ```
8. Keep this window open. Once the terminal logs `Bootstrapped 100% (done)`, your service is live online.
9. Copy your static onion address by opening `C:\TorData\haveno_onion_service\hostname`.
10. Right click the Haveno shortcut on your desktop and select **Properties**.
11. Add a startup flag to **Target** with your onion address:
    ```
    --hiddenServiceAddress=your_onion_address.onion [--nodePort=9999]
    ```
    For example:
    ```
    C:\Users\YourUsername\AppData\Local\Haveno\Haveno.exe --hiddenServiceAddress=wkisvdobtloqx5grm25o7vr6rjkhbs5qbqoqkw3tzqusr3ppi6wvceid.onion
    ```
12. Double click the Haveno shortcut to open the application bound to your onion service.

## Every OS

Backup your Tor Hidden (Onion) Service Private Key

!!! remember "Reminder"
    You may backup the onion service key. This is necessary in order to restore it on another machine, after HDD/SSD failure, etc. to recover or reuse your Haveno ID.

Root permission is required to access it ('su -' or sudo)

`cp /var/lib/tor/hidden_service/hs_ed25519_secret_key /home/user/hs_ed25519_secret_key`

Although only the private key is needed to restore a HiddenService, I prefer to back up the entire HiddenService folder:
`cp -r /var/lib/tor/hidden_service/ /home/user/hidden_service/`

Then save the key or folder in a secure location. Best together with your Haveno wallet seed and backup.
