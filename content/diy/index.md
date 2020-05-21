+++
title = "DIY BigBlueButton Server"
type = "about"
+++
{{< note >}}
**Last update:** 21.05.2020
{{< /note >}}

# DIY BigBlueButton Server

During the talks, the FD Serminar runs on a server with the following specifications:

{{< server >}}{{< /server >}}

[![21.05.2020](/img/grafana-21.05.2020.png)](/img/grafana-21.05.2020.png)

For those who might be interested, below are the steps I followed to set it up. Although the main part took only one afternoon, one does need to be comfortable managing a GNU+Linux system and mildly familiar with maintaining a server.

Happy hacking!

Gustavo

{{< warning >}}
I am not an expert in server administration (not even close). The steps below are provided for reference only and under no warranty of applicability or suitability. Follow them at your own risk!
{{< /warning >}}

## BigBlueButton

The FD Seminar itself runs on [BigBlueButton (BBB)](https://bigbluebutton.org/), an open-source web conferencing software. I installed it following the [BBB official installation guide](https://docs.bigbluebutton.org/2.2/install.html) and it took me around one hour.

## Greenlight

You need to install one more piece of software, called [Greenlight](https://docs.bigbluebutton.org/greenlight/gl-overview.html), which serves as a front-end to your BBB server; with it you can manage BBB's users, rooms, and recordings. I installed it following the [Greenlight official installation guide](https://docs.bigbluebutton.org/greenlight/gl-install.html) and it took me about half an hour. I needed to customise Greenlight to fit our needs; for this I followed the [Greenlight official customisation guide](https://docs.bigbluebutton.org/greenlight/gl-customize.html), which took me a further ten minutes. Customising Greenlight is a potential rabbit hole; I probably spent around four hours on this.

## Firewall

I used [UFW](https://launchpad.net/ufw) to protect our server by allowing incoming traffic only through [our custom SSH port](https://www.linode.com/docs/security/securing-your-server/#harden-ssh-access) and through the ports required by BBB to function (see the BBB installation guide). For this, I followed [this guide provided by Linode](https://www.linode.com/docs/security/firewalls/configure-firewall-with-ufw/) and it took me about five minutes.

## Server monitoring

We use [Grafana](https://grafana.com/) and [Prometheus](https://prometheus.io/) to monitor our BBB instance. For setting this up I used the following [docker container provided by a third-party developer](https://bigbluebutton-exporter.greenstatic.dev/installation/all_in_one_monitoring_stack/). The installation takes about 10 minutes.
