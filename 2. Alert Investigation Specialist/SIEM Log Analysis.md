
# 🔎 Writing SPL

Search Processing Language (SPL) is the standard for querying big data in Splunk.

📝 The Scenario

Your boss says "Find all failed logins from China in the last 24 hours."

📝 The Raw Query (SPL)

COPY_CODE

```splunk
index=main sourcetype=wineventlog:security EventCode=4625 
| iplocation SourceNetworkAddress 
| search Country="China"
| stats count by SourceNetworkAddress, User
| sort - count
```

🕵️‍♂️ Investigation Steps
### Step 1: Filter Phase

- `index=main`: Search only the main storage.
- `EventCode=4625`: Windows Failed Identity.
### Step 2: Enrichment

- `iplocation`: Splunk automatically adds `Country`, `City`, `Lat`, `Lon` based on the IP address.
### Step 3: Aggregation

- `stats count by...`: Instead of showing 10,000 individual log lines, show me a table.
- **Row 1**: IP `1.2.3.4`, User `Admin`, Count `500`.
- **Conclusion**: We are under a brute force attack.
# 🔗 Correlating Events

A single log is a dot. Correlation connects the dots to make a picture.

📝 The Raw Logs (Sequence)

COPY_CODE

```text
[09:00:01] Failed Login (User: Bob, IP: 10.0.0.50)
[09:00:02] Failed Login (User: Bob, IP: 10.0.0.50)
[09:00:03] Failed Login (User: Bob, IP: 10.0.0.50)
... (50 more times)
[09:01:00] Successful Login (User: Bob, IP: 10.0.0.50)
[09:01:05] User Created (User: Admin2, By: Bob)
```

🕵️‍♂️ Investigation Steps
### Step 1: Brute Force

- The first block is a classic brute force attack.
### Step 2: The Breakthrough

- `[09:01:00]`: The attack succeeded. They guessed the password.
### Step 3: Persistence

- `[09:01:05]`: The very first thing the attacker did was create a BACKDOOR user (`Admin2`).
- **Why**: Even if Bob changes his password, the attacker can still login as Admin2.
### Step 4: The Correlation Rule

- `Trigger Alert IF (Failed_Login > 10 in 5 mins) FOLLOWED BY (Successful_Login)`