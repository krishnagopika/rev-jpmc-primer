# Splunk


Splunk is a powerful platform used for searching, monitoring, and analyzing machine-generated big data via a web-style interface. It is widely used in SIEM (Security Information and Event Management) and log management.

**Key Features of Splunk**

- Real-time data processing.
- Scalable architecture.
- Alerts, dashboards, and custom visualizations.
- Integration with third-party tools (via apps and APIs).
- Role-based access control and audit trails for security compliance.


### The core components

- **Forwarder:** Collects logs and forwards raw data to the indexer.
  - Universal Forwarder (UF): Lightweight agent, only forwards data.
  - Heavy Forwarder (HF): Parses and filters data before forwarding.
  - Installed on the source machine (where logs are generated).
- **Indexer:** Receives, parses, and indexes incoming data.
  - Processes logs into events.
  - Stores the indexed data for search and analysis.
- **Search Head:** Provides a user interface to search and analyze indexed data.
  - Uses SPL (Search Processing Language) to perform queries.
  - Visualizes data through dashboards, alerts, and reports.



### Deployment Types


**Standalone:** 

- All roles (Forwarder, Indexer, Search Head) run on a single Splunk instance. 
- Used for small-scale testing or personal learning environments.

**Basic:** 

- Forwarders installed on source machines collect and send data. 
- A single Splunk instance acts as both indexer and search head. Suitable for small to medium setups.

**Distributed (Multi-Instance) Deployment:** 

- Components are separated across different instances:
  - Forwarders send data.
  - Indexers store and index the data.
  - Search Heads query and analyze data.
- Scalable and suitable for larger environments.

**Clustering:**

- Enables high availability and fault tolerance.
- **Types:**
  - Indexer Clustering: Replicates indexed data across multiple indexers for redundancy.
  - Search Head Clustering: Enables load balancing and configuration synchronization across multiple search heads.
- Ideal for enterprise-scale deployments.


**splunk enterprise setup**


- [splunk enterprise download](https://www.splunk.com/en_us/download/splunk-enterprise.html?locale=en_us)

```bash
wget -O splunk-9.4.2-e9664af3d956-linux-amd64.tgz "https://download.splunk.com/products/splunk/releases/9.4.2/linux/splunk-9.4.2-e9664af3d956-linux-amd64.tgz"
sudo tar xvzf splunk-9.2.1-78803f08aabb-Linux-x86_64.tgz -C /opt
cd /opt/splunk/bin
sudo ./splunk start --accept-license
# enter username and password
# splunk interface will be available at port 8000
```

**Splunk Apps**

A Splunk App is a full-featured package that includes:

- Dashboards
- Reports
- Visualizations
- Saved searches
- Possibly also add-ons bundled within
- User interface elements
- e.g., AWS, Azure and GCP, Splunk Enterprise Security

**Splunk Add-ons:**

A Splunk Add-on or TA (Technology Add-on) is a supporting component that typically includes:

- No dashboards or UI
- Inputs (for collecting data)
- Field extractions
- Lookups


**SPL (Search Processing Language)**

**search commands:**

- search: retreives events from the index. e.g., search ERROR
- timechart: creates a time-based chart of results. e.g., count firewall actions over time, grouped by the action type.
- stats: computes aggregate statistics over the events. e.g., count events grouped by source IP address.
- eval: calculates an expression and assigns the result to a new field
- transaction: groups events that are related to a single event.
- lookup: enriches events with data from external sources.

**reporting and visualization commands**

- chart: generates chat from the results.
- table: displays result in table format.
- dedub: removes duplicate events from the results.


- [SPL Cheat Sheet](https://www.splunk.com/en_us/blog/learn/splunk-cheat-sheet-query-spl-regex-commands.html)


**Report:** A saved search that displays results and can be scheduled or shared.

**Dashboard:** A visual collection of panels showing search results and metrics.

**Alert:** A triggered action based on search conditions to notify of specific events.

### HTTP Logs demo

- [http logs download](https://www.secrepo.com/maccdc2012/http.log.gz)

**fields to extract**

- src_ip
- dst_ip
- src_port
- dst_port
- method
- status


- create a table with required fields

```
table src_ip dst_ip method status

```


**Analyze Web Traffic Patterns**


1. distribution of request methods (GET, POST, etc.) to understand web traffic patterns.

```
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats count by method
```

2. Identify top URLs or endpoints or destination ip accessed by users.

```
index=<your_http_index> sourcetype=<your_http_sourcetype>
| top limit=10 dst_ip
```

3. Analyze response codes to identify errors or successful requests.

```
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats count by status
```

4. Analyze high volumes of error responses

```
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats count by status
| where status >= 400
```

5. Pie Chart for HTTP Methods Distribution


```
| stats count by method
| sort - count
| rename method as "HTTP Method", count as "Request Count"
| chart sum("Request Count") by "HTTP Method"
```

6. Bar Chart for Top Destination IPs

```
index=<your_http_index> sourcetype=<your_http_sourcetype>
| top limit=10 dst_ip
| rename dst_ip as "Destination IP", count as "Request Count"
| sort - "Request Count"

```

7. Alert for Unusually High Traffic (Sudden Spike in Requests)


```
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats count by dst_ip
| sort - count
```
Alert:

```
| stats count by dst_ip
| where count > 3000
```


**Self Study Activity:**


SSH (Secure Shell) log files contain valuable information about remote access to servers, including login attempts, commands executed, and session details. Analyzing SSH logs using Splunk SIEM enables security professionals to monitor access to critical systems, detect anomalies, and identify potential security threats.


- [ssh logs download](https://www.secrepo.com/maccdc2012/ssh.log.gz)

1. Determine the distribution of SSH commands executed
2. Identify top users or source IP addresses accessing the SSH server
3. Analyze successful vs. failed SSH login attempts
4. Analyze failed login attempts
5. Identify users or source IP with multiple failed login attempts
7. visualization: 
    - Success vs Failed login pie chart
    - Trend of failed logins over time (timechart)
8. Alert when any single user has more than 5 failed login attempts in 10 minutes.

-  [free splunk courses](https://www.splunk.com/en_us/training/course-catalog.html?sort=Newest&filters=filterGroup1FreeCourses)
-  [splunk core certified user](https://www.splunk.com/en_us/training/course-catalog.html?sort=Newest&filters=filterGroup1FreeCourses%2CfilterGroup2SplunkCoreCertifiedUser)













































