To install Prometheus on a Linux server, you can follow these steps:

1. **Download Prometheus**:

    Visit the Prometheus download page (https://prometheus.io/download/) and get the link for the latest version of Prometheus suitable for your operating system. You can use tools like `wget` to download it directly onto your server. For example:
	```bash
	wget https://github.com/prometheus/prometheus/releases/download/v2.34.0/prometheus-2.34.0.linux-amd64.tar.gz
	```

2. **Extract Prometheus**:

    Extract the downloaded tarball using the following command:
	```bash
	tar -xvf prometheus-*.linux-amd64.tar.gz
	```

3. **Navigate to Prometheus Directory**:

    Move into the extracted directory:
	```bash
	cd prometheus-*
	```

4. **Run Prometheus**:

    Prometheus can be started directly without any installation. You can run it in the foreground for testing purposes:
	```bash
	./prometheus
	```

    By default, Prometheus will start on port 9090.

5. **Access Prometheus Web UI**:

    Open a web browser and navigate to `http://your_server_ip:9090` to access the Prometheus web interface.

7. **Configure Prometheus (Optional)**:

    You can customize Prometheus configuration according to your requirements by editing the `prometheus.yml` file located in the Prometheus directory. This file defines the targets (such as servers and services) to monitor and scrape metrics from.

8. **Run Prometheus as a Service (Optional)**:

    To run Prometheus as a service that starts automatically on system boot, you can create a systemd service unit file. Here's an example:

    Create a file named `prometheus.service` in `/etc/systemd/system/` directory:
	```bash
	sudo vim /etc/systemd/system/prometheus.service
	```
	Add the following content to the file:

	```makefile
	[Unit]
	Description=Prometheus Monitoring
	After=network.target

	[Service]
	ExecStart=/path/to/prometheus/prometheus
	Restart=always

	[Install]
	WantedBy=multi-user.target
	```

	Replace `/path/to/prometheus/prometheus` with the actual path to your Prometheus binary.

	Save the file and exit the text editor.

	Reload systemd and start the Prometheus service:

	```bash
	sudo systemctl daemon-reload
	sudo systemctl start prometheus
	sudo systemctl enable prometheus
	```

    Now, Prometheus will start automatically on system boot.

	That's it! You've successfully installed Prometheus on your Linux server. You can now configure it to monitor your infrastructure and applications.

---
### Configuration

```yaml
global:
  scrape_interval: 15s  # How frequently to scrape targets by default
  evaluation_interval: 15s  # How frequently to evaluate rules
  retention: 30d  # 30 days retention period

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']  # The Prometheus server itself

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['node1:9100', 'node2:9100']  # Targets running node_exporter

  # Add more scrape configurations for other exporters or services as needed

```
