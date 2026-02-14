# Quick Reference: New Features

## Command Flags

| Flag | Tool | Purpose | Output File |
|------|------|---------|-------------|
| `-cms` | wpscan | WordPress vulnerability scanning | `cms_scan.txt` |
| `-jsendpoints` | LinkFinder | Extract hidden endpoints from JS | `endpoints.txt` |
| `-screenshot` | gowitness | Capture screenshots of live hosts | `screenshots/` |
| `-vuln` | Nuclei | Scan for critical vulnerabilities | `vuln_scan.txt` |

## Installation Commands

```bash
# CMS Analysis
gem install wpscan

# JS Endpoint Extraction  
pip install linkfinder

# Screenshot Capture
go install github.com/sensepost/gowitness@latest

# Vulnerability Scanning
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
nuclei -update-templates
```

## Quick Examples

### Basic Usage
```bash
# CMS scanning only
./0xMarvul_RECON_FLOW.sh target.com -cms

# JS endpoint extraction only
./0xMarvul_RECON_FLOW.sh target.com -jsendpoints

# Screenshot capture only
./0xMarvul_RECON_FLOW.sh target.com -screenshot

# Vulnerability scanning only
./0xMarvul_RECON_FLOW.sh target.com -vuln
```

### Combined Usage
```bash
# All new features
./0xMarvul_RECON_FLOW.sh target.com -cms -jsendpoints -screenshot -vuln

# Security-focused
./0xMarvul_RECON_FLOW.sh target.com -cms -vuln -takeover

# Endpoint discovery
./0xMarvul_RECON_FLOW.sh target.com -jsendpoints -secret -moreurls

# Complete recon with all features
./0xMarvul_RECON_FLOW.sh target.com -parallel -moreurls -cms -jsendpoints -screenshot -vuln -secret -takeover -gf -grep -port
```

## Expected Results

### CMS Scan (`cms_scan.txt`)
```
[+] WordPress version 5.8.1 identified
[!] Vulnerabilities:
    - CVE-2021-xxxxx: WordPress Core SQL Injection
    - CVE-2021-xxxxx: Plugin XYZ RCE
```

### JS Endpoints (`endpoints.txt`)
```
/api/v1/users
/admin/dashboard
/internal/metrics
https://api.target.com/v2/data
```

### Screenshots (`screenshots/`)
```
screenshots/
├── https_target_com.png
├── https_api_target_com.png
├── https_admin_target_com.png
└── ...
```

### Vulnerability Scan (`vuln_scan.txt`)
```
[high] [CVE-2021-44228] Log4j RCE on https://target.com/app
[critical] [env-exposure] .env file exposed at https://api.target.com/.env
```

## Discord Notification Output

When enabled, you'll receive notifications showing:
- 🔧 CMS Vulns: X found
- 🔗 JS Endpoints: X found  
- 📸 Screenshots: X captured
- ⚠️ Vulnerabilities: X found

## Workflow Integration

```
Standard Recon Flow:
1. Subdomain Enumeration
2. Live Host Detection
3. Technology Detection
   ├─→ [NEW] CMS Analysis (-cms)
4. URL Gathering
5. JavaScript Extraction
   ├─→ [NEW] JS Endpoint Extraction (-jsendpoints)
6. Parameter Discovery
7. [NEW] Screenshot Capture (-screenshot)
8. Vulnerability Scanning
   ├─→ Subdomain Takeover (-takeover)
   └─→ [NEW] Critical Vuln Scan (-vuln)
9. Final Summary & Notifications
```

## Pro Tips

✓ Use `-cms` when WordPress is detected in tech_detect.txt
✓ Combine `-jsendpoints` with `-secret` for complete JS analysis
✓ Use `-screenshot` on smaller subdomain lists first (resource-intensive)
✓ `-vuln` focuses on critical/high severity only (faster than full Nuclei scan)
✓ All new features respect the graceful skip (press ENTER to skip)

## Troubleshooting

**Tool not found?**
- Run with the flag to see which tool is missing
- Check the dependency check output
- Install missing tools using commands above

**No results?**
- CMS: May not be WordPress sites
- JS Endpoints: No JavaScript files collected
- Screenshots: Sites may be blocking automation
- Vulnerabilities: Good news! No critical issues found

**Slow performance?**
- Use flags selectively based on target
- `-screenshot` is most resource-intensive
- Consider using on subsets of large target lists

