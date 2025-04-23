# 🔐 Swarms x Phala Deployment Guide

This guide will walk you through deploying your project to Phala's Trusted Execution Environment (TEE).

## 📋 Prerequisites

- Docker installed on your system
- A DockerHub account
- Register a [Phala Cloud](https://cloud.phala.network/) account

## 🛡️ TEE Overview

For detailed instructions about Trusted Execution Environment setup, please refer to our [TEE Documentation](./tee/README.md).

## 🚀 Deployment Steps

### 1. Configure Your Environment

First, prepare your `docker-compose.yaml` file. You can find an example in [docker-compose.yaml](./docker-compose.yaml). Make sure to have your OpenAI API key ready.

```yaml
services:
  swarms-agent-server:
    image: python:3.12-slim
    volumes:
      - swarms:/app
    restart: always
    environment:
      - OPENAI_API_KEY=${OPENAI_API_KEY}
    command: # Run swarms agent example
      - /bin/sh
      - -c
      - |
        # install dependencies
        apt update && apt install -y git python3-pip
        mkdir -p /app && cd /app

        git clone --depth 1 https://github.com/The-Swarm-Corporation/swarms-examples
        cd swarms-examples/
        pip install -r requirements.txt && pip install langchain-community langchain-core
        cd examples/agents/
        python o1_preview.py

        # keep container running
        sleep infinity

volumes:
  swarms:
```

### 2. Deploy to Phala Cloud

Choose one of these deployment methods:

- Use [tee-cloud-cli](https://github.com/Phala-Network/tee-cloud-cli) (Recommended)
- Deploy manually via the [Phala Cloud Dashboard](https://cloud.phala.network/)
  1. Click `Deploy` button on the Phala Cloud dashboard.
  2. Choose `docker-compose.yaml` and then click `Advanced` tab to paste the content of your docker-compose.yaml file.
  3. Importantly, make sure to add the `OPENAI_API_KEY` in the `Encrypted Secrets` section with your own OpenAI API key.
  4. Click `Create` button to create a new Swarms agent application.
      <p align="center">
      <img src="./imgs/01_create_agent_on_phala_cloud.png" alt="Creating a Swarms agent on Phala Cloud" style="width: 700px;">
      </p>

### 3. Monitor Your Deployment

1. Check the initialization logs of your agent
   <p align="center">
   <img src="./imgs/02_serial_logs.png" alt="Agent initialization logs" style="width: 700px;">
   <img src="./imgs/03_serial_logs.png" alt="Detailed initialization logs" style="width: 700px;">
   </p>

2. Verify your container is running
   <p align="center">
   <img src="./imgs/04_swarms_agent_containers.png" alt="Swarms Agent Container Status" style="width: 700px;">
   </p>

3. Monitor your agent's output
   <p align="center">
   <img src="./imgs/05_agent_output.png" alt="Swarms Agent Logs" style="width: 700px;">
   </p>

### 4. Verify TEE Attestation

Visit the [TEE Attestation Explorer](https://proof.t16z.com/) to check and verify your agent's TEE proof.

<p align="center">
<img src="./imgs/06_attestation.png" alt="TEE Attestation Verification" style="width: 700px;">
</p>

## 📚 Additional Resources

For more comprehensive documentation and examples, visit our [Official Documentation](https://docs.swarms.world/en/latest/).
