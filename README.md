# cross-network-monitor
A cross-network health monitoring agent running on Docker and GitLab CI/CD. Uses Bash, cURL, and jq to verify external service availability and automatically creates incident Issues via the GitLab REST API upon failure detection.
