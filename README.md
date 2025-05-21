# Eclipse ECSP Docker Base Images

## Table of Contents

1. [Introduction](#introduction)
2. [Image Details](#image-details)
3. [Configuration](#configuration)
4. [How to Build](#how-to-build)
5. [How to Push](#how-to-push)
6. [Open Items](#open-items)
7. [Announcements](#Announcements)

## Introduction

This module maintains all the base images for the Eclipse ECSP Platform.

## Image Details

### ecsp-base-java17

- **Base Image:** `azul/zulu-openjdk-alpine:17.0.9-17.46.19-jre-headless`
- **OS:** Alpine Linux
- **Purpose:** Minimal Java 17 runtime for ECSP applications.

### ecsp-base-java17-tomcat10-apr

- **Base Image:** `azul/zulu-openjdk-alpine:17.0.9-17.46.19`
- **OS:** Alpine Linux
- **Java:** OpenJDK 17
- **Tomcat:** 10.1.40 (installed from official Apache archives)
- **APR:** Tomcat Native Library (APR) compiled and included for enhanced performance.
- **Purpose:** Java 17 runtime with Tomcat 10 and APR for ECSP web applications.

## Configuration

Update the `.env` file as per release requirements:

```
build_repo=docker.io/eclipseecsp
build_version=1.0.0-1
```

## How to Build

Follow these steps to build the images:

1. **Clone the repository** (if not already done):

   ```
   git clone <repository-url>
   cd docker-base-image
   ```

2. **Update the `.env` file** with the correct `build_repo` and `build_version` values.

3. **Build the images** using Docker Compose:
   go to the directory where the `docker-compose.yml` file is located and run:
   ```shell
    cd java17
   ```
   build images
   ```shell
   docker compose build
   ```

   - This will build all images defined in the `docker-compose.yml` file.
   - To build a specific image, use:
     ```shell
     docker compose build <service-name>
     ```
     Replace `<service-name>` with the desired image (e.g., `ecsp-base-java17-tomcat10-apr`).

4. **Verify the built images**:

   ```shell
   docker images | grep <service-name>
   ```

## How to Push

Push the images to your configured repository:

1. **Login to Docker registry** (if required):
   generate Personal Access token(PAT) on https://app.docker.com/settings
   and use it as password
   ```shell
   docker login <your-registry-url>
   ```
    - For Docker Hub, use:
      ```shell
      docker login
      ```
2. **Push the images** using Docker Compose:

   ```shell
   docker compose push
   ```

   - To push a specific image:
     ```shell
     docker compose push <service-name>
     ```

3. **Verify the images in your registry** (e.g., Docker Hub or your private registry).
    - You can check the pushed images using:
      ```shell
      docker search eclipseecsp
      ```
    - Or private registry
      ```shell
      docker search <your-registry-url>
      ```
    - Or by visiting your Docker Hub account or private registry.

## Open Items

- NA

## Announcements

All updates to this library are documented in [releases](../../releases)
For the available versions, see the [tags on this repository](../../tags).
