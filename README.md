# Dozzle Docker Setup

This repository contains the Docker Compose configuration for running Dozzle, a real-time log viewer for Docker containers.

## Overview

Dozzle is a simple, lightweight application that helps you monitor your Docker container logs in real-time through a web interface. This setup includes configuration for connecting to a remote Docker agent.

## Prerequisites

- Docker Engine installed
- Docker Compose V2+ installed
- Access to Docker socket
- Network access to remote Docker agent (if using remote configuration)

## Configuration

### Docker Compose

The setup uses Docker Compose with version 3 of the compose specification. Here's the configuration breakdown:

```yaml
version: "3"
networks:
  unklab:
    external: true
services:
  dozzle:
    image: amir20/dozzle:latest
    container_name: dozzle
    restart: unless-stopped
    environment:
      - DOZZLE_REMOTE_AGENT=10.104.0.3:7007
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - unklab
```

### Key Components

1. **Networks**:
   - Uses an external network named `unklab`
   - External networks must be created before running this compose file

2. **Service Configuration**:
   - Container name: `dozzle`
   - Image: `amir20/dozzle:latest`
   - Restart policy: `unless-stopped`
   
3. **Environment Variables**:
   - `DOZZLE_REMOTE_AGENT`: Set to `10.104.0.3:7007` for remote agent connection

4. **Volumes**:
   - Mounts Docker socket for container log access
   - Mount path: `/var/run/docker.sock:/var/run/docker.sock`

## Installation

1. Clone this repository:
   ```bash
   git clone [repository-url]
   cd [repository-name]
   ```

2. Create the required external network (if not already existing):
   ```bash
   docker network create unklab
   ```

3. Start the container:
   ```bash
   docker compose up -d
   ```

## Usage

Once the container is running, you can access the Dozzle web interface through your browser. The default port is typically 8080, but you may need to check your specific configuration.

### Monitoring Logs

1. Open your web browser
2. Navigate to the Dozzle interface
3. View real-time logs for all your Docker containers

## Maintenance

### Updating

To update to the latest version of Dozzle:

```bash
docker compose pull
docker compose up -d
```

### Stopping the Service

To stop Dozzle:

```bash
docker compose down
```

## Troubleshooting

Common issues and solutions:

1. **Docker Socket Access Issues**:
   - Ensure proper permissions on `/var/run/docker.sock`
   - Check if the user has proper group membership

2. **Network Connectivity**:
   - Verify the remote agent address is correct
   - Check if the port 7007 is accessible
   - Ensure the `unklab` network exists and is properly configured

3. **Container Won't Start**:
   - Check logs using `docker compose logs dozzle`
   - Verify all required networks are available
   - Ensure Docker socket is accessible

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## Support

For issues and feature requests, please file an issue in the GitHub repository.
