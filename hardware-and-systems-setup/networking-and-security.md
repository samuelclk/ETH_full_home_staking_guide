# Networking & Security

## Network architecture

1. Your existing home router will first be connected to a dedicated "node router" on the downstream via cable. This node router will be set to "router" mode and not "Access Point (AP)" mode - this will create a subnet within your main network for segregation of your regular home devices from your validator node setup.&#x20;
2. The validator node will be connected to the node router further downstream via cable
3. Secure your node router properly by setting strong passwords on the WIFI and device level. Do not expose these passwords or let anyone else connect to the WIFI network or log in to the device level of your node router.
4. You will need to be connected to the WIFI network of your Node Router in order to access your Validator Node remotely

<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

:bulb: **Tip:** If you need to be away from home for long periods, port forwarding will need to be configured on both your Home Modem and your Home Router (i.e. Modem->Home Router->Node Router) to allow incoming connections from outside of your home network.&#x20;

* This is so that you can access your validator node for troubleshooting and maintenance even if you are not at home.
* Turn off port forwarding on your Home Modem when you are no longer away from home

Check out the Port Forwarding page below after you are done setting up your validator node.

{% content-ref url="../tips/port-forwarding.md" %}
[port-forwarding.md](../tips/port-forwarding.md)
{% endcontent-ref %}

## Security model

1. Your validator node will be secured with an SSH key so that only users who have this SSH key can access it
2. Any home devices that become compromised will not be able to access your validator node which sits in a separate subnet
3. If you think your SSH keys could be leaked, turn off any port forwarding settings and change the SSH key pair in your Validator Node

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>
