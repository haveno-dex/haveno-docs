# Haveno using OS provided tor aka little-t-tor

Since Haveno v1.0.10 there is the [DirectBindTor](https://github.com/haveno-dex/haveno/commit/3d44f3777c7bdf56707ca5d0768877535ac854c5) option.
This means that we can create and use a HiddenService (aka Onion Service) with portforward 9999 with tor (the network daemon) provided by the operating system.

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
    <br>  
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

For Qubes there are two pull requests for install scripts that uses Haveno with DirectBindTor:

[Script to create appvm to run Haveno on qubes](https://github.com/haveno-dex/haveno/pull/1583)

Can do both: Haveno with DirectBindTor (static HiddenService) or create a dynamic one with the help of Netlayer/jtorctl.

- [PR install_qubes](https://github.com/haveno-dex/haveno/pull/1628)
- [Repo install_qubes](https://github.com/PromptPunksFauxCough/haveno/tree/install_whonix_qubes/scripts/install_whonix_qubes)

## Every OS

Backup your Tor Hidden (Onion) Service Private Key

!!! remember "Reminder"
    You may backup the onion service key. This is necessary in order to restore it on another machine, after HDD/SSD failure, etc. to recover or reuse your Haveno ID.

Root permission is required to access it ('su -' or sudo)

`cp /var/lib/tor/hidden_service/hs_ed25519_secret_key /home/user/hs_ed25519_secret_key`

Although only the private key is needed to restore a HiddenService, I prefer to back up the entire HiddenService folder:
`cp -r /var/lib/tor/hidden_service/ /home/user/hidden_service/`

Then save the key or folder in a secure location. Best together with your Haveno wallet seed and backup.
