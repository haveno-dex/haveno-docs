# Getting Started

## Install Haveno

Haveno can be installed on Linux, macOS, Windows, or Android by downloading the installer for your system.

!!! Warning "Note"
    The Haveno project does not provide installers directly, because it does not operate or endorse any mainnet network.

    First find a third party network, and then download their installer.
    
    Alternatively you can build from source by customizing [these instructions](https://docs.haveno.exchange/developers/installing/) with your provider's repository.

=== "Linux"
    
    - First uninstall Haveno if it's already installed.
    - Supports DEB, RPM, AppImage, and Flatpak.
    
    !!! note "Default install directory: `~/.local/share/`"


=== "macOS"

    You must grant permission for Haveno to run on macOS:

    1. Open the .dmg installer and drag Haveno.app to your Applications folder.
    2. Open a terminal window (cmd + space then type "terminal").
    3. Copy and paste into the terminal: `sudo xattr -rd com.apple.quarantine /Applications/Haveno.app` and press enter.
    4. Enter your computer password.
    5. Right click /Applications/Haveno.app > Open. Repeat again if needed, even if reported as damaged.

    !!! note "Default install directory: `~/Library/Application Support/`"

=== "Windows"
    - First uninstall Haveno if it's already installed.
    - Run the .exe installer normally.

    !!! note "Default install directory: `~\AppData\Roaming\`"

=== "Tails"

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

    !!! note "Default install directory: `/home/amnesia/Persistent/haveno/Data/`"

=== "Android"

    Haveno can be installed on Android via the [Haveno app](https://github.com/atsamd21/Haveno-app).
    
    The APK is provided by the third party Haveno network you're using.

## Run a Monero node (recommended)

For the best experience, running your own local Monero node is recommended, because the Tor network can be slow and unreliable when syncing with remote Monero nodes.

You can run your own Monero node by downloading the CLI from [https://www.getmonero.org/downloads/](https://www.getmonero.org/downloads/).

## Start Haveno Desktop

After installing Haveno, double click the application icon to start the application.

If you built from Haveno from source, start Haveno with `make haveno-desktop-mainnet`.

It can take a moment to connect to Tor and sync with the Monero network.

![Light mode](/resources/img/getting_started/startup_light.png#only-light)
![Dark mode](/resources/img/getting_started/startup_dark.png#only-dark)

![Light mode](/resources/img/getting_started/market_light.png#only-light)
![Dark mode](/resources/img/getting_started/market_dark.png#only-dark)

## Create a payment account

Before you can buy or sell, you must create at least one payment account.

Payment accounts define how you want to send or receive payment for XMR, whether by bank transfer, cryptocurrency, or other supported method like precious metals.

Go to the **Account** tab and click **Add new account**. Choose your preferred payment method and enter your details exactly as they appear with your bank or provider. Then click **Save**.

![Light mode](/resources/img/getting_started/add_new_account_light.png#only-light)
![Dark mode](/resources/img/getting_started/add_new_account_dark.png#only-dark)
![Light mode](/resources/img/getting_started/save_new_account_light.png#only-light)
![Dark mode](/resources/img/getting_started/save_new_account_dark.png#only-dark)

!!! info "Tips"
    - Make sure your details are accurate. Incorrect information can delay or prevent a trade.
    - You can add more than one payment account at any time.

## Fund your wallet (optional)

To create or take offers, you may either:

- Fund your Haveno wallet in advance, or
- Fund offers directly from an external wallet as needed.

To fund your Haveno wallet:

1. Go to the **Funds** tab.
2. Select the **Receive funds** sub-tab.
3. Choose a subaddress.
4. Copy the subaddress or scan the QR code to send XMR from an external wallet.

Funds are stored in your non-custodial Haveno wallet and can be withdrawn any time.

![Light mode](/resources/img/getting_started/deposit_light.png#only-light)
![Dark mode](/resources/img/getting_started/deposit_dark.png#only-dark)
![Light mode](/resources/img/getting_started/deposit_qr_light.png#only-light)
![Dark mode](/resources/img/getting_started/deposit_qr_dark.png#only-dark)


## Create or take an offer

On Haveno, you are always buying or selling XMR for another asset.

You can either:

- **Take an offer:** Instantly accept an existing offer.

- **Create a new offer:** Set your own price and payment terms.

### Example: Buying XMR

In this example, we'll create an offer to buy XMR with USD using Zelle.

Navigate to the **Buy XMR** tab. Under the **Fiat** tab, select **USD** and click **Create offer to BUY XMR**.

![Light mode](/resources/img/getting_started/offers_fiat_light.png#only-light)
![Dark mode](/resources/img/getting_started/offers_fiat_dark.png#only-dark)

### Set your offer terms

- Amount of XMR to buy
- Price
- Trigger price (optional)
- Security deposit (default 15%)
- Any additional terms you want to apply

Click the **Next step** button to continue.

![Light mode](/resources/img/getting_started/create_offer_light.png#only-light)
![Dark mode](/resources/img/getting_started/create_offer_dark.png#only-dark)

### Fund your offer

To post your offer, you must deposit XMR in the sum of:

- A security deposit
- Trading fee
- Trade amount (only if selling XMR)

You can deposit XMR from an external wallet or apply existing funds from your wallet.

When the trade completes successfully, the security deposit is returned to your main wallet.

![Light mode](/resources/img/getting_started/create_offer_fund_light.png#only-light)
![Dark mode](/resources/img/getting_started/create_offer_fund_dark.png#only-dark)

![Light mode](/resources/img/getting_started/create_offer_fund_qr_light.png#only-light)
![Dark mode](/resources/img/getting_started/create_offer_fund_qr_dark.png#only-dark)

### Post your offer

After your offer is funded, confirm your offer details to post the offer.

![Light mode](/resources/img/getting_started/confirm_offer_light.png#only-light)
![Dark mode](/resources/img/getting_started/confirm_offer_dark.png#only-dark)

## Complete a trade

A trade begins when:

- Someone takes your offer, or
- You take an existing offer.

The payment step can begin after 10 confirmations (~20 minutes).

For example, if buying XMR with Zelle:

- The XMR buyer sends the Zelle payment to the seller.
- The buyer confirms the payment in Haveno.

![Light mode](/resources/img/getting_started/confirm_payment_sent_light.png#only-light)
![Dark mode](/resources/img/getting_started/confirm_payment_sent_dark.png#only-dark)

When the XMR seller confirms that the payment was received, the trade funds are released to your Haveno wallet and the trade is complete.

![Light mode](/resources/img/getting_started/confirm_payment_received_light.png#only-light)
![Dark mode](/resources/img/getting_started/confirm_payment_received_dark.png#only-dark)

## Withdraw funds

You can withdraw funds from your Haveno wallet at any time.

To withdraw funds:

1. Go to the **Funds** tab.
2. Select the **Send funds** sub-tab.
3. Enter a XMR address to withdraw funds to.
4. Enter the amount of XMR to send.
5. Click the **Withdraw selected** button.
6. Confirm the transaction details to complete the withdraw.

## Account signing

Some fiat payment accounts can be signed in order to increase trading limits.

In order to sign a payment account, you must:

- Buy at least 0.05 XMR
- From a trader whose account is already signed
- Using a supported fiat payment account (with chargeback risk)

When your account is signed:

- Your account can sign other accounts.
- Your trading limits increase after 30 days.
- Limits increase again after 60 days.

!!! note
    - Not all payment methods support account signing.
    - Signing is tied to a specific payment account.
    - You only need to sign each payment account once.

## Backup your application

Backing up ensures you can recover your wallet and trade history if your device is lost or damaged.

### Backup your application folder

To make a complete backup of your application data, create a copy of your application's data folder.

Your data folder contains:

- Trade history
- Payment accounts
- Application settings

You can create a backup of your application folder within the application by navigating to **Account** > **Backup**.

![Light mode](/resources/img/getting_started/backup_light.png#only-light)
![Dark mode](/resources/img/getting_started/backup_dark.png#only-dark)

Alternatively, you can copy your entire application folder to another location.

!!! note "Default install locations"
    - Linux: `~/.local/share/Haveno`
    - macOS: `~/Library/Application Support/Haveno`
    - Windows: `%APPDATA%\Haveno` or `~\AppData\Roaming\Haveno`

### Backup your seed phrase

Your seed phrase can restore your main Haveno wallet.

However, the seed phrase does NOT backup critical application data like trade history, payment accounts, etc. To create a complete backup, you must backup your entire application data folder.

To view your seed phrase, navigate to **Account** > **Wallet seed**.

- Store it securely.
- Do not share it.
- Anyone with your seed can access your funds.

![Light mode](/resources/img/getting_started/seed_light.png#only-light)
![Dark mode](/resources/img/getting_started/seed_dark.png#only-dark)

!!! Warning
    The seed phrase is NOT a complete backup of the application.
    
    To create a full and complete backup, you must backup your *entire application directory*.
