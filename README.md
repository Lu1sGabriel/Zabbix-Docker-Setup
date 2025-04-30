# Zabbix Docker Compose Setup

This repository contains the configuration for setting up Zabbix using Docker Compose, simplifying the process of creating and managing containers for Zabbix components.

## Components

This setup utilizes the following Docker services:

- **PostgreSQL**: The database used to store Zabbix data.
- **Zabbix Server**: The main server that collects and processes monitoring data.
- **Zabbix Web**: The web interface to access and manage Zabbix.
- **Zabbix Agent**: The agent responsible for monitoring hosts.

## Prerequisites

- Docker
- Docker Compose

## Usage

### 1. Clone the Repository
Clone this repository to your local machine:
```bash
git clone https://github.com/Lu1sGabriel/Zabbix-Docker-Setup.git
```

### 2. Create a Docker Network
Before starting the containers, create a custom network named `zabbix` (or any name you prefer). Run the following command to create the network:
```bash
docker network create "your-network-name"
```
If you choose a different network name, update the `NETWORK` variable in the `.env` file with the name you selected. Example:
```bash
NETWORK="your-created-network-name"
```

### 3. Start the Containers
Start the containers using Docker Compose:
```bash
docker-compose up -d
```

### 4. Find the Zabbix Agent IP
After the containers are up, you need to find the IP address assigned to the `zabbix-agent` container within the `zabbix` network. Use the following command to inspect the Docker network:
```bash
docker network inspect zabbix
```
Look for the section related to the `zabbix-agent` container and find the `IPv4Address` field. Example:
```json
"Containers": {
    "88ad0d25edeb460385b145517648736992c0fed3ed9835f7e7e91fa6060c4594": {
                "Name": "zabbix-agent",
                "EndpointID": "b2576cac3af80e5aadad7d6bedea073997e98540ff010661ac7e0ec2a878a681",
                "MacAddress": "4e:35:01:e3:91:fb",
                "IPv4Address": "172.18.0.5/16",
                "IPv6Address": ""
    }
}
```
In the example above, the IP address of the `zabbix-agent` would be `172.18.0.5`.

Aqui está a versão atualizada com o nome correto do modal:

---

### 5. Add the Zabbix Agent IP in the Web Interface

After starting the containers, follow these steps to update the Zabbix Agent IP:

1. **Go to Data Collection**: Access the Zabbix web interface at [http://localhost](http://localhost), and navigate to the **Data Collection** section.
2. **Navigate to Hosts**: In the **Host** section, find the entry named **Zabbix server** and click on it.
3. **Edit Interfaces**: This will open the **Host** modal. In the **Interfaces** section, enter the IPv4 address of the `zabbix-agent` (for example, `172.18.0.5`) in the appropriate field.
4. **Update**: Click on the **Update** button to save the changes.
5. **Wait**: After updating, wait for about two minutes to allow the changes to take effect.

Once the waiting time has passed, refresh or navigate to another page to see the updated configuration.

### 6. Access the Zabbix Web Interface
Once the previous steps are completed, you can access the Zabbix web interface at:
[http://localhost](http://localhost)

## Volumes

This setup uses persistent volumes to ensure that Zabbix and PostgreSQL data are not lost when the containers are removed:

- `zabbix_postgres_data`: Stores PostgreSQL data.
- `zabbix_server_data`: Stores Zabbix server data.
- `zabbix_web_data`: Stores Zabbix web interface data.
