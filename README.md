

# 🔐 Enhanced Vulnerability Scanner

A professional, ethical, production-grade vulnerability scanner inspired by Nmap, Nikto, XSStrike, sqlmap (detection only), and Burp Suite. Built with Python asyncio for high performance and modular design.

![Python Version](https://img.shields.io/badge/python-3.8+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Security](https://img.shields.io/badge/security-ethical-red.svg)

## ⚠️ LEGAL DISCLAIMER

**This tool is for AUTHORIZED TESTING ONLY!**

- You must have **explicit written permission** to scan any target
- Unauthorized scanning is **illegal** and may violate:
  - Computer Fraud and Abuse Act (CFAA)
  - GDPR and privacy regulations
  - Local and international cybercrime laws
- Misuse can result in **criminal prosecution** and **civil liability**
- The author assumes **NO liability** for unauthorized use

## ✨ Features

### 🛡️ Ethical & Legal Safeguards
- Mandatory authorization confirmation with exact phrase
- Detailed audit logging of all actions
- Built-in rate limiting to prevent abuse
- Safe, non-destructive scanning by default
- Configurable aggressive mode for deeper testing

### 🎯 Intelligent Target Handling
- Accepts IP addresses, domains, and full URLs
- Automatic HTTPS detection and upgrade
- Follows redirect chains
- Preserves original domain information
- Shows protocol, port, and redirect history

### 🚀 High-Performance Architecture
- Asynchronous I/O with `asyncio` and `aiohttp`
- Worker pool pattern for optimal resource usage
- Modular class-based design
- Comprehensive exception handling
- Structured logging

### 🔍 Vulnerability Detection (Safe & Non-Destructive)

#### Port Scanning (Nmap-like)
- Async TCP port scanning
- Service fingerprinting with banner grabbing
- Version detection
- CVE mapping for known services
- Configurable port ranges

#### Web Vulnerability Checks

| Vulnerability | Detection Method | Confidence |
|--------------|------------------|------------|
| **SQL Injection** | Error-based, boolean-based, time-based | High/Medium/Low |
| **Reflected XSS** | Context-aware payload reflection | High/Medium |
| **Security Headers** | Missing/weak header detection | High |
| **Directory Exposure** | Sensitive path enumeration | Medium/High |
| **TLS/SSL Issues** | Certificate validation, weak protocols | High |
| **HTTP Methods** | Dangerous method detection | Medium |
| **CORS Misconfiguration** | Origin validation | Medium |

### 🛠️ Advanced Detection Capabilities

#### SQL Injection
- **Error-based**: Regex pattern matching with confidence weighting
- **Boolean-based**: Response similarity analysis
- **Time-based**: Safe delay payloads with timing analysis
- Multi-DBMS support (MySQL, MSSQL, PostgreSQL, SQLite)

#### Cross-Site Scripting (XSS)
- **Context-aware detection** (HTML, attributes, script blocks, URLs)
- **Encoding bypass detection** (URL, HTML, Unicode)
- **Case-insensitive matching** for weak filters
- 10+ payload types covering different contexts

### 📊 Professional Reporting

#### Terminal Output
- Color-coded severity levels
- Real-time progress updates
- Summary statistics
- Remediation guidance

#### JSON Report
- Machine-readable format
- Complete scan data
- Evidence and metadata
- Integration-ready

#### HTML Report
- Clean, professional styling
- Severity-based grouping
- Interactive elements
- Printable format
- CVE references
- Evidence snippets

### 🔧 External Tool Integration

Automatically detects and optionally runs:
- **nmap** - Network exploration
- **nikto** - Web server scanning
- **sqlmap** - SQL injection detection (safe mode)

Results are parsed and merged into final reports.

### 🌐 Additional Features
- **WAF Detection**: Identifies 15+ Web Application Firewalls
- **Subdomain Enumeration**: Discovers common subdomains
- **Technology Fingerprinting**: Detects server software and frameworks
- **CVE Mapping**: Links service versions to known vulnerabilities
- **Misconfiguration Checks**: HTTP methods, CORS, directory listings
- **Proxy Support**: HTTP/HTTPS/SOCKS proxies
- **User-Agent Rotation**: Avoids detection

## 📦 Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager
- (Optional) nmap, nikto, sqlmap for enhanced scanning

### Quick Install

```bash
# Clone the repository
git clone https://github.com/yourusername/enhanced-vulnerability-scanner.git
cd enhanced-vulnerability-scanner

# Create virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# (Optional) Install external tools
# Ubuntu/Debian:
sudo apt-get install nmap nikto sqlmap

# macOS:
brew install nmap nikto sqlmap
🚀 Usage
Basic Scan
bash
# Scan a domain
python scanner.py --target example.com

# Scan with IP address
python scanner.py --target 192.168.1.1

# Scan with specific port
python scanner.py --target https://example.com:8443
Advanced Options
bash
# Aggressive mode with all checks
python scanner.py --target example.com --aggressive

# Custom port range
python scanner.py --target example.com --ports 1-1000

# Multiple specific ports
python scanner.py --target example.com --ports 80,443,8080,8443

# With rate limiting and timeout
python scanner.py --target example.com --rate-limit 5 --timeout 10

# Using proxy
python scanner.py --target example.com --proxy http://127.0.0.1:8080

# Subdomain enumeration
python scanner.py --target example.com --enumerate-subdomains

# Custom output name
python scanner.py --target example.com --output my_scan_report
Command Line Options
Option	Description	Default
--target	Target IP, domain, or URL	Required
--ports	Ports to scan (common/all/range/list)	common
--timeout	Request timeout in seconds	5
--threads	Number of concurrent workers	100
--aggressive	Enable deeper (but safe) tests	False
--output	Base name for output files	scan_report
--proxy	HTTP proxy URL	None
--rate-limit	Max requests per second	10
--enumerate-subdomains	Discover common subdomains	False
--no-external	Skip external tool integration	False
--version	Show version information	
📊 Sample Output
Terminal Summary
text
================================================================================
                              SCAN COMPLETE
================================================================================
Target: example.com
Resolved: https://example.com:443
Protocol: HTTPS
Port: 443
Duration: 45.23 seconds
Total Requests: 156
Vulnerabilities Found: 3

Open Ports & Services:
PORT     SERVICE         VERSION         CONFIDENCE  CVEs
80       http            nginx 1.18.0    80%        
443      https           nginx 1.18.0    80%        
3306     mysql           8.0.28          90%        CVE-2022-21367

Vulnerabilities by Severity:

CRITICAL:
  • SQL Injection
    Endpoint: https://example.com?id=1' OR '1'='1
    Confidence: HIGH (95%)
    Fix: Use parameterized queries...

HIGH:
  • Missing Security Header: Content-Security-Policy
    Endpoint: https://example.com
    Confidence: HIGH (90%)
    Fix: Implement CSP header...
🏗️ Architecture
text
┌─────────────────────────────────────────────────────────────┐
│                     VulnerabilityScanner                     │
│                     (Main Orchestrator)                      │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│ TargetResolver │    │  PortScanner  │    │  WebScanner   │
│ • URL parsing  │    │ • Worker pool │    │ • SQLi checks │
│ • HTTPS detect │    │ • Banner grab │    │ • XSS checks  │
│ • WAF detection│    │ • Fingerprint │    │ • Headers     │
└───────────────┘    └───────────────┘    └───────────────┘
                                                      │
                                      ┌───────────────┼───────────────┐
                                      ▼               ▼               ▼
                              ┌───────────────┐┌───────────────┐┌───────────────┐
                              │   SQLi Checks ││   XSS Checks  ││    TLS/SSL    │
                              │ • Error-based ││ • Context     ││ • Certificate │
                              │ • Boolean     ││ • Encoding    ││ • Expiration  │
                              │ • Time-based  ││ • Reflection  ││ • Weak proto  │
                              └───────────────┘└───────────────┘└───────────────┘

┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│ ReportGenerator│    │ExternalToolRun│    │SubdomainEnum  │
│ • Terminal    │    │ • nmap        │    │ • Dictionary  │
│ • JSON        │    │ • nikto       │    │ • Resolution  │
│ • HTML        │    │ • sqlmap      │    │ • Validation  │
└───────────────┘    └───────────────┘    └───────────────┘
🔒 Security & Ethics
Built-in Safeguards
Authorization Required: Must type exact confirmation phrase

Audit Logging: All actions timestamped and logged

Rate Limiting: Prevents server abuse

No Exploitation: Detection only, no destructive payloads

Safe Defaults: Non-destructive scanning by default

Responsible Usage Guidelines
✅ Test your own systems

✅ Authorized penetration tests

✅ Security research with permission

❌ Never scan without authorization

❌ Don't use for malicious purposes

❌ Respect rate limits and robots.txt

📝 License
This project is licensed under the MIT License - see the LICENSE file for details.

🙏 Acknowledgments
Inspired by these excellent security tools:

Nmap - Port scanning and service detection

Nikto - Web server misconfiguration checks

XSStrike - Advanced XSS detection logic

sqlmap - SQL injection detection (safe mode only)

Burp Suite - Professional reporting and request transparency

🤝 Contributing
Contributions are welcome! Please ensure:

Code maintains ethical standards

No destructive payloads are added

Tests pass and coverage is maintained

Documentation is updated
