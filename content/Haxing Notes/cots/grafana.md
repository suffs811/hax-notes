# Grafana

Grafana is an open-source platform designed for monitoring, visualization, and observability. It allows users to query, visualize, and analyze data from various sources, such as databases, cloud services, and telemetry systems, all in one unified interface.

### Resources
- [Grafana - HackTricks](https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-web/grafana.html)

### [Interesting stuff](https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-web/grafana.html#interesting-stuff)
- The file **`/etc/grafana/grafana.ini`** can contain sensitive information such as **admin** **username** and **password.**
- Inside the platform you could **invite people** or **generate API keys** (might need to be admin)
- You could check which plugins are installed (or even install new)
- By default it uses **SQLite3** database in **`/var/lib/grafana/grafana.db`**
    - `select user,password,database from data_source;`
