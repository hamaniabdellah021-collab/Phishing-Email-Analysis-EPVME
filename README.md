# Phishing-Email-Analysis-EPVME
SOC Level 1 Phishing Email Analysis &amp; Threat Intelligence project analyzing malicious samples from the EPVME dataset. Includes header inspection, IOC extraction, and incident reports.


# 🚨 Security Analysis & Header Investigation Report

## 1. Sender & Recipient Analysis
* **Malformed / Spoofed Sender Header:** `From: <jeanne.fore@enron.com}attack.com>`
* **Syntax Malformation:** The closing brace `}` inside the domain is a syntax error, likely designed to bypass basic email filter parsing logic.
* **Domain Spoofing / Subdomain Obfuscation:** The attacker attempts to mimic `enron.com`, but the actual top-level domain receiving/processing the request is `attack.com`. The mail client display name tricks the victim into seeing an internal Enron address.
* **Mass Target List (`To`):** Multiple recipients inside the same domain (`kay.mann`, `steven.krimsky`, `eric.thode`), indicating a wide broadcast spray attack rather than a targeted spear-phishing attempt.

## 2. Subject Line & Social Engineering
* **Subject:** `We cure any desease!`
* **Spam Pattern:** Promises miraculous medical cures ("pharma spam").
* **Typo Indicator:** Misspelling of "disease" as `desease`, a common characteristic of unvetted automated spam campaigns.

## 3. Client & Mailer Fingerprinting
* **X-Mailer:** `Microsoft Outlook Express 6.00.2900.2180`
* **X-MimeOLE:** `Produced By Microsoft MimeOLE V6.00.2900.2180`
* **Legacy Software Signature:** Outlook Express 6 (Windows XP era) is widely used by spam-botnets generating automated mail bursts using outdated user-agent strings.

## 4. Security Filter Scanning Logs
* **X-Miltered:** Filtered by Joe's `j-chkmail` on host `psyche`.
* **X-Virus-Scanned:** Scanned via `ClamAV 0.90.2`.
* **X-Virus-Status:** `Clean`

> [!WARNING]
> **Critical Note:** "Clean" only means no known binary malware signature was detected in the payload at the time of scanning. It does not mean the email is safe—it can still be a phishing link, credential harvester, or scam.

## 5. Missing Authentication Headers
* **Absence of `Authentication-Results` (SPF, DKIM, DMARC):** The header lacks SPF (`Received-SPF`) or DKIM signatures.
* **Verification Failure:** Because the sending server domain (`attack.com`) does not match the internal domain claimed in the display address (`enron.com`), an SPF/DMARC check would yield a **FAIL** verdict.
