# 🎣 Phishing Types

📝 The Scenario

**Email 1**: "Dear Customer, your Netflix is expired. Click here." **Email 2**: "Hi Bob, I enjoyed our meeting about Project X in London. Please review the attached contract."

🕵️‍♂️ Investigation Steps
### Analysis of Email 1 (Bulk Phishing)

- **Greeting**: Generic ("Dear Customer").
- **Urgency**: High ("Expired").
- **Targeting**: None. Sent to millions.
### Analysis of Email 2 (Spear Phishing)

- **Greeting**: Personalized ("Hi Bob").
- **Context**: specific ("Project X", "London").
- **Source**: The attacker likely researched Bob on LinkedIn.
- **Danger**: Extremely high. Users trust context.
### 🛡️ Defense

For Spear Phishing, you rely on **User Awareness Training**. Technology often misses the context, but a human might spot that "Project X" was cancelled last week.# 

# URL Analysis

# 🔗 URL Analysis

📝 The Raw Link

`http://www.paypal.com.secure-login-updates.xyz/login.php`

🕵️‍♂️ Investigation Steps
### Step 1: The Root Domain

- **Reading**: Read from Right to Left.
- **TLD**: `.xyz` (Cheap, often malicious).
- **Domain**: `secure-login-updates.xyz`.
- **Subdomains**: `www.paypal.com`.
### Step 2: Homoglyphs

- Look closely at: `pаypal.com`.
- The 'a' might be a Cyrillic character (IDN Homograph Attack). Use a "Punycode Converter" to check.
- Real: `paypal.com`.
- Fake: `xn--pypal-4ve.com`.

### Step 3: Sandboxing

- Paste the URL into **UrlScan.io**.
- Look at the screenshot. Does it look like the real PayPal login page?
- If yes, it is a credential harvester.
