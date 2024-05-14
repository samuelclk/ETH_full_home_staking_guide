# Install the OS

## Flash Ubuntu OS on USB Drive

### Download the latest version of Ubuntu

Now that you have assembled your hardware, you will need to install the Ubuntu OS onto your device. To do that, we will need to **create a bootable USB drive flashed with the latest Ubuntu OS.** Follow the steps below:

1. Prepare a new USB drive of at least 8GB
2.  On your working laptop, download the latest version of Ubuntu here (this might take around 30 minutes**)** - [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop) -&#x20;

    <figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>
3. Once the download is complete, you will need to verify the checksum of the downloaded file to ensure it has not been tampered with during the download&#x20;

### Verify the checksum of download

Click on "verify your download" and you should see a window appearing.&#x20;

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

Open up your terminal (Mac) or Windows Power Shell (Windows) and run the following commands.

```sh
cd Downloads
echo "a435f6f393dda581172490eda9f683c32e495158a780b5a1de422ee77d98e909 *ubuntu-22.04.3-desktop-amd64.iso" | shasum -a 256 --check
```

You are good to go if you see an "OK" in the output.

```sh
ubuntu-22.04.3-desktop-amd64.iso: OK
```

### Download an ISO writer

After we download the ISO file of the latest Ubuntu version, we will need a tool to write this ISO file into the USB drive so that it is bootable when plugged into your NUC.&#x20;

1. Download and install BalenaEtcher - [https://etcher.balena.io/](https://etcher.balena.io/)
2.  Open up BalenaEtcher and choose select flash from file&#x20;

    <figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>
3.  Select your new USB drive under the "Select target" option&#x20;

    <figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>


4.  Hit the "Flash!" button and wait for the process to complete&#x20;

    <figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

## Install Ubuntu on your NUC

{% hint style="info" %}
You will need to connect your NUC device to a keyboard and monitor for the installation process
{% endhint %}

Plug your bootable USB drive into your NUC device and turn it on. Select `Try or Install Ubuntu` from the boot menu.&#x20;

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

Choose the following options when prompted:

1. Install Ubuntu (not Try)
2. _**Connect to the WIFI network of your Node Router**_
3. Normal installation + Download updates while installing Ubuntu
4. Erase disk and install Ubuntu
5. Set your username and password + "Require my password to login"
6. Restart your device
7. Skip connecting to your online accounts
8. Skip setting up Livepatch
9. Select "No, don't send system info"
10. Disable location services

Your NUC device is now installed with the Ubuntu OS.

### Install the SSH Server

Open up the Ubuntu terminal on your NUC by pressing `CTRL + ALT + T` and perform the following:

1\) General updates

```sh
sudo apt update -y && sudo apt upgrade -y
```

2\) Install the ssh server

```sh
sudo apt install openssh-server
```

3\) Get the IP address of your NUC device within your Node Router subnet.

```sh
hostname -I | awk '{print $1}'
```

**Expected output:**

```sh
192.168.x.xxx
```

Write this down as you will need to use this IP address to access your NUC remotely and we will call this the `node_IP_address` moving forward.

You can now access your NUC (`Node`) remotely by running the following command while you are in the Node Router subnet and entering the password of the Node when prompted.

```sh
ssh <username>@<node_IP_address> -v
```

**Note:** This command will change slightly once you properly secure your Node device in the next section.
