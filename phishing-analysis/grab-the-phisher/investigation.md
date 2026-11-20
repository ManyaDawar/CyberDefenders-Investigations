# GrabThePhisher — Investigation Report

## 1. Lab Overview

**Lab:** GrabThePhisher  
**Category:** Phishing Analysis / Threat Intelligence  
**Objective:** Investigate phishing infrastructure, identify credential collection mechanisms, understand data exfiltration, and extract relevant indicators of compromise.

## 2. Initial Investigation

### Initial Approach
- Examined the available files and folders.
- Identified suspicious HTML, PHP, image, and log files.
- Examined website screenshots and assets to understand the phishing page.
- Investigated how the different files interacted.

### Initial Observations
- Website assets included a website icon and PancakeSwap screenshots.
- The website imitated a PancakeSwap lottery campaign.
- `index.html` contained multiple cryptocurrency wallet options.
- MetaMask was handled separately and requested a recovery phrase.

## 3. Evidence Analysis

### A. Website Analysis — `index.html`

**Observations:**
- Identified a fake PancakeSwap lottery website.
- Found multiple cryptocurrency wallet options.
- Most wallet options did not request recovery phrases.
- MetaMask followed a separate flow.

**Significance:**
The website impersonated a legitimate cryptocurrency platform to make the phishing campaign appear trustworthy and encourage users to interact with the fake page.

### B. MetaMask Analysis — `metamask.php`

**Observations:**
- Identified a separate MetaMask-related directory.
- Found `metamask.php`.
- Identified code responsible for processing submitted information.
- Found the use of the Sypex Geo API to obtain geographic information.

**Significance:**
The PHP file was part of the credential collection mechanism. It processed submitted recovery phrases and gathered additional victim information.

### C. Log Analysis — `log/log.txt`

**Observations:**
- Found three recorded recovery phrases.

**Significance:**
The log suggests that three credential submissions were recorded.

**Limitation:**
The entries alone do not establish successful wallet access or financial loss.

### D. Telegram Exfiltration — `senTel()`

**Observations:**
- Identified a function responsible for sending collected information to a Telegram bot.
- Examined the bot identifier and token references.
- Identified the Telegram API URL.
- Observed message URL encoding.
- Identified an alias in the code comments.

**Significance:**
The function provides evidence of the phishing infrastructure's data exfiltration mechanism.

**Further investigation:**
- Determine the exact HTTP request method.
- Identify all transmitted fields.
- Establish whether transmission success is checked.
- Investigate the exposed bot configuration without interacting with a live malicious bot.

## 4. Attack Reconstruction

1. Victim visits the fake PancakeSwap lottery website.
2. Victim selects a wallet option.
3. MetaMask follows a separate recovery-phrase submission flow.
4. `metamask.php` processes submitted information.
5. Geographic information is obtained through the Geo API.
6. `senTel()` formats and transmits information to the Telegram bot.
7. Submitted phrases are recorded in the log file.

## 5. Indicators of Compromise (IOCs)

| Category | Finding |
|---|---|
| Files | `metamask.php`, `index.html`, `log.txt` |
| Network | Telegram Bot API endpoint |
| External service | Sypex Geo API |
| Behavioral | Recovery phrase collection |
| Exfiltration | Telegram bot communication |

Note: File names and API endpoints are investigation leads. Validate their malicious context before treating them as definitive IOCs. Never publish live bot tokens or recovery phrases.

## 6. MITRE ATT&CK Mapping

Potential techniques to investigate and map:

- T1056 — Input Capture
- T1041 — Exfiltration Over C2 Channel (only if the observed communication meets the technique's definition)

Mappings should be supported by the actual code and behavior.

## 7. Findings and Conclusion

The investigation identified a cryptocurrency phishing website impersonating PancakeSwap. The website offered multiple wallet options, with a separate MetaMask flow requesting recovery phrases.

The PHP infrastructure processed submitted information, collected geographic information through an external Geo API, and used a Telegram bot to transmit collected data.

Three recovery-phrase entries were identified in the log file, suggesting three recorded submissions.

However, the available evidence does not independently establish successful wallet access, unauthorized transactions, or financial loss.

## 8. Further Investigation

- Verify the exact fields transmitted through Telegram.
- Examine timestamps associated with the three submissions.
- Investigate the hosting infrastructure and related domains.
- Determine whether additional logs or artifacts establish successful wallet compromise.
- Research the identified Telegram bot infrastructure using safe, passive methods.

## 9. Lessons Learned

- Understand the difference between credential collection and successful account compromise.
- Trace information from collection to exfiltration.
- Extract and categorize IOCs.
- Correlate multiple artifacts to reconstruct an attack.
- Distinguish confirmed findings from assumptions.