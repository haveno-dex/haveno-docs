# Bridges

Bridges are Tor relays that help Haveno connect when Tor is blocked by your internet provider or country.

Haveno includes Snowflake, which works without any setup. You can also enter custom bridges, such as WebTunnels, which disguise Tor traffic as standard HTTPS web traffic.

![Light mode](/resources/img/connectivity/bridges_light.png#only-light)
![Dark mode](/resources/img/connectivity/bridges_dark.png#only-dark)

## Use Snowflake

1. Open the network settings menu from **Settings > Network Info > Open Tor settings** or click the onion icon in the bottom right.
2. Select **Use Snowflake**.
3. Restart Haveno for the changes to take effect.

## Use custom bridges

To use custom bridges, you must first obtain bridge addresses from the Tor Project.

1. Visit [bridges.torproject.org](https://bridges.torproject.org).
2. Click 'Get Bridges'.
3. Select a bridge type, such as 'webtunnel', from the drop down selection.
4. Complete the CAPTCHA if necessary to reveal your bridge addresses.
5. Copy the provided addresses (it will look like webtunnel 10.0.0.1:443...)

Then configure Haveno to use them:

1. Open the network settings menu from **Settings > Network Info > Open Tor settings** or click the onion icon in the bottom right.
2. Select **Enter custom bridges** and paste the addresses, one per line.
3. Restart Haveno for the changes to take effect.
