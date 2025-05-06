# Grafana - Dashbaords


## metrics types in prometheus

**counter** : A counter is a monotonic metric that only increases over time. e.g., total number of HTTP requests or errors.

**gauge** :  A gauge is a metric that represents a single numerical value that can go up and down over time. Ideal for measuring values that fluctuate. e.g., memory usage.

**histogram** : A histogram samples observations (e.g., request durations) and buckets them into configurable ranges. It allows for aggregating data into specific ranges, like "how many requests were served in less than 1 second, between 1 and 5 seconds, etc."

**summary** : A summary is similar to a histogram, but it provides quantile values (e.g., 95th percentile, 99th percentile). It tracks the sum of observations and the count of events, along with configurable quantiles. e.g., request durations


## Grafana Drilldown (previously explore)

- Grafana Explore is an interactive interface within Grafana that allows you to query and explore your metrics, logs, and traces in real-time.
- It provides a simplified view for troubleshooting, ad-hoc querying, and visualizing time-series data without creating full dashboards.
- It’s especially useful when you need to quickly investigate a metric, correlate logs, or perform troubleshooting in a low-effort environment.

### Queries


#### CPU usage

-  Refers to the usage of CPU cores or processing power by the system's active tasks.

**Query:**

```
avg without(cpu) (irate(node_cpu_seconds_total{job="node_exporter", instance="localhost:9100", mode!="idle"}[1m]))
```

#### Memory

- Refers to the system's RAM (Random Access Memory) used by running processes.

**Query:**


`Used`: tmemory actively used by running processes, excluding memory that the operating system can reclaim (such as cache and buffers).

```
node_memory_MemTotal_bytes{job="node_exporter", instance="localhost:9100"} - node_memory_MemFree_bytes{job="node_exporter", instance="localhost:9100"} - node_memory_Cached_bytes{job="node_exporter", instance="localhost:9100"} - node_memory_Buffers_bytes{instance="localhost:9100", job="node_exporter"}
```

`Cached`: the memory used by the kernel for file system caching (in bytes).

```
node_memory_Cached_bytes{job="node_exporter", instance="localhost:9100"}
```

`Buffer`: the memory used by the kernel for buffering I/O operations (in bytes).

```
node_memory_Buffers_bytes{instance="localhost:9100", job="node_exporter"}
```

`Free`:  unused or free memory (in bytes).

```
node_memory_MemFree_bytes{job="node_exporter", instance="localhost:9100"}
```

#### Disk usage

- Referes to the amount of storage space used on the system's disk. 

**Query:** 

`Used Disk Space`

```
node_filesystem_size_bytes{job="node_exporter", instance="localhost:9100", device!="tmpfs"} - node_filesystem_avail_bytes{job="node_exporter", instance="localhost:9100", device!="tmpfs"}

```

`Available Disk Space`:

```
node_filesystem_avail_bytes{instance="localhost:9100", job="node_exporter", device!~"tmpfs"}
```


#### Networking

- The volume of data being transmitted over the network interface, including incoming and outgoing traffic.

**Query:**

`Received`:

```
irate(node_network_receive_bytes_total{job="node_exporter", instance="localhost:9100"}[5m])
```

`Transmitted`:
```
irate(node_network_transmit_bytes_total{job="node_exporter", instance="localhost:9100"}[5m])
```


### Alerts

Grafana Alerts allow you to monitor metrics and get notified when specific conditions are met — like high CPU usage or low disk space.


**Core Components:**

- Query: PromQL (or other datasource query) that returns a value to evaluate.
- Condition: Defines when the alert should trigger (e.g., WHEN avg() IS ABOVE 80).
- Evaluator: Compares the query result against the defined threshold.
- For Duration: Optional: specifies how long the condition must be true before triggering the alert (e.g., 5 minutes).

**Contact Points (Notification Channels):**

Alert notifications can be sent to various destinations, including:

- Email (via SMTP configuration)
- Slack
- Teams
- PagerDuty
- Webhook
- Discord
- Opsgenie
- Telegram
- Grafana OnCall (for scheduling and escalation)

Query:

```
100 * (1 - avg(rate(node_cpu_seconds_total{mode="idle", instance="localhost:9100"})))
```


### Administration

#### Plugins

- Plugins in Grafana are extensions that enhance Grafana’s functionality. They can be used to add new data sources, panels, dashboards, or even entire applications to Grafana. Plugins can be installed from the Grafana plugin repository or manually uploaded.

**Examples**

- Data Source Plugins: Prometheus, Elasticsearch, MySQL, etc.

- Panel Plugins: Heatmap, Graph, Table, etc.

- App Plugins: Custom applications that extend Grafana’s functionality.

[grafana plugins](https://grafana.com/grafana/plugins/)


#### User and Access

Users in Grafana are individuals who have access to the Grafana dashboard. They can have different roles, and each role grants specific permissions for viewing and interacting with the data, dashboards, and settings.

**Roles:**

- Viewer: A user with this role can only view dashboards and other resources. They do not have permission to make any changes.
- Editor: A user with this role can view and edit dashboards, panels, and other resources. However, editors do not have permission to manage users, data sources, or other administrative settings.
- Admin: A user with this role has full access to Grafana. They can create, edit, and delete dashboards, data sources, and other resources, as well as manage users and settings.
- No basic role: No predefined role assigned, can be used for special cases.


- Teams group users together, making it easier to manage access and permissions. Teams can be assigned roles, and all members of a team inherit the team's permissions for accessing resources.

##### Authentication

- Authentication in Grafana ensures secure user access through various methods, enabling organizations to control who can view and manage dashboards.
-  Supports basic auth, OAuth, LDAP, SAML, and external providers (e.g., Google, GitHub).

##### Service Account

A service account is a non-human account used for automating tasks and interacting with the Grafana API. It allows applications and systems to authenticate and perform actions without manual user login.

**Key Points:**

- Automated Tasks: Used for dashboard provisioning, configuration, report generation, and interacting with the Grafana API.
- Not a User: Service accounts are for automation, unlike user accounts linked to individual users.
- Replaces API Keys: Grafana recommends using service accounts over API keys for better management and security.
- Role-based Permissions: In Grafana Enterprise, service accounts can be combined with role-based access control (RBAC) for fine-grained permission management.

**Common Use Cases:**

- Automate dashboard creation and updates.
- Schedule and send reports.
- Manage alerts and integrate with external authentication systems.
- Authenticate applications like Terraform without logging in as a user.


**reference:** [grafana latest docs](https://grafana.com/docs/grafana/latest/)

