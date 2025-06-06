# 🛡️ Custom SOC Ticketing System + Wazuh SIEM Integration

This project simulates the full lifecycle of a SOC (Security Operations Center) alert pipeline. A custom-built Flask-based ticketing system generates log entries on ticket creation or updates, which are parsed, decoded, and forwarded into the Wazuh SIEM. These entries trigger real-time alerts based on custom-defined rules, and are visualized and verified via OpenSearch Dashboards.

---

## 📌 Project Objective
Design and build a ticketing system that integrates seamlessly into a SIEM workflow — enabling blue team analysts to generate logs through a web interface and track them through Wazuh into OpenSearch in real time. This project bridges developer operations with blue team monitoring practices.

---

## 🧱 Key Features

### 1. Ticketing System (Python Flask)
- CRUD-enabled web application for ticket management
- SQLite backend with two relational tables:
  - `tickets`: includes title, description, priority, source, status, and timestamp
  - `updates`: includes ticket ID references, notes, and update timestamps
- HTML front-end interface with basic form handling

### 2. Log Integration
- Flask app writes structured log messages to `/var/log/soc_tickets.log`
- Logs are formatted to include:
  - Timestamp
  - Status (INFO)
  - Action (`New Ticket Created`, `Ticket Updated`, etc.)
  - Ticket metadata (e.g. title, priority, source)

**Example Log:**
```
2025-06-06 12:23:55,318 - INFO - New Ticket Created | Title: Wazuh Test Ticket | Priority: Low | Source: Wazuh | Time: 2025-06-06 12:23:55
```

### 3. Wazuh SIEM Pipeline
- Created a new `<localfile>` entry in `ossec.conf` to ingest the log file
- Wrote a custom decoder (`soc_ticketing_decoder.xml`) to extract fields from log messages:
  ![Image](https://github.com/user-attachments/assets/6c0e9da6-e931-42a5-b4bd-d6bcc6a5016b)

- Wrote a custom rule (`soc_ticketing_rules.xml`) to match new ticket logs:
  ![Image](https://github.com/user-attachments/assets/aaa969f2-71d1-4e3e-90d6-07d3ab16d95b)
  - `rule.id`: `100101`
  - `rule.description`: `New SOC Ticket Created from Flask App`
  - `level`: `6`
  - `group`: `soc-ticketing`

### 4. Log Verification & Testing

- Restarted Wazuh manager and tailed `ossec.log` to verify ingestion
- Successfully created and updated tickets through Flask app
- Confirmed Wazuh alerts:
  - Rule fired correctly
  - Fields parsed and labeled
  - JSON/log output matched expectations in OpenSearch Dashboards

---

## 🧠 Skills Demonstrated
- Flask web development (Python backend, HTML frontend)
- Relational DB schema design and implementation (SQLite)
- Log formatting and file permission handling
- Wazuh decoder/rule authoring and alert pipeline configuration
- SIEM alert validation with OpenSearch

---

## 📊 Visual Verification
Screenshots from OpenSearch Dashboards confirm:
- Log ingestion from `/var/log/soc_tickets.log`
- Proper decoder and rule matching
- Alerts triggering with correct field mapping

---

## 🧪 Future Enhancements
- User authentication and role-based access to the ticketing app
- Email/Slack integration for ticket updates
- More granular rules with anomaly detection thresholds
- Dockerized deployment for cross-environment portability

---

## 🧩 Repo Contents
- `app.py`: Flask backend
- `templates/`: HTML files for frontend
- `soc_ticketing_decoder.xml`: Wazuh log decoder
- `soc_ticketing_rules.xml`: Wazuh rule file
- `screenshots/`: Visual documentation of alert workflow

---

## 📅 Project Date
**June 6, 2025**

---

