# Internet Pi

**A Raspberry Pi Configuration for Internet connectivity**

I have had a couple Pis doing random Internet-related duties for years. It's finally time to formalize their configs and make all the DNS/ad-blocking/monitoring stuff encapsulated into one Ansible project.

So that's what this is.

## Features

**Internet Monitoring**: Installs an [`internet-monitoring` Docker config](https://github.com/geerlingguy/internet-monitoring), which exposes a Grafana dashboard with historical uptime, ping stats, and speedtest results over time.

![Internet Monitoring Dashboard in Grafana](/images/internet-monitoring.png)

**Pi-hole**: Installs the Pi-hole Docker configuration so you can use Pi-hole for network-wide ad-blocking and local DNS. Make sure to update your network router config to direct all DNS queries through your Raspberry Pi if you want to use Pi-hole effectively!

![Pi-hole on the Internet Pi](/images/pi-hole.png)

Other features:

  - **Shelly Plug Monitoring**: Installs a [`shelly-plug-prometheus` exporter](https://github.com/geerlingguy/shelly-plug-prometheus) and a Grafana dashboard, which tracks and displays power usage on a Shelly Plug running on the local network. (This is disabled by default. Enable and configure using the `shelly_plug_*` vars in `config.yml`.)
  - **Starlink Monitoring**: Installs a [`starlink` prometheus exporter](https://github.com/danopstech/starlink_exporter) and a Grafana dashboard, which tracks and displays Starlink statistics. (This is disabled by default. Enable and configure using the `starlink_enable` var in `config.yml`.)

**IMPORTANT NOTE**: If you use this playbook, it will download a decently-large amount of data through your Internet connection on a daily basis. Don't use it, or tune the `internet-monitoring` setup to not run the speedtests as often, if you have a metered connection!

## Setup

  1. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (either full version or ansible-base): `pip3 install ansible`.
  1. Colone this repo `git clone https://upamanyudas@github.com/upamanyudas/internet-monitoring-pi.git`
  1. Install requirements: `ansible-galaxy collection install -r requirements.yml`
  1. Make copies of the following files and customize them to your liking:
    - `example.inventory.ini` to `inventory.ini` (replace IP address with your Pi's IP, or comment that line and uncomment the `connection=local` line if you're running it on the Pi you're setting up).
    - `example.config.yml` to `config.yml`
  1. Run the playbook: `ansible-playbook main.yml`

## Logging in

Grafana has an admin username and password by default; currently this is not easily configurable, but follow [this issue](https://github.com/geerlingguy/internet-pi/issues/23) for progress making it more configurable.

The default is `admin`/`wonka`.

## Updating and Backup

A guide for backing up your configurations and monitoring data, and for keeping everything up to date is being worked on in [Issue #7: Create upgrade / update guide](https://github.com/geerlingguy/internet-pi/issues/7).

## License

MIT

## Author

This project was created in 2021 by [Jeff Geerling](https://www.jeffgeerling.com/).
