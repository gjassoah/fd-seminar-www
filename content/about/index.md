+++
title = "About"
menu = "main"
type = "about"
+++

## About the FD Seminar

The FD Seminar is a weekly online seminar on representation theory of quivers and finite-dimensional algebras. The seminar aims to become a virtual meeting space for representation-theorists worldwide (transcending the COVID-19 crisis which catalysed its inception), and wishes also to provide an international forum for researchers who may otherwise not have access to such. 

The seminar takes place every Thursday at 12:00 UTC (times are displayed in your local timezone elsewhere in the website), and will be suspended during certain holidays and to prevent clashes with important events in the field (e.g. the ICRA). These suspensions will be announced in this website and through [our mailing list](https://listen.uni-bonn.de/wws/info/fd-seminar).

The seminar is organised by

- [Eleonore Faber](http://www1.maths.leeds.ac.uk/~pmtemf/) (Leeds)
- [Gustavo Jasso](http://www.math.uni-bonn.de/~schroer/) (Bonn)
- [Ryan Kinser](https://homepage.math.uiowa.edu/~rkinser/) (Iowa)
- [Julian Külshammer](https://katalog.uu.se/profile/?id=N18-1115) (Uppsala)
- [Rosanna Laking](http://profs.scienze.univr.it/laking/) (Verona)
- [Alexandra Zvonareva](https://www.iaz.uni-stuttgart.de/institut/team/Zvonareva-00001/) (Stuttgart)

Please send your comments and suggestions (including suggestions of potential speakers) by e-mail to [organisers@fd-seminar.xyz](mailto:organisers@fd-seminar.xyz).

## Information for participants

The FD Seminar runs on [BigBlueButton](https://bigbluebutton.org/), an open-source web conferencing system. Although you do not need to register to attend talks in the seminar, creating an account in [our BigBlueButton instance](https://bbb.fd-seminar.xyz) will allow you to watch the recordings of previous talks. If you do not wish to create an account, you will need to [subscribe to our mailing list](https://listen.uni-bonn.de/wws/info/fd-seminar) where the password for the upcoming talk will be distributed. We will also use the mailing list to inform you if the upcoming speaker wishes to have their talk recorded; please take this into consideration when deciding to attend the talk.

## Information for speakers

We recommend that you create an account in [our BigBlueButton instance](https://bbb.fd-seminar.xyz). Please inform us after you do so; we will then give your account the role of 'speaker' so you can test-drive the software before your talk. During your talk you will be able to upload a PDF file which will be shown in the participants' screens (for example, a LaTeX beamer presentation). This is much less resource-intensive than sharing your screen with the PDF file open in your own computer. BigBlueButton provides basic annotation functionalities which we encourage you to experiment with before your talk. We also suggest that you allow us to make your PDF file available for download as it will be helpful for participants to have access to it in a different application during your talk; this can be done directly through BigBlueButton during the talk.

In addition, BigBlueButton provides the usual web-conferencing functionalities: public and private chat, screen sharing, polling, status icons, etc. We recommend you to watch some of the [video tutorials](https://bigbluebutton.org/html5/) for more details.

When submitting your abstract, please include a list of up to five keywords; these will help us improve the results of our search function in the long term. Mathematical symbols are rendered using \\(\KaTeX\\); see [here](https://katex.org/docs/supported.html) for the list of available \\(\TeX\\) functions. For example,
```
\\[ \operatorname{Ext}_A^1(M,N)\cong D\operatorname{\underline{Hom}}_A(\tau^{-1}N,M)\cong D\operatorname{\overline{Hom}}_A(N,\tau M) \\]
```
is displayed as \\[ \operatorname{Ext}_A^1(M,N)\cong D\operatorname{\underline{Hom}}_A(\tau^{-1}N,M)\cong D\operatorname{\overline{Hom}}_A(N,\tau M), \\]
while `\\( \operatorname{mod}A \\)` will be rendered inline as \\(\operatorname{mod}A\\) (note the double backslashes).
### Recordings 

By default, talks will not be recorded. If you are interested in your talk being recorded please let us know in advance so we can inform the participants of this through our mailing list. The recording will then be hosted in our BigBlueButton instance, where it can be viewed by any registered user. As a speaker, you retain ownership of the recording and can request its removal from our server at any point (we are also happy to provide you with a copy of the recording).

Thank you for contributing to the FD Seminar!

## Technical information

### Mailing list

Our mailing list is hosted at the [Hochschulrechenzentrum der Universität Bonn](http://www.hrz.uni-bonn.de/). To prevent [Zoombombing](https://en.wikipedia.org/wiki/Zoombombing) all registrations must be approved manually by an administrator. We ask you to use the name you normally use to identify yourself within our community as well as as your institutional email address; if you do not have one, please write to [admin@fd-seminar.xyz](mailto:admin@fd-seminar.xyz). Thank you for your understanding.

### Server

Currently, our server is located in Frankfurt, Germany and is rented from the cloud-hosting company [Linode](https://www.linode.com/).

 If you create an account in our BigBlueButton instance, the following information will be stored in our server:

* The e-mail address you use to create your account.
* The password you use to create your account; for your own protection, we strongly recommend that you use a new password for your account.
* The name you enter when creating your account.

Our SSL/TLS certificates (needed for encrypting the traffic through our website and BigBlueButton instance) are provided by [Let's Encrypt](https://letsencrypt.org).

{{< note >}}If you are interested in how we set-up our server, click [here](/diy).{{< /note >}}

### Website

Our website is built with [Hugo](https://gohugo.io/), an open-source static website generator; it uses a modified version of the [Hyde-Hyde theme](https://themes.gohugo.io/hyde-hyde/) by [htr3n](https://htr3n.github.io/) as well as [Google Fonts](https://fonts.google.com/). We use JavaScript for [displaying times in your local timezone](https://momentjs.com/), [rendering mathematical formulae](https://katex.org/), and [searching the talk archive](https://fusejs.io/).
