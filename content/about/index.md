+++
title = "About"
menu = "main"
type = "about"
+++

## About the FD Seminar

The FD Seminar is a weekly online seminar on representation theory of quivers and finite-dimensional algebras. The seminar aims to become a virtual meeting space for representation-theorists worldwide (transcending the COVID-19 crisis which catalysed its inception), and wishes also to provide an international forum for researchers who may otherwise not have access to such. 

The seminar takes place every Thursday at 13:00 UTC during Summer Time and 14:00 UTC during Winter Time (times are automatically displayed in your local timezone elsewhere in the website), and will be suspended during certain holidays and to prevent clashes with important events in the field (e.g. the ICRA). These suspensions will be announced in this website and through [our mailing list](https://listen.uni-bonn.de/wws/info/fd-seminar).

The seminar is organised by

- [Eleonore Faber](http://www1.maths.leeds.ac.uk/~pmtemf/) (Leeds)
- [Gustavo Jasso](http://www.math.uni-bonn.de/people/gjasso/) (Bonn)
- [Ryan Kinser](https://homepage.math.uiowa.edu/~rkinser/) (Iowa)
- [Julian Külshammer](https://katalog.uu.se/profile/?id=N18-1115) (Uppsala)
- [Rosanna Laking](http://profs.scienze.univr.it/laking/) (Verona)
- [Alexandra Zvonareva](https://www.iaz.uni-stuttgart.de/institut/team/Zvonareva-00001/) (Stuttgart)

Please send your comments and suggestions (including suggestions of potential speakers) by e-mail to [organisers@fd-seminar.xyz](mailto:organisers@fd-seminar.xyz).

### Funding

For the first two years, the FD Seminar is funded by the [Hausdorff Center for Mathematics](https://www.hcm.uni-bonn.de/) of the [Rheinische Friedrich-Wilhelms-Universität Bonn](https://www.uni-bonn.de/).

### Group photo

We thank Bernhard Keller for providing the following group photo, taken after his talk on 03.09.2020 to commemorate the resumption of the FD Seminar after the summer break.

[![03.09.2020](/img/group-photo-03.09.2020.jpg)](/img/group-photo-03.09.2020.jpg)

## Information for participants

The FD Seminar runs on [BigBlueButton](https://bigbluebutton.org/), an open-source web conferencing system.  The simplest way to join the talks is using the direct link and access code which are distributed through [our mailing list](https://listen.uni-bonn.de/wws/info/fd-seminar).

Although you do not need to register to attend talks in the seminar, creating an account in [our BigBlueButton instance](https://bbb.fd-seminar.xyz) will allow you to watch the recordings of previous talks.

If you experience problems registering or using BigBlueButton, please read the troubleshooting information [below](/about/#troubleshooting).

{{< note >}}We **strongly recommend** to use the latest version of the following web browsers when joining the FD Seminar:
* **Desktop:** [Google Chrome](https://www.google.com/chrome/)/Chromium or [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/new/) 
* **iOS:** [Safari](https://www.apple.com/safari/)
* **Android:** [Google Chrome](https://www.google.com/chrome/){{< /note >}}

### Live stream

The talks will also be [live-streamed](https://www.fd-seminar.xyz/live). The login credentials for accessing the live stream are as follows:
```
username: fd-seminar
password: access code for the BigBlueButton session
```

## Information for speakers

We recommend that you create an account in [our BigBlueButton instance](https://bbb.fd-seminar.xyz). Please inform us after you do so; we will then give your account the role of 'speaker' so you can test-drive the software before your talk. During your talk you will be able to upload a PDF file which will be shown in the participants' screens (for example, a LaTeX beamer presentation). This is much less resource-intensive than sharing your screen with the PDF file open in your own computer. BigBlueButton provides basic annotation functionalities which we encourage you to experiment with before your talk. We also suggest that you allow us to make your PDF file available for download as it will be helpful for participants to have access to it in a different application during your talk; this can be done directly through BigBlueButton during the talk.

In addition, BigBlueButton provides the usual web-conferencing functionalities: public and private chat, screen sharing, polling, status icons, etc. We recommend you to watch some of the [video tutorials](https://bigbluebutton.org/html5/) for more details.

When submitting your abstract, please include a list of up to five keywords; these will help us improve the results of our search function in the long term. Mathematical symbols are rendered using \\(\KaTeX\\); see [here](https://katex.org/docs/supported.html) for the list of available \\(\TeX\\) functions. For example,
```
\\[ \operatorname{Ext}_A^1(M,N)\cong D\operatorname{\overline{Hom}}_A(N,\tau M) \\]
```
is displayed as \\[ \operatorname{Ext}_A^1(M,N)\cong D\operatorname{\overline{Hom}}_A(N,\tau M), \\]
while `\\( \operatorname{mod}A \\)` will be rendered inline as \\(\operatorname{mod}A\\) (note the double backslashes).

### Recordings 

By default, talks will not be recorded. If you are interested in your talk being recorded please let us know in advance. The recording will then be hosted in our BigBlueButton instance, where it can be viewed by any registered user. As a speaker, you retain ownership of the recording and can request its removal from our server at any point.

We are also happy to provide you with a recording of your talk. Due to technical limitations, it is not possible to download recordings from BigBlueButton; a second recording will be produced with [OBS Studio](https://obsproject.com/) for this purpose. Let us know if you need such copy.

Thank you for contributing to the FD Seminar!

## Technical information

### Troubleshooting

#### Problems registering

After a successful registration to our BigBlueButton instance, you should be able to sign in and see the following screen:

[![Greenlight](/img/greenlight.png)](/img/greenlight.png)

{{< note >}}
In case you cannot log in to your BigBlueButton account (e.g. because you did not receive the e-mail verification message), **you can simply join the talk with the direct link and access code** which is distributed through our mailing list.
{{< /note >}}

Some participants have reported some issues registering in our BigBlueButton instance:

* Missing e-mail verification messages, possibly due to aggressive spam filters put in place by some institutions (in one case the message was flatly rejected). Some users have received the e-mail verification message only after a couple of days, possibly because the message had been quarantined.

  In this case you have the following options:
  
  * Create an account using a different e-mail address; please [contact us](mailto:admin@fd-seminar.xyz) in case you think we will not recognise your address. 
  
  * Contact the IT department at your institution and ask them to whitelist our domain name (fd-seminar.xyz) so that our e-mails no longer go to the spam/quarantine folder.

* Some users received an error (404) after clicking on the verification link. This is probably due to some anti-phishing system pre-loading the e-mail verification page. You can try to sign in since your e-mail address was probably verified successfully (despite the error).

* Some users reported not being able to click the 'Verify Account' button in the e-mail verification message (clicking on the button had no discernible effect). Again, this is possibly due to an anti-phishing mechanism preventing users from clicking on suspicious links. You can circumvent this protection by _right-clicking_ on the 'Verify account' button and selecting the option 'Copy link'; you can then paste the link in the address bar in your web browser and verify your account.

Note that, unless you need to reset your password, the verification e-mail is the only important message that you will receive from our BigBlueButton instance. The weekly announcement are sent through our mailing list which is hosted at the University of Bonn (so hopefully these will not go to your spam folder).

Please write to [admin@fd-seminar.xyz](mailto:admin@fd-seminar.xyz) if you have any problems registering. We apologise for any inconveniences.

#### Joining the audio session

{{< note >}}
Some users reported an **error (1007)** when trying to join the audio session (and thus they could not hear anything). We have implemented a technical fix to address this issue; please do not hesitate in contacting us if you experience this problem.
{{< /note >}}

The user experience when joining a BigBlueButton session can be confusing. After entering the FD Seminar room you will be presented with the following choice:

![Joining the audio session](/img/joining_the_audio_session.png)

If you select **Microphone**, BigBlueButton will run an echo test on your microphone and you will then enter the session with your microphone enabled (but muted by default).

If you select **Listen only** you will enter the session with the microphone _disabled_ and you will not be able to unmute yourself to ask a question. You can re-join the audio session by clicking on the 'Leave audio' button (coloured blue and marked with headphones):

![Leave audio](/img/leave_audio.png)

You can then click on the 'Join audio' button (marked with a crossed-out phone) and select to join the session by **Microphone**:

![Join audio](/img/join_audio.png)

Alternatively, you can reload the page in your web browser and select to join the audio session by **Microphone**.

### Mailing list

Our mailing list is hosted at the [Hochschulrechenzentrum der Universität Bonn](http://www.hrz.uni-bonn.de/). To prevent [Zoombombing](https://en.wikipedia.org/wiki/Zoombombing) all registrations must be approved manually by an administrator. We ask you to use the name you normally use to identify yourself within our community as well as as your institutional email address; if you do not have one, please write to [admin@fd-seminar.xyz](mailto:admin@fd-seminar.xyz). Thank you for your understanding.

### Website

Our website is built with [Hugo](https://gohugo.io/), an open-source static website generator; it uses a modified version of the [Hyde-Hyde theme](https://themes.gohugo.io/hyde-hyde/) by [htr3n](https://htr3n.github.io/) as well as [Google Fonts](https://fonts.google.com/). We use JavaScript for [displaying times in your local timezone](https://momentjs.com/), [rendering mathematical formulae](https://katex.org/), [searching the talk archive](https://fusejs.io/), and [displaying the map of speakers](https://leafletjs.com/) in the archives.

The source code for the website is available [here](https://git.sr.ht/~gjasso/fd-seminar-www).

> Due to an influx of spam registrations to our BigBlueButton instance, we added a [reCAPTCHA](https://www.google.com/recaptcha/about/) widget (provided by Google) to the registration form. If you are already registered, this will not affect you; if not, please take this into account when registering to our server.

### Server

{{< note >}}If you are interested in how we set up our server, click [here](/diy).{{< /note >}}

Our servers are located in Falkenstein, Germany and are rented from the internet hosting company [Hetzner](https://www.hetzner.com/).

If you create an account in our BigBlueButton instance, the following information will be stored in our servers:

* The e-mail address you use to create your account.
* The password you use to create your account; for your own protection, we strongly recommend that you use a new password for your account.
* The name you enter when creating your account.

We have made slight modifications to the web interface to our BigBlueButton instance; the source code is available [here](https://git.sr.ht/~gjasso/fd-seminar-greenlight).

Our SSL/TLS certificates (needed for encrypting the traffic through our website and BigBlueButton server) are provided by [Let's Encrypt](https://letsencrypt.org).
