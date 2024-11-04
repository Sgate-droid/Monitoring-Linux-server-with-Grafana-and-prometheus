# Monitoring CPU Usage with Prometheus
This guide explains how to monitor CPU usage in Prometheus, with an example query for aggregating CPU utilization across all cores. This query can help track CPU usage trends and trigger alerts in Grafana if CPU load exceeds a certain threshold.

## Prerequisites
Prometheus and Node Exporter configured to collect metrics from your instances.
Grafana configured to visualize and alert on Prometheus data.
### CPU Usage Query in Prometheus
To measure CPU usage, we’ll use the node_cpu_seconds_total metric with the following query:

promql
Copy code
100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
### Explanation
Metric Used: node_cpu_seconds_total{mode="idle"}
This metric represents the time CPU cores spend in an idle state.
Function: irate(...[5m])
Calculates the per-second rate of increase of idle time over a 5-minute window, making it more responsive to recent changes.
Aggregation: avg by (instance)
Averages the rate across all CPU cores for each instance.
Conversion: 100 - ...
Converts idle time to active CPU usage as a percentage by subtracting the idle percentage from 100%.
### Result
This query gives the average CPU usage (in %) for each instance, updated based on a 5-minute moving average.

## Adding the Query to Grafana
Open Grafana and navigate to your Dashboard.
Select Add Panel to create a new chart.
In the Metrics tab, enter the query:
promql
Copy code
100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
Customize your chart to visualize CPU usage effectively.
Set alert thresholds as needed to trigger alerts based on high CPU utilization.
## Example Usage
promql
Copy code
100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
Usage: To monitor CPU utilization in real-time across all instances.
Output: Displays active CPU usage for each instance in percentages.
