To install Node Exporter on your server, you can follow these general steps. Node Exporter is a Prometheus exporter for hardware and OS metrics exposed by *NIX kernels. Here's how you can install it:

1. **Download Node Exporter**:
   You can download the Node Exporter binary from the official Prometheus website or GitHub repository. Here's an example of how to download it using `curl`:

   ```bash
   curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
   ```

   Replace `v1.2.2` with the version you want to download.

2. **Extract the Tarball**:
   Once the download is complete, extract the tarball:

   ```bash
   tar xvfz node_exporter-1.2.2.linux-amd64.tar.gz
   ```

3. **Move Node Exporter Binary**:
   Move the Node Exporter binary to a directory in your PATH, for example `/usr/local/bin/`:

   ```bash
   sudo mv node_exporter-1.2.2.linux-amd64/node_exporter /usr/local/bin/
   ```

4. **Create a Systemd Service**:
   Create a systemd service file to manage the Node Exporter service. You can create a file named `node_exporter.service` in `/etc/systemd/system/`:

   ```plaintext
   [Unit]
   Description=Node Exporter
   After=network.target

   [Service]
   User=node_exporter
   Group=node_exporter
   Type=simple
   ExecStart=/usr/local/bin/node_exporter

   [Install]
   WantedBy=multi-user.target
   ```

5. **Create a Node Exporter User**:
   It's a good practice to run Node Exporter as a non-root user for security reasons. Create a dedicated user for Node Exporter:

   ```bash
   sudo useradd -rs /bin/false node_exporter
   ```

6. **Reload Systemd and Start Node Exporter**:
   Reload systemd to load the new service file and start Node Exporter:

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start node_exporter
   ```

7. **Enable Node Exporter to Start on Boot**:
   If you want Node Exporter to start automatically when the system boots, enable the service:

   ```bash
   sudo systemctl enable node_exporter
   ```

8. **Verify Node Exporter Status**:
   Check the status of Node Exporter to ensure that it's running without any errors:

   ```bash
   sudo systemctl status node_exporter
   ```

   If everything is configured correctly, you should see that Node Exporter is active and running.

9. **Configure Prometheus to Scrape Node Exporter**:
   Finally, you need to configure Prometheus to scrape metrics from Node Exporter. Add Node Exporter as a target in your Prometheus configuration file (`prometheus.yml`).

That's it! Node Exporter should now be installed and running on your server, exposing hardware and OS metrics for Prometheus to scrape.

---

# Configure node exporter config

To configure Prometheus to scrape metrics from Node Exporter, you need to add Node Exporter as a target in your Prometheus configuration file (`prometheus.yml`). Here's how you can do it:

1. **Open `prometheus.yml`**: Use a text editor to open the Prometheus configuration file (`prometheus.yml`). This file is typically located in the Prometheus installation directory, such as `/etc/prometheus/prometheus.yml`.

2. **Add Node Exporter as a Target**:
   Add a new scrape configuration under the `scrape_configs` section of the `prometheus.yml` file. Here's an example configuration:

   ```yaml
   scrape_configs:
     - job_name: 'node_exporter'
       static_configs:
         - targets: ['localhost:9100']
   ```

   In this configuration:
   - `job_name`: This is an arbitrary name for the job. You can use any descriptive name.
   - `targets`: This specifies the address and port where Node Exporter is running. If Node Exporter is running on the same server as Prometheus, you can use `localhost:9100`. If Node Exporter is running on a different server, replace `localhost` with the IP address or hostname of the server where Node Exporter is running.

3. **Save and Close the File**: Once you've added the scrape configuration for Node Exporter, save the `prometheus.yml` file and exit the text editor.

4. **Reload Prometheus Configuration**:
   After making changes to the Prometheus configuration file, you need to reload Prometheus to apply the changes. You can do this by sending a SIGHUP signal to the Prometheus process:

   ```bash
   sudo killall -HUP prometheus
   ```

   Alternatively, if Prometheus is managed by systemd, you can use the following command:

   ```bash
   sudo systemctl reload prometheus
   ```

5. **Verify Configuration**:
   After reloading Prometheus, verify that the Node Exporter target has been successfully added. You can check the Prometheus targets page (`http://prometheus-server:9090/targets`) to see if Node Exporter is listed as a target and whether it's being scraped successfully.

6. **Check Metrics**:
   Finally, navigate to the Prometheus web interface (`http://prometheus-server:9090`) and use the expression browser to query Node Exporter metrics. You should see a list of metrics exposed by Node Exporter, such as CPU usage, memory usage, disk I/O, etc.

That's it! You've successfully configured Prometheus to scrape metrics from Node Exporter. You can now use these metrics to monitor the health and performance of your servers.