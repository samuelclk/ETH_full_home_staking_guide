# Set up and configure MEV-boost

### Install and configure MEV-boost

{% tabs %}
{% tab title="Build from source" %}
Install dependencies - Make, Git

```sh
sudo apt install make git
```

Install dependencies - Go (download page [here](https://go.dev/dl/)) - and make sure the latest version (1.22.0) is output at the end of this command batch.

```sh
curl -LO https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
echo "f6c8a87aa03b92c4b0bf3d558e28ea03006eb29db78917daec5cfb6ec1046265 go1.22.0.linux-amd64.tar.gz" sha256sum --check
sudo tar xvf go1.22.0.linux-amd64.tar.gz -C /usr/local
export PATH=$PATH:/usr/local/go/bin
echo "export PATH=$PATH:/usr/local/go/bin"
go version
```

Download latest version of MEV-boost.

```sh
cd
git clone https://github.com/flashbots/mev-boost.git
cd mev-boost
git checkout tags/v1.7-alpha1
```

Build the executable file.

```sh
make build
```

Copy the executable file to the `/usr/local/bin` folder.

```sh
sudo cp mev-boost /usr/local/bin
```
{% endtab %}

{% tab title="Using binaries" %}
{% hint style="info" %}
This method does not support Holesky yet. Use the **Build from source** method for now
{% endhint %}

Download latest version of MEV-boost [here](https://github.com/flashbots/mev-boost/releases) and run the checksum verification process to ensure that the downloaded file has not been tampered with. The checksums can be found in the `checksums.txt` file - open it up and copy the `linux_amd64` version to use below.

```bash
cd 
curl -LO https://github.com/flashbots/mev-boost/releases/download/v1.7/mev-boost_1.7_linux_amd64.tar.gz
echo "92b435d200451e190000c4f8d82eef0b3ce72d2759153357883e81261ffe98e3 mev-boost_1.7_linux_amd64.tar.gz" | sha256sum --check
```

{% hint style="info" %}
Each downloadable file comes with it's own checksum. Replace the actual checksum and URL of the download link in the code block above.

{% hint style="info" %}
Make sure to choose the amd64 version. Right click on the linked text and select "copy link address" to get the URL of the download link to `curl`.
{% endhint %}
{% endhint %}

_**Expected output:** Verify output of the checksum verification_

```
mev-boost_1.7_linux_amd64.tar.gz: Ok
```

If checksum is verified, extract the files and move them into the `(/usr/local/bin)` directory for neatness and best practice. Then, clean up the duplicated copies.

```bash
tar xvf mev-boost_1.7_linux_amd64.tar.gz
sudo cp mev-boost /usr/local/bin
rm mev-boost LICENSE README.md mev-boost_1.7_linux_amd64.tar.gz

```
{% endtab %}
{% endtabs %}

Create an account (`mevboost`) without server access for MEV Boost to run as a background service. This restricts potential attackers to only the MEV Boost service in the unlikely event that they manage to infiltrate via a compromised client update.

```bash
sudo useradd --no-create-home --shell /bin/false mevboost
```

Create a systemd configuration file for the tekubeacon service to run in the background.

```bash
sudo nano /etc/systemd/system/mevboost.service
```

Paste the configuration parameters below into the file:

```bash
[Unit]
Description=mev-boost (Holesky)
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=mevboost
Group=mevboost
Restart=always
RestartSec=5
ExecStart=/usr/local/bin/mev-boost \
    -mainnet \
    -min-bid 0.05 \
    -relay-check \
    -relay <https://example.com> \
    -relay <https://example.com> \
    -relay <https://example.com> \
    -relay <https://example.com> 

[Install]
WantedBy=multi-user.target
```

Once you're done, save with `Ctrl+O` and `Enter`, then exit with `Ctrl+X`. Understand and review your configuration summary (flags) below, and amend if needed.

**MEV Boost configuration summary:**

1. `-holesky`: Run the MEV-boost service on the Holesky testnet
2. `-min-bid`: Set the threshold to accept blocks from relays if they bid above a chosen value, otherwise propose a locally-built block. This sacrifices a small \~0.1% APR in exchange for much better censorship resistance, allowing you to use OFAC-compliant relays guilt-free! More information [here](https://writings.flashbots.net/the-cost-of-resilience/)
3. `-relay-check`: check relay status on startup and on the status API call
4. `-relay`: A chosen relay URL. Choose your preferred ones here - [https://github.com/eth-educators/ethstaker-guides/blob/main/MEV-relay-list.md](https://github.com/eth-educators/ethstaker-guides/blob/main/MEV-relay-list.md)

### Start the MEV Boost service

Reload the systemd daemon to register the changes made, start MEV Boost, and check its status to make sure its running.

```bash
sudo systemctl daemon-reload
sudo systemctl start mevboost
sudo systemctl status mevboost.service
```

**Expected output:** The output should say MEV Boost is **“active (running)”.** Press CTRL-C to exit and MEV Boost will continue to run.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption><p>sudo systemctl status mevboost.service</p></figcaption></figure>

Use the following command to check the logs for any warnings or errors:

```bash
sudo journalctl -fu mevboost -o cat | ccze -A
```

**Expected output:**

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Press `CTRL-C` to exit.

If the MEV Boost service is running smoothly, we can now enable it to fire up automatically when rebooting the system.

```bash
sudo systemctl enable mevboost.service
```
