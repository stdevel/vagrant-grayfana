# vagrant-grayfana
This repository contains a Vagrantfile for a client/server scenario leveraging Graylog and Grafana.
It is designed as an example lab for checking-out system logging and visualization.

# Requirements
Make sure to install the following tools:
- [Oracle VirtualBox](https://virtualbox.org)
- [Hashicorp Vagrant](https://vagrantup.com)

# Scenario
This scenario consists of two VMs based on Debian Stretch 9.x:
- Graylog server
  - Graylog
  - MongoDB
  - Elasticsearch 6.x
- Client system
  - running Apache2
  - configured to forward all syslog data to Graylog server
  - configured with dummy website and posting custom logging via GELF to Graylog

![Scenario](https://raw.githubusercontent.com/stdevel/vagrant-grayfana/master/Scenario.png "Scenario")

# Usage
Simply clone this repository or unzip the archive, open a terminal and move to the folder before entering the following command:
```
$ vagrant up
```

Afterwards, you can access the following URLs:

| URL | Description |
|:----|:------------|
| http://localhost:9000 | Graylog interface on ``graylog`` |
| http://localhost:3000 | Grafana interface on ``graylog`` |
| http://localhost:8080 | Apache2 web server on ``client`` |

For Graylog, the default password assigned via Vagrant is ``test123``, the Grafana default credentials are ``admin`` / ``admin``.

## Further steps
The next steps include:
- Configure a Syslog TCP input
- Configure a GELF UDP input
- Login into Grafana and create an Elasticsearch data source
- Create a Grafana dashboard

To do this, proceed with the following:
1. Login into Graylog via http://localhost:9000
2. Click ``System`` > ``Inputs``
3. Select ``Syslog TCP`` from the dropdown menu and click ``Launch new Input``
4. In the form, select the ``graylog`` node and enter port ``1514``
5. Click ``Save`` and ``Start Input``
6. Click ``System`` > ``Inputs``
7. Select ``GELF UDP`` from the dropdown menu and click ``Launch new Input``
8. In the form, select the ``graylog`` node and enter port ``12201``
9. Click ``Save`` and ``Start Input``
10. Login into Grafana via http://loacalhost:3000
11. Click ``Skip`` or change the administrator password
12. Select ``Elasticsearch`` from the source type
13. In the form, enter the following:
  - URL: ``http://localhost:9200``
  - Index name: ``graylog_0``
  - Time field name: ``timestamp``
  - Version: ``6+`
14. Click ``Save & Test``
15. Import the dashboard by clicking ``Dashboard`` > ``Manage`` > ``Import`` > ``Upload .json file``
16. Click ``Import``

To fill the Graylog and Grafana with senseful data, start some web server requests, e.g.:

```
$ while true; do curl http://localhost:8080; done
```

Check-out the Graylog inputs and Grafana dashboard!

![Grafana dashboard](https://raw.githubusercontent.com/stdevel/vagrant-grayfana/master/Grafana_dashboard.png "Grafana dashboard")
