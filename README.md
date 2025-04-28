# OWASP ZAP Ansible Role

This Ansible role installs and configures OWASP ZAP (Zed Attack Proxy) as a service (ZaaS - ZAP as a Service) with both web interface and command-line access for web application security testing.

## Features

- Deploys ZAP in Docker container with persistent storage
- Provides a web interface for easy scan management (ZaaS - ZAP as a Service)
- Includes CLI tool for automated scanning
- Supports VNC/noVNC access to ZAP GUI
- Generates HTML security reports
- Records scan history

## Requirements

- Debian/Ubuntu-based system
- Python 3
- Docker and Docker Compose
- Internet access for downloading Docker images

## Role Variables

The following variables can be configured (defaults shown):

```yaml
# Default variables for OWASP ZAP installation

# ZAP API key - change this in your playbook for security
zap_api_key: "zaproxy"

# ZAP ports
zap_api_port: 8080
zap_web_ui_port: 8090
zap_vnc_port: 5901
zap_novnc_port: 6901

# ZAP version - used for Docker image
zap_version: "stable"

# Java memory allocation
zap_java_memory: "3g"

# Scanner settings
zap_timeout_secs: 600
zap_spider_duration: 60
zap_scanner_threads_per_host: 5
zap_scanner_host_per_scan: 5
```

## Dependencies

- Docker (installed automatically if not present)

## Usage

Include the role in your playbook:

```yaml
- hosts: servers
  roles:
    - role: tools/owaspzap
      vars:
        zap_api_key: "secure-api-key-here"  # It's recommended to change this
```

## Access Points

After installation, ZAP is available through multiple interfaces:

- **ZaaS Web Interface**: http://your-server:5000
- **ZAP API**: http://your-server:8080
- **ZAP GUI (via noVNC)**: http://your-server:6901
- **ZAP GUI (via VNC client)**: your-server:5901

## Command Line Usage

The `zapcli` command is installed and can be used to run scans from the command line:

```bash
# Check ZAP status
zapcli --status

# Run a full scan against a target
zapcli --url https://example.com --type full

# Run a passive scan (spidering only)
zapcli --url https://example.com --type passive

# Run an active scan only
zapcli --url https://example.com --type active
```

## Security Considerations

- Change the default API key (`zap_api_key`) to a strong, unique value
- Consider restricting network access to ZAP ports
- Review Docker container security settings
- Do not use this tool against systems you do not have permission to test

## Directories and Files

- `/opt/zaproxy`: Main ZAP installation directory
- `/opt/zaas`: ZaaS web interface directory
- `/opt/zaas/static/reports`: Scan reports directory
- `/usr/local/bin/zapcli`: CLI tool for running scans

## Service Management

```bash
# Check ZAP service status
systemctl status zaproxy

# Restart ZAP service
systemctl restart zaproxy

# View ZAP logs
journalctl -u zaproxy
```