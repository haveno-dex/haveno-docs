# WebTunnels

WebTunnels are a modern Tor bridge designed to bypass network censorship.

By disguising Tor traffic as standard HTTPS web traffic, WebTunnels allow Haveno to connect even in environments where Tor is actively blocked.

## Obtain WebTunnel Bridges

To use this feature, you must first obtain bridge addresses from the Tor Project.

1. Visit [bridges.torproject.org](https://bridges.torproject.org).
2. Click 'Get Bridges'.
3. Select 'webtunnel' from the drop down selection.
4. Complete the CAPTCHA if necessary to reveal your bridge addresses.
5. Copy the provided addresses (it will look like webtunnel 10.0.0.1:443...)

## Configure Haveno to use WebTunnels

![Light mode](/resources/img/connectivity/bridges_light.png#only-light)
![Dark mode](/resources/img/connectivity/bridges_dark.png#only-dark)

1. Open the network settings menu from **Settings > Network Info > Open Tor settings** or click the onion icon in the bottom right.
2. Paste the webtunnel addresses into the custom bridges.
3. Restart Haveno for the changes to take effect.
