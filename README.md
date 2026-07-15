# cross-network-monitor
A cross-network health monitoring agent running on Docker and GitLab CI/CD. Uses Bash, cURL, and jq to verify external service availability and automatically creates incident Issues via the GitLab REST API upon failure detection.

# Cross-Network Monitor & Auto-Reporter

![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=flat-square&logo=docker&logoColor=white)
![GitLab CI](https://img.shields.io/badge/gitlab%20ci-%23181717.svg?style=flat-square&logo=gitlab&logoColor=white)
![Bash](https://img.shields.io/badge/bash-%23121011.svg?style=flat-square&logo=gnu-bash&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)

## Overview
This repository contains a lightweight, containerized DevOps automation tool designed to cross network boundaries. It acts as a scheduled health monitor that pings external services (e.g., in "Network B") from a GitLab CI/CD pipeline (in "Network A"). If the target service is unreachable or returns an error, the agent automatically communicates with the GitLab REST API to create an Incident Issue.

## Architecture & Workflow
1. **Scheduled Execution:** GitLab CI/CD runs the pipeline on a defined cron schedule (e.g., hourly).
2. **Containerization:** A lightweight Alpine Docker container is spun up.
3. **Health Check:** A Bash script uses `curl` to fetch the HTTP status code of the target endpoint.
4. **Logic & Parsing:** If the status code is `200 OK`, the pipeline succeeds silently. If it fails (e.g., `500`, `404`, or timeout), the script proceeds to the reporting phase.
5. **Auto-Reporting:** The script builds a JSON payload using `jq` and sends a POST request to the GitLab API to automatically open an issue containing the exact failure details and timestamp.

## Tech Stack
* **Orchestration:** GitLab CI/CD (Scheduled Pipelines)
* **Container Environment:** Docker (Alpine Linux)
* **Scripting:** Bash
* **Networking & API:** cURL
* **Data Processing:** jq

## Directory Structure
```text
.
├── .gitlab-ci.yml         # CI/CD pipeline definition
├── Dockerfile             # Alpine-based agent image
├── README.md              # Project documentation
├── LICENSE                # MIT License
└── src/
    └── health_monitor.sh  # Core logic script
