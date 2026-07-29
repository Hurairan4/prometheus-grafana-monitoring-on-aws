# Troubleshooting Guide

## Overview

During the deployment of the monitoring stack, several configuration and infrastructure issues were encountered. This document summarizes the most common problems and the steps taken to resolve them.

---

# Issue 1: Unable to Access Grafana

## Symptoms

- Grafana web interface was inaccessible.
- Browser timed out while connecting to port 3000.

## Cause

AWS Security Group rules did not allow inbound traffic to the Grafana service.

## Resolution

- Added an inbound rule for TCP port **3000**.
- Verified that the Grafana service was running.
- Confirmed browser access using the EC2 public IP.

---

# Issue 2: Prometheus Target Showing DOWN

## Symptoms

Node Exporter appeared as **DOWN** in the Prometheus Targets page.

## Cause

The Node Exporter service was either not running or the scrape target was incorrectly configured.

## Resolution

- Verified that Node Exporter was running.
- Checked the target configuration in `prometheus.yml`.
- Restarted the Prometheus service.
- Confirmed the target status changed to **UP**.

---

# Issue 3: Grafana Dashboards Displayed No Data

## Symptoms

Imported dashboards displayed "No Data."

## Cause

Grafana was unable to retrieve metrics from Prometheus.

## Resolution

- Verified Prometheus was running.
- Configured Prometheus as the Grafana data source.
- Confirmed successful data source connection.
- Refreshed the dashboards.

---

# Issue 4: Alert Emails Not Received

## Symptoms

Alertmanager generated alerts, but email notifications were not delivered.

## Cause

SMTP configuration was incorrect.

## Resolution

- Generated a Gmail App Password.
- Updated Alertmanager SMTP configuration.
- Restarted Alertmanager.
- Triggered test alerts to verify email delivery.

---

# Issue 5: Services Failed to Start

## Symptoms

Prometheus or Alertmanager failed to start correctly.

## Cause

Configuration errors or incorrect service settings.

## Resolution

- Reviewed systemd service status.
- Checked service logs.
- Corrected configuration issues.
- Restarted the affected services.

---

# Issue 6: High Resource Usage

## Symptoms

The EC2 instance became slow during deployment.

## Cause

Multiple monitoring services were running simultaneously on a low-resource instance.

## Resolution

- Monitored CPU and memory utilization.
- Optimized service configuration.
- Considered upgrading the EC2 instance for improved performance.

---

# Useful Linux Commands

The following commands were frequently used during deployment and troubleshooting.

## Check Service Status

```bash
sudo systemctl status prometheus
sudo systemctl status grafana-server
sudo systemctl status alertmanager
```

## Restart Services

```bash
sudo systemctl restart prometheus
sudo systemctl restart grafana-server
sudo systemctl restart alertmanager
```

## View Logs

```bash
journalctl -u prometheus
journalctl -u grafana-server
journalctl -u alertmanager
```

## Verify Listening Ports

```bash
sudo ss -tlnp
```

---

# Lessons Learned

Troubleshooting this deployment provided practical experience with:

- AWS networking and Security Groups
- Linux service management
- Monitoring service configuration
- Infrastructure diagnostics
- Log analysis
- Alert validation
- Cloud infrastructure troubleshooting

---

# Conclusion

Resolving these issues strengthened the reliability of the monitoring stack and improved understanding of real-world DevOps troubleshooting techniques. The deployment now provides stable monitoring, visualization, and automated alerting for the AWS EC2 infrastructure.
