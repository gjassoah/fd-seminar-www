+++
title = "DIY BigBlueButton Server"
type = "about"
+++
{{< note >}}
**Last update:** 28.05.2020
{{< /note >}}

# DIY BigBlueButton Server

During the talks, the FD Serminar runs on a cloud server with the following specifications:

{{< server product="Hetzner CCX41" os="Ubuntu 16.04 (Linux 4.4.0-178-generic)" cpu="Intel Xeon Processor (Skylake, IBRS, 16 dedicated vCPUs)" ram="64 GB RAM / 8 GB Swap" >}}{{< /server >}}

For those who might be interested, below are the steps I followed to set up our BigBlueButton instance. Although the main part took only one afternoon, one does need to be comfortable managing a GNU+Linux system and mildly familiar with maintaining a server.

Happy hacking!

Gustavo

P.S. FWIW [Help the World by Healing Your NGINX Configuration](https://www.nginx.com/blog/help-the-world-by-healing-your-nginx-configuration/)

{{< warning >}}
I am not an expert in server administration (not even close). The steps below are provided for reference only and under no warranty of applicability or suitability. Follow them at your own risk!
{{< /warning >}}

## BigBlueButton

The FD Seminar itself runs on [BigBlueButton (BBB)](https://bigbluebutton.org/), an open-source web conferencing software. I installed it following the [BBB official installation guide](https://docs.bigbluebutton.org/2.2/install.html) and it took me around one hour.

### Greenlight

You need to install one more piece of software, called [Greenlight](https://docs.bigbluebutton.org/greenlight/gl-overview.html), which serves as a front-end to your BBB server; with it you can manage BBB's users, rooms, and recordings. I installed it following the [Greenlight official installation guide](https://docs.bigbluebutton.org/greenlight/gl-install.html) and it took me about half an hour. I needed to customise Greenlight to fit our needs; for this I followed the [Greenlight official customisation guide](https://docs.bigbluebutton.org/greenlight/gl-customize.html), which took me a further ten minutes. Customising Greenlight is a potential rabbit hole; I probably spent around four hours on this.

### TURN server

Some users experienced problems when trying to join audio sessions in our BigBlueButton instance (error 1007); this is likely due to users attempting to connect behind a firewall. I followed the [BBB official guide for setting up a TURN server](https://docs.bigbluebutton.org/2.2/setup-turn-server.html) in an attempt to fix this (~~it remains to be seen if this solves our users' issues~~ this solved our users' issues). The installation took around 30 minutes.

These are the specifications of our TURN server:

{{< server product="Hetzner CPX11" os="Ubuntu 18.04.4 (Linux 4.15.0-99-generic)" cpu="AMD EPYC Processor (IBPB, 2 vCPUs)" ram="2 GB RAM / 2 GB Swap" >}}{{< /server >}}

## Live streaming

To accommodate for a larger number of participants, we use [OBS Studio](https://obsproject.com/) to stream the talks to an [RTMP server](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol) set up following this [excellent guide at the NGINX blog](https://www.nginx.com/blog/video-streaming-for-remote-learning-with-nginx/); we stream using [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP). In order to view the stream in a web browser we use [indigo-player](https://matvp91.github.io/indigo-player/#/), for which you need to configure correctly the CORS headers in your `http` block, see for example [this guide](https://docs.peer5.com/guides/cors/). This should take you around 10 minutes (it took me much longer since I was not aware of the need of configuring the CORS headers).

These are the specifications of our RTMP server during the live streams:

{{< server product="Hetzner CPX21" os="Ubuntu 18.04.4 (Linux 4.15.0-99-generic)" cpu="AMD EPYC Processor (3 vCPUs)" ram="4 GB RAM / 2 GB Swap" >}}{{< /server >}}

## Firewall

I used [UFW](https://launchpad.net/ufw) to protect our server by allowing incoming traffic only through [our custom SSH port](https://www.linode.com/docs/security/securing-your-server/#harden-ssh-access) and through the ports required by BBB to function (see the BBB installation guide). For this, I followed [this guide provided by Linode](https://www.linode.com/docs/security/firewalls/configure-firewall-with-ufw/) and it took me about five minutes.

## Server monitoring

We use [Grafana](https://grafana.com/) and [Prometheus](https://prometheus.io/) to monitor our BBB instance. For setting this up I used the following [docker container provided by a third-party developer](https://bigbluebutton-exporter.greenstatic.dev/installation/all_in_one_monitoring_stack/). The installation takes about 10 minutes.

### 21.05.2020

The FD seminar ran on a cloud server with the following specifications:

{{< server product="Linode Dedicated 16GB" os="Ubuntu 16.04 (Linux 4.4.0-179-generic)" cpu="AMD EPYC 7501 32-Core Processor (8 dedicated vCPUs)" ram="16 GB RAM / 10 GB Swap" >}}{{< /server >}}

We found these to be sub-optimal for running a single BBB session with 77 participants and 8 shared webcams (some users reported low audio quality). Below is a screenshot with the metrics gathered during the talk.

[![21.05.2020](/img/grafana-21.05.2020.png)](/img/grafana-21.05.2020.png)

### 28.05.2020

The FD seminar ran on a cloud server with the following specifications:

{{< server product="Hetzner CCX41" os="Ubuntu 16.04 (Linux 4.4.0-178-generic)" cpu="Intel Xeon Processor (Skylake, IBRS, 16 dedicated vCPUs)" ram="64 GB RAM / 8 GB Swap" >}}{{< /server >}}

We found these to be optimal for running a single BBB session with close to 100 participants and 6 shared webcams. Below is a screenshot with the metrics gathered during the talk.

[![21.05.2020](/img/grafana-28.05.2020.png)](/img/grafana-28.05.2020.png)
