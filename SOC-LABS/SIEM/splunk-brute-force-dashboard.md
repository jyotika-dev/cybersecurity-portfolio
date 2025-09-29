# Splunk Brute Force Login Attempts Dashboard

## Objective
Build a Splunk dashboard to detect brute force login attempts by analyzing failed login events and summarizing suspicious IP addresses, login failures over time, and status code distributions. This aids SOC analysts in early attack detection.

<img width="1879" height="915" alt="image" src="https://github.com/user-attachments/assets/62a04950-6b88-4e2e-a034-0208389e192c" />
<img width="1866" height="801" alt="image" src="https://github.com/user-attachments/assets/bf42fee2-3a1c-4f5d-8b1a-bfbb8a468278" />


---

## SPL Queries / Detection Logic

### 1. Offending IPs (Login Failures > 10)
```spl
index=main status=401 OR status=403 OR status=404
| stats count by clientip
| where count > 10
| sort -count
```

*Identifies IP addresses with more than 10 login failures/denied requests.*

![Denied requests IPs](https://github.com/user-attachments/assets/fd8b6348-d8e3-42aa-9051-c11152b72e3a)



### 2. Top 10 Attacking IPs (Failed Logins)
```spl
index=main status=401 OR status=403 OR status=404
| top limit=10 clientip
```

*Shows the top 10 IPs with the highest failed login attempts.*

![Top 10 offending ips](https://github.com/user-attachments/assets/6653b222-7d0b-4cec-a6f5-57f8fd145040)



### 3. Failed Login Attempts Over Time (per IP)
```spl
index=main status=401 OR status=403 OR status=404
| timechart count by clientip
```

*Visualizes the frequency of failed login attempts over time.*

![Timechart (attack over time)](https://github.com/user-attachments/assets/255a89ca-5f11-4e4d-afdf-6c12a2987b68)


### 4. Status Code Breakdown by IP (Success vs Failures)
```spl
index=main (status=200 OR status=401 OR status=403 OR status=404)
| stats count by clientip status
| sort clientip
```

*Shows counts of successful and failed logins per client IP for comprehensive analysis.*

![Combine Success + Failures](https://github.com/user-attachments/assets/89450d82-463f-412d-9a4f-543469c8d0fa)


---

## Dashboard Panels

- **Offending IPs (Login Failures > 10)**: Table listing IPs with more than 10 failed logins.
- **Top 10 Attacking IPs (Failed Logins)**: Bar chart of the top attacking IPs.
- **Failed Login Attempts Over Time (per IP)**: Line chart tracking attack activity by IP hourly.
- **Status Code Breakdown by IP**: Table showing login success and failure counts per IP.

---

## Setup and Usage

1. Save each SPL query as a report in Splunk with meaningful names (e.g., `bf_suspicious_ips_count_gt10`).
2. Create a new Dashboard named **Brute Force Login Attempts**.
3. Add panels to the dashboard using saved reports and assign the panel titles above.
4. Configure a global time picker (default Last 24 hours) and set refresh intervals (1-5 mins recommended).
5. (Optional) Create alerts on the `bf_suspicious_ips_count_gt10` report for email/webhook notifications.
6. Upload relevant log data (e.g., Apache logs) if no production data exists for testing.

---

## Summary

This project demonstrates practical SIEM monitoring skills by building an effective brute force attack detection dashboard with real-time visualization and alerting. It can be adapted to various log sources and extended with GeoIP lookups, notable events, and enriched threat intelligence for advanced SOC capabilities.

---

*Feel free to customize the project to fit your environment or expand the use case to other attack types.*
