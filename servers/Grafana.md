 1. **Update Packages**: It's always a good idea to update your package lists and upgrade your installed packages to their latest versions before starting the installation process.
	```bash
	sudo apt update sudo apt upgrade
	```

1. **Install Dependencies**: Grafana has a few dependencies that need to be installed. These include `apt-transport-https` and `software-properties-common`.
	```bash
	sudo apt install -y apt-transport-https software-properties-common
	```

3. **Add Grafana Repository**: Add the Grafana repository to your system.
	```bash
	sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
	```

- NOTE: May be possible to add architecture to fix a problem with packages (depends on linux distro you use)

	```
	# add [arch=amd64]
	# deb-src http://security.ubuntu.com/ubuntu focal-security multiverse
	deb [arch=amd64] https://packages.grafana.com/oss/deb stable main
	```

4. **Import GPG Key**: Import the GPG key used to sign Grafana packages.
	```bash
	wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
	```

5. **Update Package Lists**: Update your package lists again to include the Grafana repository.
	```bash
	sudo apt update
	```

6. **Install Grafana**: Now, you can install Grafana using apt.
	```bash
	sudo apt install grafana
	``` 

7. **Start Grafana Service**: Once Grafana is installed, you can start the Grafana server.
	```bash
	sudo systemctl start grafana-server
	```

8. **Enable Grafana Service**: If you want Grafana to start automatically when your system boots up, you can enable the service.
	```bash
	sudo systemctl enable grafana-server
	```

9. **Access Grafana**: Grafana should now be running on your Debian system. You can access it by navigating to `http://your_server_ip:3000` in your web browser. The default username and password are both `admin`. Upon logging in for the first time, Grafana will prompt you to change the password.
    
		First login
		
	> 	user: admin

	> 	password: admin
		
		After login password must be changed to something else


That's it! You should now have Grafana installed and running on your Debian system. You can start configuring data sources, dashboards, and exploring its features.