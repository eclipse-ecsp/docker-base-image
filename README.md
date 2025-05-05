# Eclipse ECSP Docker Base Images

## Table of Contents

1. [Introduction](#introduction)
1. [Configuration](#configuration)
2. [How to Build](#how-to-build)
3. [How to Push](#how-to-push)
4. [Open Items](#open-items)
5. [Release Notes](release-notes.md)

## Introduction

This module is to maintain all the base images for Eclipse ECSP Platform.

### ecsp-base-java17

Image with

- Alpine OS
- azul/zulu-openjdk-alpine:17.0.9-17.46.19-jre-headless

### ecsp-api-base-java17-tomcat10-apr

Image with

- Alpine OS
- azul/zulu-openjdk-alpine:17.0.9-17.46.19-jre-headless
- tomcat 10.1.40 (with APR library)

## Configuration

Update .env file as per release.

```
build_repo=docker.io/eclipseecsp
build_version=1.0.0-1
```

## How to Build

```
docker compose build
```

## How to Push

```
docker compose push
```

## Open Items

- NA
