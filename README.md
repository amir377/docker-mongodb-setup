
# Docker MongoDB Setup

This repository provides an easy-to-use solution for deploying a MongoDB database inside a Docker container. It includes user-friendly PowerShell and Bash scripts to automate the process of setting up a MongoDB container with customizable parameters.

## Features

1. **Environment File Generation**: Automatically generates a `.env` file with user-defined parameters such as container name, network name, port, and credentials.
2. **Docker Network Management**: Creates a Docker network if it doesn’t already exist.
3. **Docker Compose Configuration**: Dynamically generates a `docker-compose.yaml` file from a `docker-compose.example.yaml` template.
4. **Build and Run**: Executes `docker-compose up -d --build` to build and start the MongoDB container.
5. **Container Status Check**: Checks the container status post-deployment.
6. **Error Handling**: Automatically displays logs if the container fails to start.

## Prerequisites

- Docker must be installed and running on your system.
- Docker Compose must be installed.
- **Linux Only**: Ensure `dos2unix` is installed to handle line ending conversions if needed.
  ```bash
  sudo apt install dos2unix
  ```
- Git must be installed and available in your system's PATH.
  ```bash
  sudo apt install git       # For Debian/Ubuntu
  sudo yum install git       # For CentOS/RHEL
  brew install git           # For macOS
  ```
- Windows PowerShell (Admin) or a Bash-compatible terminal for script execution.

## Installation Steps

### One Command Install and Clone

#### Windows

Open PowerShell as Administrator and run:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass; git clone https://github.com/amir377/docker-mongodb-setup; cd docker-mongodb-setup; ./install.ps1
```

#### Linux/Mac

Run the following command in your terminal:
```bash
git clone https://github.com/amir377/docker-mongodb-setup && cd docker-mongodb-setup && dos2unix install.sh && chmod +x install.sh && ./install.sh
```

## Manual Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/amir377/docker-mongodb-setup.git
   cd docker-mongodb-setup
   ```

2. Place your `docker-compose.example.yaml` file in the root directory (if not already provided).

3. Run the appropriate installation script:
    - For Windows: `install.ps1`
    - For Linux/Mac: `install.sh`

4. Follow the prompts to customize your setup:
    - Container name
    - Network name
    - MongoDB port
    - MongoDB username and password

## File Descriptions

### `install.ps1`

Automates the entire setup process for Windows, including:

- Generating `.env` and `docker-compose.yaml` files.
- Ensuring Docker and Docker Compose are installed.
- Creating the specified Docker network.
- Building and starting the MongoDB container.
- Providing access to container logs if needed.

### `install.sh`

Automates the setup process for Linux/Mac, including:

- Generating `.env` and `docker-compose.yaml` files.
- Ensuring Docker and Docker Compose are installed.
- Creating the specified Docker network.
- Building and starting the MongoDB container.

### `.env`

Holds configuration values for the MongoDB container. Example:

```env
# MONGODB container settings
CONTAINER_NAME=mongodb
NETWORK_NAME=general
MONGODB_PORT=27017
ALLOW_HOST=0.0.0.0

# MONGODB credentials
MONGODB_USER=admin
MONGODB_PASSWORD=admin123
```

### `docker-compose.example.yaml`

A template file for the `docker-compose.yaml` file. Placeholders are dynamically replaced with values from the `.env` file. Example:

```yaml
services:
  mongodb:
    image: mongo:latest
    container_name: ${CONTAINER_NAME}
    environment:
      MONGO_INITDB_ROOT_USERNAME: ${MONGODB_USER}
      MONGO_INITDB_ROOT_PASSWORD: ${MONGODB_PASSWORD}
    volumes:
      - mongodb_data:/data/db
    ports:
      - "${ALLOW_HOST}:${MONGODB_PORT}:27017"
    networks:
      - ${NETWORK_NAME}

volumes:
  mongodb_data:
    driver: local

networks:
  ${NETWORK_NAME}:
    external: true
```

### `docker-compose.yaml`

Generated file used to deploy the MongoDB container with Docker Compose.

## Usage

### Accessing MongoDB

- Host: The `ALLOW_HOST` value specified during setup (default: `0.0.0.0`)
- Port: The `MONGODB_PORT` value specified during setup (default: `27017`)
- Username: The `MONGODB_USER` value specified during setup
- Password: The `MONGODB_PASSWORD` value specified during setup

### Viewing Logs

If the container fails to start, the script will display logs automatically:

```bash
docker logs <container-name>
```

### Accessing the Container Shell

If you need to access the MongoDB container shell:

```bash
docker exec -it <container-name> bash
```

## Notes

- Ensure Docker is running before executing the script.
- If you encounter permission issues, run PowerShell or Bash as Administrator.
- Update the `docker-compose.example.yaml` file to suit your specific requirements.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

---

Feel free to customize the script or template files to fit your specific use case!
