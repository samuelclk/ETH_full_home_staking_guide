# Port Forwarding

Port forwarding allows devices outside of your home network to access devices within. For our use case, this allows you to:

1. SSH into your validator node to perform maintenance and troubleshooting even when you are on vacation
2. Addresses the "low peer count" problem on your execution layer or consensus layer client

Due to routers/modems having different interfaces, you will need to play around with your own configuration panel or look up the official manual of your router's model.&#x20;

**Example:**

{% tabs %}
{% tab title="SSH" %}
<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>


{% endtab %}

{% tab title="EL low peer count" %}
<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Cl low peer count" %}
<figure><img src="../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

If you have a router sitting below your modem, you will need to configure port forwarding from `modem->router->validator` using the same port numbers on both the modem and the router.&#x20;
