# Cyber-Threat-Intelligence-Report-Business-Email-Compromise-BEC-


 # 1. Identify the Attack Type**


**Attack Description**

This email is a Business Email Compromise (BEC) attempt in which the attacker impersonates a company's CEO to convince the finance department to perform an unauthorized wire transfer.
Rather than using malware or malicious attachments, the attacker relies on social engineering and executive impersonation to manipulate the recipient into transferring money.

 **Why this is a BEC attack**

1. CEO impersonation
2. Financial fraud objective
3. Urgent payment request
4. No legitimate authentication
5. Use of deceptive email infrastructure


# 2. Analyze the Email Header

**Subject**

```splunk
 Immediate Action Required: Wire Transfer Request
```
The subject creates a sense of urgency, encouraging the recipient to act without proper verification.

---

**From**

```splunk
 CEO <ceo@ceorp.com>
```

The sender claims to be the CEO.

However, this identity cannot be trusted because the authentication mechanisms fail.

---

**Return-Path**

```splunk
 <ceo@ceorp.com>
```
The Return-Path matches the visible sender, but this alone does not prove authenticity because attackers can spoof this value.

---

**Authentication Results**

```splunk
 spf=fail
dkim=fail
dmarc=fail
```

This is the strongest technical evidence that the email is fraudulent.

**SPF Failed**

The sending server is not authorized to send email for ceorp.com.

**DKIM Failed**

The digital signature could not be validated, indicating the email was not cryptographically verified.

**DMARC Failed**

DMARC combines SPF and DKIM validation. Since both failed, DMARC also failed.

This strongly suggests sender spoofing.

---

 **Message-ID**

 ```splunk
 <6666@attacker-infra.com>
```
A legitimate corporate email would normally generate a Message-ID associated with the organization's mail system.

Instead, this email originates from an attacker-controlled domain.

---

 **Received Headers**

 ```splunk
  Received:
from attacker-infrastructure.com
```
This identifies the first mail server that handled the email.

Instead of an official corporate mail server, the message originated from attacker-infrastructure.com, indicating attacker-controlled infrastructure

---  

# 3. Identify Attacker Infrastructure

Several indicators reveal the attacker's infrastructure.

Infrastructure Used
Domain

```splunk
 attacker-infrastructure.com
```
Likely used as the outbound SMTP server.

---

 **Local SMTP Host**

 ```splunk
  localhost.localdomain
127.0.0.1
```
Shows the attacker generated the email from a local SMTP service before relaying it externally.

---

 **Message-ID Domain**

 ```splunk
 attacker-infra.com
```
This domain appears inside the Message-ID and likely belongs to the attacker's infrastructure.

 ---
 
 **Infrastructure Summary**

 ---

 ## Attacker Infrastructure

| Infrastructure | Purpose |
|---------------------------|------------------------|
| attacker-infrastructure.com | SMTP relay |
| attacker-infra.com | Message-ID generation |
| localhost.localdomain | Local mail server |
---

# 4. Extract Indicators of Compromise (IOCs)

**Domains**

```splunk
 ceorp.com (spoofed)
attacker-infrastructure.com
attacker-infra.com
```
---

**Email Address**

```splunk
 ceo@ceorp.com
```
Spoofed sender identity.

---

**Message-ID**

```splunk
 6666@attacker-infra.com
```

---

**Subject**

```splunk
 Immediate Action Required:
Wire Transfer Request
```
Useful for email hunting.

---

**Authentication Indicators**

```splunk
 SPF Failure
DKIM Failure
DMARC Failure
```

---

**Recipient Target**

```splunk
finance@recipient.com
```

---

# 5. Finance teams are common BEC targets.

## MITRE ATT&CK Mapping

| Technique | MITRE ATT&CK ID | Evidence |
|-----------|-----------------|----------|
| Phishing | T1566 | Fraudulent email sent to the finance department |
| Spearphishing | T1566.001 | Targeted email requesting a wire transfer |
| Impersonation | T1656 | Attacker impersonates the CEO |
| Trusted Relationship | T1199 | Exploits trust between executives and finance staff |

---















