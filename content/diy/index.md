+++
title = "DIY BigBlueButton Server"
type = "about"
+++
{{< note >}}
**Last update:** 11.06.2020
{{< /note >}}

# DIY BigBlueButton Server

The FD Seminar runs on a cloud server with the following specifications:

{{< server product="Hetzner CCX41" os="Ubuntu 16.04 (Linux 4.4.0-178-generic)" cpu="Intel Xeon Processor (Skylake, IBRS, 16 dedicated vCPUs)" ram="64 GB RAM / 8 GB Swap" >}}{{< /server >}}

We found these to be **optimal** for running a **single BigBlueButton session** with **close to 100 participants**.

[![11.06.2020](/img/grafana-11.06.2020.png)](/img/grafana-11.06.2020.png)

Below I outline the steps followed to set up our BigBlueButton instance.

Happy hacking!

Gustavo

P.S. FWIW [Help the World by Healing Your NGINX Configuration](https://www.nginx.com/blog/help-the-world-by-healing-your-nginx-configuration/).

{{< warning >}}
**Disclaimer:**
I am not an expert in server administration (not even close). The information below is provided for reference only and under no warranty of applicability or suitability. Use at your own risk!
{{< /warning >}}

## BigBlueButton

The FD Seminar uses [BigBlueButton (BBB)](https://bigbluebutton.org/)---an open-source web conferencing software---installed following the [BBB official installation guide](https://docs.bigbluebutton.org/2.2/install.html).

### Greenlight

[Greenlight](https://docs.bigbluebutton.org/greenlight/gl-overview.html) is the standard end-user interface for the BBB server (needed to manage users, rooms, and recordings). It can be installed following the [Greenlight official installation guide](https://docs.bigbluebutton.org/greenlight/gl-install.html). For customising Greenlight to fit the needs of the seminar, I followed the [Greenlight official customisation guide](https://docs.bigbluebutton.org/greenlight/gl-customize.html).

[![Greenlight](/img/greenlight.png)](/img/greenlight.png)

### TURN server

Some users experienced problems when trying to join audio sessions in our BigBlueButton instance ([error 1007](https://docs.bigbluebutton.org/2.2/troubleshooting.html#client-webrtc-error-codes)). We set up a TURN server following the [BBB official guide for setting up a TURN server](https://docs.bigbluebutton.org/2.2/setup-turn-server.html) to solve this issue.

These are the specifications of our TURN server:

{{< server product="Hetzner CPX11" os="Ubuntu 18.04.4 (Linux 4.15.0-99-generic)" cpu="AMD EPYC Processor (IBPB, 2 vCPUs)" ram="2 GB RAM / 2 GB Swap" >}}{{< /server >}}

## Live streaming

We use [OBS Studio](https://obsproject.com/) to stream the talks to an [RTMP server](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol) which we set up following this [guide at the NGINX blog](https://www.nginx.com/blog/video-streaming-for-remote-learning-with-nginx/), see also [this guide](https://docs.peer5.com/guides/cors/) for configuring the CORS headers. We stream using [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP)) and [indigo-player](https://matvp91.github.io/indigo-player/#/).

These are the specifications of our RTMP server:

{{< server product="Hetzner CPX21" os="Ubuntu 18.04.4 (Linux 4.15.0-99-generic)" cpu="AMD EPYC Processor (3 vCPUs)" ram="4 GB RAM / 2 GB Swap" >}}{{< /server >}}

## Firewall

We use [UFW](https://launchpad.net/ufw) to protect our servers by allowing incoming traffic only through [a custom SSH port](https://www.linode.com/docs/security/securing-your-server/#harden-ssh-access) as well as the ports required by BBB to function, see for example [this guide provided by Linode](https://www.linode.com/docs/security/firewalls/configure-firewall-with-ufw/).

## Server monitoring

We use [Grafana](https://grafana.com/) and [Prometheus](https://prometheus.io/) to monitor our BBB instance. We the following [Docker container provided by a third-party developer](https://bigbluebutton-exporter.greenstatic.dev/installation/all_in_one_monitoring_stack/).

### 21.05.2020

The FD Seminar ran on a cloud server with the following specifications:

{{< server product="Linode Dedicated 16GB" os="Ubuntu 16.04 (Linux 4.4.0-179-generic)" cpu="AMD EPYC 7501 32-Core Processor (8 dedicated vCPUs)" ram="16 GB RAM / 10 GB Swap" >}}{{< /server >}}

We found these to be sub-optimal for running a single BBB session with 77 participants and 8 shared webcams (some users reported low audio quality).

[![21.05.2020](/img/grafana-21.05.2020.png)](/img/grafana-21.05.2020.png)

## Automation

We manage our servers with [Ansible](https://www.ansible.com/); the playbooks below are run in the following order: TURN &rarr; BBB &rarr; Greenlight.

### Updating BigBlueButton

{{< highlight yaml >}}
# https://docs.bigbluebutton.org/2.2/install.html
# https://docs.bigbluebutton.org/2.2/customize.html

- hosts: bbb.fd-seminar.xyz
  become: yes
  vars:
    fqdn: bbb.fd-seminar.xyz
  tasks:
  - name: register cronjob for renewing letsencrypt certs
    cron:
      name: "renew letsencrypt certs"
      minute: "30"
      hour: "2"
      weekday: "1"      
      job: "/usr/bin/certbot renew >> /var/log/le-renew.log"

  - name: register cronjob for restarting nginx after renewing letsencrypt certs
    cron:
      name: "restart nginx after renewing letsencrypt certs"
      minute: "35"
      hour: "2"
      weekday: "1"
      job: "/bin/systemctl reload nginx"
      
  - name: deny all incoming connections
    ufw:
      default: deny
      direction: incoming

  - name: allow all outgoing connections
    ufw:
      default: allow
      direction: outgoing

  - name: allow connections to port 22 (SSH) 
    ufw:
      rule: allow
      port: '22'
      # use a different port for added security 

  - name: allow connections to port 80 (HTTP)
    ufw:
      rule: allow
      port: '80'
      proto: tcp

  - name: allow connections to port 433 (TLS)
    ufw:
      rule: allow
      port: '443'
      proto: tcp

  - name: allow connections to port 16384:32768 (BBB)
    ufw:
      rule: allow
      port: '16384:32768'
      proto: udp

  - name: enable uncomplicated firewall (UFW)
    ufw:
      state: enabled

  - name: update all packages to the latest version
    apt:
      upgrade: dist
      update_cache: yes

  - name: restore BBBs FQDN
    command: bbb-conf --setip {{ fqdn }}

  - name: configure FreeSWITCH to support ipv6
    copy:
      src: files/nginx/bigbluebutton_sip_addr_map.conf
      dest: /etc/nginx/conf.d/bigbluebutton_sip_addr_map.conf
    # https://docs.bigbluebutton.org/2.2/troubleshooting.html#configure-bigbluebuttonfreeswitch-to-support-ipv6

  - name: replace static ip (v4) by $freeswitch_addr (ipv4+ipv6) in nginx sip conf (for use with FreeSwitch)
    lineinfile:
      path: /etc/bigbluebutton/nginx/sip.nginx
      regexp: 'proxy_pass'
      line: 'proxy_pass https://$freeswitch_addr:7443;'
      state: present

  - name: enable 3pcc in FreeSWITCHipv6 sip profile
    lineinfile:
      path: /opt/freeswitch/etc/freeswitch/sip_profiles/external-ipv6.xml
      regexp: '    <!--<param name="enable-3pcc" value="true"/>-->'
      line: '    <param name="enable-3pcc" value="true"/>'
      state: present

  - name: configure BBB to use our TURN server
    copy:
      src: files/coturn/turn-stun-servers.xml
      dest: /usr/share/bbb-web/WEB-INF/classes/spring/turn-stun-servers.xml
      owner: bigbluebutton
      group: bigbluebutton
      mode: u=rw,g=r,o=r
    # https://docs.bigbluebutton.org/2.2/setup-turn-server.html#configure-bigbluebutton-to-use-the-coturn-server
   
  - name: get local username
    local_action: command whoami
    become: no
    register: local_user

  - name: retrieve current TURN server static auth secret
    shell: "ssh {{ local_user.stdout }}@turn.fd-seminar.xyz cat /etc/turnserver.conf | grep static-auth-secret= | awk -F '=' '{print $NF}'"
    register: current_secret
    delegate_to: 127.0.0.1
    become_user: "{{ local_user.stdout }}"

  - name: update TURN server static auth secret in BBBs conf
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/spring/turn-stun-servers.xml
      regexp: 'STATIC_AUTH_SECRET'
      replace: "{{ current_secret.stdout }}"

  - name: set URL for default presentation in BBB sessions
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'beans.presentationService.defaultUploadedPresentation=.*'
      replace: 'beans.presentationService.defaultUploadedPresentation=https://www.fd-seminar.xyz/pdf/bbb-howto.pdf'

  - name: set default welcome message in BBB sessions
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'defaultWelcomeMessage=.*'
      replace: 'defaultWelcomeMessage=Welcome to <b>%%CONFNAME%%</b>!<br><br>A live stream of the talk is also available at <a href="https://www.fd-seminar.xyz/live" target="_blank"><u>https://www.fd-seminar.xyz/live</u></a><br><br>Username: fd-seminar<br>Password: access code for this meeting'

  - name: set default welcome message (footer) in BBB sessions
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'defaultWelcomeMessageFooter=.*'
      replace: 'defaultWelcomeMessageFooter=Thank you for joining!'

  - name: set maximum presentation page number
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'maxNumPages=.*'
      replace: 'maxNumPages=1000'

  - name: set maximum presentation file size
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'maxFileSizeUpload=.*'
      replace: 'maxFileSizeUpload=30000000'

  - name: mute meetings on start by default
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'muteOnStart=.*'
      replace: 'muteOnStart=true'

  - name: force BBB HTML5 client (participants)
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'attendeesJoinViaHTML5Client=.*'
      replace: 'attendeesJoinViaHTML5Client=true'

  - name: force BBB HTML5 client (moderators)
    replace:
      path: /usr/share/bbb-web/WEB-INF/classes/bigbluebutton.properties
      regexp: 'moderatorsJoinViaHTML5Client=.*'
      replace: 'moderatorsJoinViaHTML5Client=true'

  - name: disable "you are now muted" feedback sound
    replace:
      path: /opt/freeswitch/etc/freeswitch/autoload_configs/conference.conf.xml
      regexp: '      <param name="muted-sound" value="conference/conf-muted.wav"/>'
      replace: '      <!-- <param name="muted-sound" value="conference/conf-muted.wav"/> -->'

  - name: disable "you are now unmuted" feedback sound
    replace:
      path: /opt/freeswitch/etc/freeswitch/autoload_configs/conference.conf.xml
      regexp: '      <param name="unmuted-sound" value="conference/conf-unmuted.wav"/>'
      replace: '      <!-- <param name="unmuted-sound" value="conference/conf-unmuted.wav"/> -->'

  - name: set camera defaults to reduced bitrates (to reduce bandwidth)
    shell: >
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[0].bitrate 50
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[1].bitrate 100
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[2].bitrate 200
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[3].bitrate 300
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[0].default true
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[1].default false
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[2].default false
      yq w -i /usr/share/meteor/bundle/programs/server/assets/app/config/settings.yml public.kurento.cameraProfiles.[3].default false

  - name: delete raw data from unpublished recordings after 2 days
    replace:
      path: /etc/cron.daily/bigbluebutton
      regexp: 'recorded_days=.*'
      replace: 'recorded_days=2'

  - name: delete raw data from published recordings after 2 days
    replace:
      path: /etc/cron.daily/bigbluebutton
      regexp: 'published_days=.*'
      replace: 'published_days=2'

  - name: restart BigBlueButton
    command: bbb-conf --restart
    
  - name: restart nginx
    service:
      name: nginx
      state: restarted
      enabled: yes    
{{< / highlight >}}

### Updating Greenlight

{{< highlight yaml >}}
# https://docs.bigbluebutton.org/greenlight/gl-customize.html

- hosts: bbb.fd-seminar.xyz
  tasks:
  - name: update greenlight sources
    command: git pull upstream master
    args:
      chdir: /var/docker/greenlight
    become: yes

  - name: push greenlight sources to sourcehut
    command: git push origin fd-seminar
    args:
      chdir: /var/docker/greenlight
    become: yes

  - name: stop greenlight
    command: docker-compose down
    args:
      chdir: /var/docker/greenlight
    become: yes

  - name: rebuild greenlight
    command: /var/docker/greenlight/scripts/image_build.sh fd-seminar-bbb release-v2
    args:
      chdir: /var/docker/greenlight
    become: yes

  - name: start greenlight
    command: docker-compose up -d
    args:
      chdir: /var/docker/greenlight
    become: yes
{{< / highlight >}}

### Updating the TURN server

{{< highlight yaml >}}
# https://docs.bigbluebutton.org/2.2/setup-turn-server.html

- hosts: turn.fd-seminar.xyz
  become: yes
  tasks:
  - name: deny all incoming connections
    ufw:
      default: deny
      direction: incoming

  - name: allow all outgoing connections
    ufw:
      default: allow
      direction: outgoing

  - name: allow connections to port 22 (SSH)
    ufw:
      rule: allow
      port: '22'
      # use a different port for added security

  - name: allow connections to port 80 (HTTP)
    ufw:
      rule: allow
      port: '80'
      proto: tcp

  - name: allow connections to port 443/tcp (TLS)
    ufw:
      rule: allow
      port: '443'
      proto: tcp

  - name: allow connections to port 433/udp (TLS)
    ufw:
      rule: allow
      port: '443'
      proto: udp

  - name: allow connections to port 3478/tcp (COTURN)
    ufw:
      rule: allow
      port: '3478'
      proto: tcp

  - name: allow connections to port 3478/udp (COTURN)
    ufw:
      rule: allow
      port: '3478'
      proto: udp

  - name: allow connections to port 49152:65535/udp (relay ports)
    ufw:
      rule: allow
      port: '49152:65535'
      proto: udp

  - name: enable uncomplicated firewall (UFW)
    ufw:
      state: enabled

  - name: update all packages to the latest version
    apt:
      upgrade: dist
      update_cache: yes

  - name: full system update
    apt:
      update_cache: yes
      upgrade: dist

  - name: install coturn
    apt:
      name: coturn
      state: present

  - name: set TLS listening port to port 443 (to bypass firewalls)
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'tls-listening-port=.*'
      line: 'tls-listening-port=443'

  - name: enable fingerprints in TURN messages
    lineinfile:
      path: /etc/turnserver.conf
      regexp: '#fingerprint'
      line: 'fingerprint'
      state: present

  - name: enable long-term credential mechanism
    lineinfile:
      path: /etc/turnserver.conf
      regexp: '#lt-cred-mech'
      line: 'lt-cred-mech'
      state: present

  - name: enable secret-based authentication
    lineinfile:
      path: /etc/turnserver.conf
      regexp: '#use-auth-secret'
      line: 'use-auth-secret'
      state: present

  - name: generated static auth secret
    shell: openssl rand -hex 16
    register: new_secret

  - name: update static auth secret
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'static-auth-secret=.*'
      line: 'static-auth-secret={{ new_secret.stdout }}'
      state: present

  - name: set default realm
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'realm=.*'
      line: 'realm=fd-seminar.xyz'
      state: present

  - name: set path to let's encrypt certificate file
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'cert=.*'
      line: 'cert=<path to certificate file>'
      state: present

  - name: set path to let's encrypt private key
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'pkey=.*'
      line: 'pkey=<path to certificate private key>'
      state: present

  - name: limit the allowed ciphers
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'cipher-list=.*'
      line: 'cipher-list="ECDH+AESGCM:ECDH+CHACHA20:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS"'
      state: present

  - name: enable longer DH TLS key
    lineinfile:
      path: /etc/turnserver.conf
      regexp: '#dh2066'
      line: 'dh2066'
      state: present

  - name: set path to log file
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'log-file=.*'
      line: 'log-file=/var/log/coturn.log'
      state: present

  - name: enable logging into a single file
    lineinfile:
      path: /etc/turnserver.conf
      regexp: '#simple-log'
      line: 'simple-log'
      state: present
  
  - name: disable TLS/DTLS protocols v1
    lineinfile:
      path: /etc/turnserver.conf
      regexp: '#no-tlsv1'
      line: 'no-tlsv1'
      state: present

  - name: disable TLS/DTLS protocols v1_1
    lineinfile:
      path: /etc/turnserver.conf
      regexp: '#no-tlsv1_1'
      line: 'no-tlsv1_1'
      state: present

  - name: log to a single file
    lineinfile:
      path: /etc/turnserver.conf
      regexp: 'log-file=.*'
      line: 'log-file=/var/log/coturn.log'
      state: present

  - name: copy log rotation conf
    copy:
      src: files/coturn/coturn
      dest: /etc/logrotate.d/coturn
      owner: root
      group: root
      mode: u=rw,g=r,o=r
    # https://docs.bigbluebutton.org/2.2/setup-turn-server.html#configure-log-rotation
    
  - name: enable coturn service in conf file
    lineinfile:
      path: /etc/default/coturn
      regexp: '#TURNSERVER_ENABLED=.*'
      line: 'TURNSERVER_ENABLED=1'
      state: present

  - name: enable coturn service
    service:
      name: coturn
      state: restarted
      enabled: yes
{{< / highlight >}}
