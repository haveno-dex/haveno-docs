# Install Notes

Download and install Haveno using an installer for Linux, macOS, or Windows. The installer is provided by the third party Haveno network you're using.

!!! Warning "Notice"
    The Haveno project does not provide installers directly, because we do not operate or endorse any mainnet network.

    First find a third party network and then use their installer, or build their repository by customizing [these instructions](https://docs.haveno.exchange/development/installing/).


## Run your own Monero node (recommended)

For the best experience, running your own local Monero node is highly recommended, because the Tor network can be slow and unreliable with Monero.

You can run your own Monero node by downloading the CLI from [https://www.getmonero.org/downloads/](https://www.getmonero.org/downloads/).

## Install Haveno on Linux

First uninstall Haveno if it is already installed.

Then install from .deb, .rpm, AppImage, or Flatpak for your system.

**Default install directory:** `~/.local/share/`

## Install Haveno on macOS

!!! Note
    macOS will not allow Haveno to run until permission is granted. Follow these steps to allow the application to run.

1. Open the .dmg installer and drag Haveno.app to your Applications folder.
2. Open a terminal window (cmd + space then type "terminal").
3. Copy and paste into the terminal: `sudo xattr -rd com.apple.quarantine /Applications/Haveno.app` and press enter.
4. Enter your computer password.
5. Right click /Applications/Haveno.app > Open. Repeat again if needed, even if reported as damaged.

**Default install directory:** `~/Library/Application Support/`

## Install Haveno on Windows

First uninstall Haveno if it is already installed.

Then run the .exe installer normally.

**Default install directory:** `~\AppData\Roaming\`

## Install Haveno on Tails

After you already have a [Tails USB](https://tails.net/install/index.en.html):

1. Enable [persistent storage](https://tails.net/doc/persistent_storage/index.en.html).
2. Set up [administration password](https://tails.net/doc/first_steps/welcome_screen/administration_password/).
3. Activate dotfiles in persistent storage settings.
4. Execute the following command in the terminal to download and execute the installation script.

    ```
    curl -fsSLO https://github.com/haveno-dex/haveno/raw/master/scripts/install_tails/haveno-install.sh && bash haveno-install.sh <REPLACE_WITH_INSTALLER_URL> <REPLACE_WITH_PGP_FINGERPRINT>
    ```
    
    Replace the installer URL and PGP fingerprint for the network you're using. For example:
    
    ```
    curl -fsSLO https://github.com/haveno-dex/haveno/raw/master/scripts/install_tails/haveno-install.sh && bash haveno-install.sh https://github.com/havenoexample/haveno-example/releases/latest/download/haveno-v1.2.2-linux-x86_64-installer.deb FAA24D878B8D36C90120A897CA02DAC12DAE2D0F
    ```
5. Start Haveno by finding the icon in the launcher under **Applications > Other**.

**Default install directory:** `/home/amnesia/Persistent/haveno/Data/`

