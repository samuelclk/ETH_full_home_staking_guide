# Validator key generation

## Important

* It is highly recommended that you perform this step using an air-gapped machine - i.e. a device that has never connected to the public internet before. We will describe a few methods below.
* If this is not available, turn off all internet and wireless connection (e.g. Ethernet, WiFi, Bluetooth) before proceeding with the key generation step
* In both cases above, make sure you are in a safe environment (e.g. home or office) with a trusted WiFi network for building the validator key generation tool from source. Make sure to also physically block all camera devices - e.g. laptop cameras, Webcams, people standing behind you during this process

## Creating an air-gapped machine

1. The least technical way is to buy a cheap single board computer like the Raspberry Pi from official distributors for less than S$100 SGD
2. **"OS-on-a-stick":** For more technical workarounds, we can flash a new USB drive with either Ubuntu or TailsOS and run a completely fresh OS from this USB drive itself. This system will be completely isolated from your host device (e.g. working laptop) and the described method below will not store any files after you remove the USB drive

#### _**We will cover Method 2 in this guide.**_&#x20;

## Flash and install OS

1\) Download latest Ubuntu OS [here](https://ubuntu.com/download) or TailsOS [here](https://tails.net/install/download/) and follow the respective instructions to verify the checksums of the downloaded file.

2\) Download an ISO flasher (e.g. [BalenaEtcher](https://etcher.balena.io/)) and flash your USB drive with your preferred OS. Refer to the previous section for steps (1) and (2) if needed.

{% content-ref url="../hardware-and-systems-setup/install-the-os.md" %}
[install-the-os.md](../hardware-and-systems-setup/install-the-os.md)
{% endcontent-ref %}

3\) Once your USB drive is flashed with your preferred OS, plug it into your working device and reboot the device to go into the boot menu. Depending on your system, you might need to hold `F2`, `F10`, `F12`, or `ESC` during the rebooting process to bring up the boot menu. If it boots to a `grub>` prompt awaiting a command, type `exit` and press Enter.

4\) Once you see the boot menu, select the option to boot up from your USB drive instead of your usual storage volume and you should see the following screen.

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

5\) Select `*Try or Install Ubuntu` and then `Try Ubuntu` when you get to the next screen

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

## Building the validator key generation tool from source

Connect your OS-on-a-stick to a trusted WiFi network and fire up the Linux terminal using `Ctrl + Alt + T` .

Install system dependencies - `pip3` & `virtualenv`. _**Note:** Remove the `sudo` prefix for each line if your system returns an error._

```bash
sudo add-apt-repository universe
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt install python3-venv python3-pip
sudo apt install python3-virtualenv
sudo apt-get install git
```

Create a python virtual environment to run the tool under:

```bash
virtualenv venv
source venv/bin/activate
```

Download the git repository of the tool by running. Reference and verify the URL of the git repository [here](https://github.com/ethereum/staking-deposit-cli).

```bash
git clone -b master --single-branch https://github.com/ethereum/staking-deposit-cli.git
```

Install the dependency packages for running the tool:

```bash
cd staking-deposit-cli/
python3 setup.py install
pip3 install -r requirements.txt
```

**Expected output:** You should see some progress bars after a few seconds.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

### \*BEFORE PROCEEDING TO THE NEXT STEP

1. #### TURN OFF YOUR ETHERNET, WIFI, AND BLUETOOTH ACCESS&#x20;
2. #### PHYSICALLY COVER ALL CAMERA DEVICES - e.g. PHONES, WEBCAMS, LAPTOP CAMERAS, PEOPLE STANDING BEHIND YOU

## Generate your validator signing keys

Run the following command to generate your validator keys. Replace `<number>` with the number of validators you want to set up and `<YourWithdrawalAaddress>` with the actual withdrawal address

```bash
python3 ./staking_deposit/deposit.py new-mnemonic --num_validators <number> --chain mainnet --eth1_withdrawal_address <YourWithdrawalAaddress>
```

You will be prompted to key in the following. Select accordingly.

1. Choose your language (for the session)
2. Confirm your execution address (your withdrawal address)
3. Choose the language of your mnemonic word list (seed phrase)
4. Create a password to encrypt your validator signing keystores
5. Confirm password created in step 4

**Expected output:**

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

Next, your mnemonic word list will be generated. Write it down on a piece of paper or notebook  -\***Never store this online or on any device that is connected to the internet**.

&#x20;**Expected output:**

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Press any key once you have written your mnemonic down and the tool will prompt you to key in your mnemonic in the same order to verify that you have recorded it correctly.

If you typed in your mnemonic correctly, you will be greeted by an ASCII art of a Rhino!

**Expected output:**

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

**There will be 2 files generated.**&#x20;

1. A `keystore-m_<timestamp>.json` file: This is your validator signing keystore that your validator node will use to sign attestations. Keep this file extremely secure.
2. A `deposit_data-<timestamp>.json`: This is the file that links your ETH deposit to your validator. You will only use this once, during the deposit process.

Store both files on a new USB drive by copying the entire staking-deposit-cli folder into it. After that, remove the original copy by running:

```
sudo rm -r $HOME/staking-deposit-cli/validator_keys
```

Restart your host device (e.g. working laptop) and remove the OS-on-a-stick. There will not be any persistent memory stored on it.

## Add validator key to the Node

Now that we have our validator signing keystore, we will need to place it in our validator node itself so that the node can sign attestations and propose blocks.

Plug in the USB drive with your validator signing keystores into your node device. Once the USB drive is plugged in, we will need to identify it. On the terminal of your node, run:

```
lsblk
```

**Expected output:**

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

Look for your USB drive in the output list. It will take a name similar to the screenshot above - i.e. `sdx`.

After you find it, you can proceed to mount your USB drive onto the `/media` folder.

```sh
sudo mount /dev/sda1 /media
```

**Note:** Replace `sda1` with the actual name of your USB drive.

You will now be able to access your USB drive via the terminal by going into the `/media` folder.

Go into your USB drive and copy your validator signing keystore into the HOME directory of your node.

```sh
cd /media/staking-deposit-cli
sudo cp -r validator_keys ~
```

Unmount and eject your USB drive.

```sh
cd
sudo umount /media
```

Now you need to create a plain text password file for your validator node to decrypt your validator signing keystores.

First let's print and copy the file name of your validator signing keystore.

```
cd ~/validator_keys
ls
```

With the `validator_signing_keystore_file_name` copied (without the `.json` file extension), create the password file.

<pre><code>sudo nano <a data-footnote-ref href="#user-content-fn-1">&#x3C;validator_signing_keystore_file_name></a>.txt
</code></pre>

Type in the password you used when generating your validator keys in the earlier step. Then save and exit the file with `CTRL + O, enter, CTRL + X`.

[^1]: 
