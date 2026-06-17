InterDiode
==========

![architecture](https://interdiode.fr/wp-content/uploads/2024/10/interdiode.png)

InterDiode helps you to comfortably work with a proper air-gapped network. InterDiode gathers many resources from the internet and makes them available on your internal, secured, network. You need two InterDiode servers, the first one being connected to the Internet and pushing downloaded data to the second, offline, server.

InterDiode copies programming language repositories, documentation, emails, or RSS feeds along with their linked web pages. All data is transferred through a data diode; you can either use your own data diode protocol or utilize the built-in one. With Interdiode:
- Your USB drives will no longer travel back and forth between the internet and your internal network.,
- third-party packages or libraries will always be up-to-date,
- Developers are happier and more productive with a better environment 🙂

A complete authorization system is provided, allowing only allowed users to select which data is gathered and transferred to your red network.


Overview
--------

Visit [our website](https://interdiode.fr/static/latest/index.html) for documentation.


<video controls width="100%" preload="metadata">
  <source src="https://interdiode.fr/static/latest/_static/examples/demo-black.mp4" type="video/mp4">
  Your browser does not support the video tag.
  You can watch it here: https://interdiode.fr/static/latest/_static/examples/demo-black.mp4
</video>


Features
--------

Developer tools

All developers are now accustomed to working with numerous online tools. Here they are, now available on your internal network!
* Ansible roles,
* Helm charts,
* Dash / Zeal documentations,
* Ends of life of well-known softwares,
* Plain Git repositories,
* GitHub repositories with associated wikis and issues,
* Popular JetBrains IDEs, libraries and plugins,
* ReadTheDocs documentations,
* Any ZIP archive of HTML files, that can be some selected documentation from a public website,
* Containers from Docker registries,
* Boxes from Vagrant repositories.
* System and programming language packages

The primary use of InterDiode is copying and making available libraries or packages for numerous programming languages or operating systems.
* Complete mirror of APK (Alpine) repositories,
* Complete clones of APT (Debian/Ubuntu) repositories or private ones,
* Complete clones of Yum/DNF (Fedora/CentOS/RedHat) repositories,
* Software released via GitHub releases,
* C# .Net packages from NuGet repositories,
* Go packages from golang.org,
* Java (and similar languages) packages from Maven-compatible indexes,
* JavaScript packages from NPM-compatible indexes,
* PHP packages from Packagist,
* Python packages from Pypi-compatible indexes,
* Ruby packages from Rubygems-compatible indexes,
* Rust packages from Cargo-compatible indexes.
* Misc internet tools

In addition to these copying functions, you also have a number of very useful utilities.
* Paste text on the black side and copy it on the red side, like a pastebin,
* Copy public GPG keys through a HKP server,
* Download links found in a given URL,
* Regularly fetch a link to make it available on the red network,
* Copy RSS feeds and the linked pages as PDF files,
* Downloads videos from standard video platforms,
* ZIM archives, like Wikipedia or StackOverflow dumps,
* Transfer raw uploaded files,
* Transfer emails from an IMAP server to internal email accounts,
* Plugins for Grafana.


Requirements
------------

InterDiode requires the following to run:
- Two servers, one connected to the internet (called "the black server") and the other one disconnected (called "the red one").
- A one-way communication channel between the two servers, that can be either:
  - an actual data diode allowing only UDP packets,
  - a firewall allowing only UDP or TCP packets with only empty packets going from the red server to the black one,
  - any method that you can think of, as long as it takes files from a directory on the black server and puts them in a directory on the red server (USB drives, DVDs, etc.).
- The requirements on the servers depend on the amount of data you want to transfer and the number of users you want to support, but you can start with 4 CPUs and 16GB of RAM.
- Of course, the disk space on the servers must be large enough to store all the data you want to transfer and to keep.
  Most of the data can be stored either in a block storage mounted on all InterDiode processes or in a S3 (slower), even if a local storage is always required.
  Moreover, for the sake of simplicity, the data to transfer must be stored several times, even if these temporary data is removed after the transfer.
- The ability to run Docker containers on both servers.


Local demo
-----------------

Start the required services with the following command:

```bash
   export EMAIL=<enter_your_email_here>
   export NAME=<enter_your_name_here>
   curl -o compose.yaml https://interdiode.fr/static/latest/_static/examples/compose.yaml
   echo "LICENSE_KEY=$(curl -L https://license.interdiode.fr/v1/license/\?product_id=42\&buyer_name=${NAME}\&buyer_email=${EMAIL}\&order_id=1)" > compose.env
   docker compose up -d
```

* Once all the containers are running, you can access the web interfaces of both instances at [http://localhost:8881/](http://localhost:8881/)_ for the black instance and [http://localhost:8882/](http://localhost:8882/) for the red instance.
* Access to the [black instance](http://localhost:8881/) and create a new account with the "Create account" button. Then, log in with the newly created account.
* In the "Administration" section, add the license file that you obtained from the InterDiode team. The license will be automatically applied to the red instance.
* Access to the [red instance](http://localhost:8882/) and create a new account with the "Create account" button. Then, log in with the newly created account.


Data diodes
-----------

Diodes allow data to be transferred securely from an internet-connected network to a disconnected network.
However, they do not directly address the issue of retrieving this data from the internet, nor that of making it available on the internal network.
InterDiode is designed to solve these two problems, relying on an existing diode for the transfer itself.

If you cannot acquire a certified diode such as the Thales Elips SD, you can easily build your own with affordable hardware
such as three TP-Link MC100CM or MC200CM transceivers. InterDiode allows direct use of a diode in UDP, without investing in additional software.
You can look at [Dyode](https://github.com/wavestone-cdt/dyode/) to build your own diode with _three_ transceivers
(the third one is connected to the RX of the emitter to bypass the link failure protection).
