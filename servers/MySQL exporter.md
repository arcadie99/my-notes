To install `mysqld_exporter` for exporting MySQL metrics to Prometheus, you can follow these general steps:

### 1. Download `mysqld_exporter` Binary
You can download the latest version of `mysqld_exporter` from the official GitHub repository releases page: [Prometheus Mysqld Exporter Releases](https://github.com/prometheus/mysqld_exporter/releases).

### 2. Extract the Binary
Once downloaded, extract the `mysqld_exporter` binary from the archive.

### 3. Move the Binary to a Permanent Location
Move the `mysqld_exporter` binary to a permanent location on your system. It's a good practice to place it in a directory included in your system's `$PATH`.

For example, you might move it to `/usr/local/bin`:

```bash
sudo mv mysqld_exporter /usr/local/bin
```

### 4. Create a Configuration File (Optional)
You can create a configuration file for `mysqld_exporter` to customize its behavior. A sample configuration file is usually provided with the `mysqld_exporter` binary.

```bash
vim /etc/mysqld_exporter.my-cnf
```

```cnf
[client]
user = exporter
password = password
```

In case MySQL is connected throw socket you have to use parameter `socket`:
```
socket=/var/run/mysql/mysql.sock
```

### 5. Configure MySQL Access
Ensure that `mysqld_exporter` has the necessary permissions to access MySQL. You may need to create a dedicated MySQL user with appropriate privileges for scraping metrics.
```sql
CREATE USER 'exporter'@'localhost' IDENTIFIED BY 'XXXXXXXX' WITH MAX_USER_CONNECTIONS 3;
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';
```

### 6. Start `mysqld_exporter`
You can start `mysqld_exporter` by running the binary directly:

```bash
nohup mysqld_exporter &
```

Or, if you prefer, you can create a systemd service file to manage `mysqld_exporter` as a service. Here's an example systemd service file (`mysqld_exporter.service`):

```ini
[Unit]
Description=MySQL Exporter
After=network.target

[Service]
User=mysqld_exporter
Group=mysqld_exporter
Type=simple
ExecStart=mysqld_exporter --config.my-cnf=/etc/mysqld_exporter.my-cnf

[Install]
WantedBy=multi-user.target
```

Place this file in `/etc/systemd/system/` directory, then start and enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl start mysqld_exporter
sudo systemctl enable mysqld_exporter
```

### 7. Verify `mysqld_exporter` Status
Check the status of `mysqld_exporter` to ensure it's running without any issues:

```bash
systemctl status mysqld_exporter
```

### 8. Configure Prometheus
Update your Prometheus configuration (`prometheus.yml`) to scrape metrics from `mysqld_exporter`. Add a new scrape configuration targeting the address and port where `mysqld_exporter` is running.

### 9. Import Grafana Dashboards (Optional)
You can import pre-made Grafana dashboards for MySQL metrics from the Grafana dashboard repository or create custom dashboards tailored to your specific needs.