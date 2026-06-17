InterDiode
==========

![architecture](https://interdiode.fr/wp-content/uploads/2024/10/interdiode.png)

InterDiode helps you to comfortably work with a proper air-gapped network. InterDiode gathers many resources from the internet and make them available on your internal, secured, network. You need two InterDiode servers, the first one being connected to the Internet and pushing downloaded data to the second, offline, server.

InterDiode copies programming language repositories, documentation, emails, or RSS feeds along with their linked web pages. All data is transferred through a data diode; you can either use your own data diode protocol or utilize the built-in one. With Interdiode:
- Your USB drives will no longer travel back and forth between the internet and your internal network.,
- third-party packages or libraries will always be up-to-date,
- Developers are happier and more productive with such a better environment 🙂

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

Installation
------------

The easiest way is to use the provided Docker images with the following docker compose file.

:download:`example script </_downloads/contributing/example_script.py>`.

Data diodes
-----------

Diodes allow data to be transferred securely from an internet-connected network to a disconnected network.
However, they do not directly address the issue of retrieving this data from the internet, nor that of making it available on the internal network.
InterDiode is designed to solve these two problems, relying on an existing diode for the transfer itself.

If you cannot acquire a certified diode such as the Thales Elips SD, you can easily build your own with affordable hardware
such as three TP-Link MC100CM or MC200CM transceivers. InterDiode allows direct use of a diode in UDP, without investing in additional software.
You can look at [Dyode](https://github.com/wavestone-cdt/dyode/) to build your own diode with _three_ transceivers
(the third one is connected to the RX of the emitter to bypass the link failure protection).

Local development
-----------------

Start the required services with the following command:

```bash
docker-compose up -d
export DATABASE_URL="postgresql://username:password@127.0.0.1:5432/database?ssl_check_hostname=true&ssl_cert_reqs=required&ssl_certfile=./id_tools/tox/localhost.crt&ssl_keyfile=./id_tools/tox/localhost.key&ssl_ca_certs=./id_tools/tox/CA.crt"
export REDIS_URL="rediss://:password@127.0.0.1:6379/1?ssl_check_hostname=true&ssl_certfile=./id_tools/tox/localhost.crt&ssl_keyfile=./id_tools/tox/localhost.key&ssl_ca_certs=./id_tools/tox/CA.crt"
poetry run python3 -m interdiode configuration apply
poetry run python3 -m interdiode run http &
poetry run python3 -m interdiode run beat &
poetry run python3 -m interdiode run background
```

Then, you can access the web interface at http://localhost:8000/.
You can also create a `local_settings.py` file to override the default settings.
