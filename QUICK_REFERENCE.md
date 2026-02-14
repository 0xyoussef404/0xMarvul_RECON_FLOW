# Quick Reference: New Features

## Command Flags

| Flag | Tool | Purpose | Output File |
|------|------|---------|-------------|
| `-cms` | wpscan | WordPress vulnerability scanning | `cms_scan.txt` |
| `-jsendpoints` | LinkFinder | Extract hidden endpoints from JS | `endpoints.txt` |
| `-screenshot` | gowitness | Capture screenshots of live hosts | `screenshots/` |
| `-vuln` | Nuclei | Scan for critical vulnerabilities | `vuln_scan.txt` |
| `-perm` | dnsgen + dnsx | Generate & validate subdomain permutations | `valid_permutations.txt` |
| `-fuzz` | kxss | Test parameters for XSS reflection | `potential_xss.txt` |

## Installation Commands

```bash
# Previous features
gem install wpscan                                        # CMS scanning
pip install linkfinder                                    # JS endpoints
go install github.com/sensepost/gowitness@latest         # Screenshots
nuclei -update-templates                                  # Vulnerabilities

# New features
pip install dnsgen                                        # Subdomain permutations
go install -v github.com/projectdiscovery/dnsx/cmd/dnsx@latest  # DNS validation
go install github.com/Emoe/kxss@latest                   # XSS fuzzing
pip install s3scanner                                     # Cloud storage scanning
```

## Quick Examples

### Basic Usage
```bash
# Subdomain permutations only
./0xMarvul_RECON_FLOW.sh target.com -perm

# XSS fuzzing only
./0xMarvul_RECON_FLOW.sh target.com -fuzz

# Cloud storage scanning (auto with grep)
./0xMarvul_RECON_FLOW.sh target.com -grep
```

### Combined Usage
```bash
# All new features
./0xMarvul_RECON_FLOW.sh target.com -perm -fuzz -grep

# Security-focused
./0xMarvul_RECON_FLOW.sh target.com -perm -fuzz -grep -cms -vuln

# Complete recon with all features (old + new)
./0xMarvul_RECON_FLOW.sh target.com -parallel -moreurls -perm -cms -jsendpoints -screenshot -vuln -fuzz -secret -takeover -gf -grep
```

## Expected Results

### Subdomain Permutations (`valid_permutations.txt`)
```
dev-api.target.com
staging-admin.target.com
test-app.target.com
```

### XSS Fuzzing (`potential_xss.txt`)
```
https://target.com/search?q=REFLECTION
https://target.com/page?name=REFLECTION
https://target.com/user?id=REFLECTION
```

### Cloud Storage Scan (`cloud_vulnerabilities.txt`)
```
[PUBLIC] s3://target-backups.s3.amazonaws.com
[WRITABLE] target-uploads.s3.amazonaws.com
[OPEN] target-data.blob.core.windows.net
```

### Summary File (`summary.txt`)
```
=================================================
0xMarvul RECON FLOW - Scan Summary
=================================================
Target: target.com
Scan Date: 2024-XX-XX
Duration: 5m 23s

Total Subdomains: 150
Live Hosts: 45
Subdomain Permutations: 12
Potential XSS: 3
Cloud Storage Vulns: 2
=================================================
```

## Discord Notification Output

### Normal Completion
```
✅ Recon Complete
Finished scanning target.com
📍 Subdomains: 150
🌐 Live Hosts: 45
🔀 Permutations: 12
⚠️ Potential XSS: 3
☁️ Cloud Vulns: 2
⏱️ Duration: 5m 23s

[Attached: summary.txt]
[Attached: all_subs.txt]
```

### Critical Alerts
```
⚠️ Potential XSS Found!
Found 3 parameters with reflection on target.com

🚨 Cloud Storage Vulnerabilities!
Found 2 potential misconfigurations on target.com
```

## Workflow Integration

```
Standard Recon Flow:
1. Subdomain Enumeration
2. [NEW] Subdomain Permutations (-perm)
3. DNS Resolution
4. Live Host Detection
5. Technology Detection
6. CMS Analysis (-cms)
7. URL Gathering
8. Parameter Discovery
9. [NEW] XSS Fuzzing (-fuzz)
10. JavaScript Extraction
11. [NEW] JS Endpoint Extraction (-jsendpoints)
12. Grep Juicy URLs
13. [NEW] Cloud Storage Scanning (auto with -grep)
14. GF Patterns (-gf)
15. Directory Bruteforce (-dir)
16. Secret Finding (-secret)
17. [NEW] Screenshot Capture (-screenshot)
18. Vulnerability Scanning
    ├─→ Subdomain Takeover (-takeover)
    └─→ [NEW] Critical Vuln Scan (-vuln)
19. Final Summary & [NEW] Discord File Upload
```

## Pro Tips

✓ Use `-perm` early to expand subdomain list before live host check
✓ Combine `-fuzz` with `-grep` to find both XSS and sensitive URLs
✓ Cloud scanning is automatic when `-grep` is enabled
✓ Discord file attachments include summary.txt and all_subs.txt
✓ All new features respect graceful skip (press ENTER)
✓ XSS findings are sent as immediate Discord alerts
✓ Cloud vulnerabilities trigger critical Discord alerts

## Troubleshooting

**Tool not found?**
- Run with the flag to see which tool is missing
- Check the dependency check output
- Install missing tools using commands above

**No results?**
- Permutations: Need existing subdomains in all_subs.txt
- XSS Fuzzing: Need parameters in params.txt
- Cloud Scanning: Need grep enabled and cloud URLs found
- Discord Upload: Check webhook is configured

**Slow performance?**
- Permutations can be slow with many seeds - use graceful skip
- XSS fuzzing time depends on parameter count
- Cloud scanning is relatively fast
- Discord uploads are near-instant

## Feature Comparison

| Feature | Time Impact | Detection Rate | Best For |
|---------|-------------|----------------|----------|
| Subdomain Permutations | +2-5 min | 10-30% new | Large apps, cloud infra |
| XSS Fuzzing | +3-10 min | 5-15% params | Many parameters |
| Cloud Scanning | +1-3 min | High for public | AWS/Azure users |
| File Attachments | <10 sec | N/A | Teams, reporting |

