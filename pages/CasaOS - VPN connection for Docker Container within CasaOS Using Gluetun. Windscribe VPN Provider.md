# Goal:
	- Route a docker container within CasaOS to connect to a VPN.
- # Prerequisites
	- Install `Portainer` docker container
	- Install `Gluetun` docker container
	- As of the writing of this page, CasaOS version 0.4.15 was used. BigBearCasaOS repository is preinstalled within the App Store. If the repo is not present, the previous two containers may not be present in the App Store.
	- Docker container to be routed to the VPN. For my use case during the writing of this page, I want to connect a torrent client docker container `qBittorrent` to the VPN provider `windscribe`.
- # Setting up Gluetun to connec to Windscribe
	- This section uses the [big-bear-casaos documentation](https://community.bigbeartechworld.com/t/added-gluetun-to-big-bear-casaos/175) as reference.
	- Using the CasaOS WebUI, the following environment variables were used for the `Gluetun` container:
		- ```
		  OPENVPN_USER
		  OPENVPN_PASSWORD
		  SERVER_REGIONS
		  VPN_SERVICE_PROVIDER
		  VPN_TYPE
		  ```
	- The first two are obtained from [Windscribe's official OpenVPN Config Generator](https://windscribe.com/getconfig/openvpn). `SERVER_REGIONS` depends on the "Build a Plan" subscription. For more info: https://github.com/qdm12/gluetun-wiki/blob/main/setup/providers/windscribe.md
	- Restart the container and check the container logs (can be accessed using the CasaOS WebUI) if the VPN connection is working. Can compare the public IP of your connection to the container's VPN IP.
- # Configuring a container to connect to Gluetun's Network and hence the VPN
	- This section follows the steps outlined in this [website.](https://dbtechreviews.com/2021/02/10/run-all-your-docker-containers-through-a-gluetun-vpn-container/)
	- Open the `Portainer` WebUI and access the container.  This can be accessed on the `local` Environments.
	- Click the `Duplicate/Edit` button. Delete the lines under the Port Mapping part after taking notes of them.
	- Click ``Deploy the container ``
	- Go to the `Gluetun` container page and use `Duplicate/Edit`. Enter the noted ports from the previous container into the Port Mapping part and deploy the container.
	- Return to the previous container and click `Duplicate/Edit`. Navigate to "Advanced container settings and look for "Network". Change the "Network" option to "container" and the "Container" and "hostname" options to "gluetun"
	- Deploy the container.
	- Check the public IP of the container and compare to the VPN Public IP using the CasaOS WebUI and accessing the container's terminal. The following command can be used
		- ```
		  $ curl ifconfig.io
		  ```