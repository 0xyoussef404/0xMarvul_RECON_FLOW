# New Features Usage Guide

This document provides detailed examples of using the new reconnaissance features added to 0xMarvul RECON FLOW.

## 🔧 CMS Analysis (`-cms`)

Detects and scans WordPress installations for known vulnerabilities using wpscan.

### What it does:
- Automatically detects WordPress sites from live hosts
- Scans for vulnerable plugins, themes, and core versions
- Identifies known CVEs in WordPress installations
- Outputs results to `cms_scan.txt`

### Example Usage:
```bash
./0xMarvul_RECON_FLOW.sh target.com -cms
```

### Output:
- **File**: `target.com/cms_scan.txt`
- Contains vulnerability details including CVE IDs, affected components, and severity levels

### Requirements:
- wpscan must be installed
- Internet connection for vulnerability database updates

---

## 🔗 JavaScript Endpoint Extraction (`-jsendpoints`)

Extracts hidden API endpoints and paths from JavaScript files using LinkFinder.

### What it does:
- Analyzes all collected JavaScript files
- Extracts potential API endpoints and hidden paths
- Identifies URLs that may not be discoverable through crawling
- Saves unique endpoints to `endpoints.txt`

### Example Usage:
```bash
./0xMarvul_RECON_FLOW.sh target.com -jsendpoints
```

### Combine with secret finding:
```bash
./0xMarvul_RECON_FLOW.sh target.com -secret -jsendpoints
```

### Output:
- **File**: `target.com/endpoints.txt`
- Contains extracted endpoints and paths from JavaScript analysis

### Use Case:
Perfect for discovering hidden admin panels, API endpoints, and internal routes that aren't linked in the main application.

---

## 📸 Screenshot Capture (`-screenshot`)

Captures visual screenshots of all live hosts using gowitness.

### What it does:
- Takes screenshots of every live host discovered
- Helps quickly identify interesting pages without manually visiting each
- Filters out dead pages and default installations visually
- Saves screenshots to `screenshots/` directory

### Example Usage:
```bash
./0xMarvul_RECON_FLOW.sh target.com -screenshot
```

### Output:
- **Directory**: `target.com/screenshots/`
- Contains PNG/JPG screenshots of each live host
- Filenames typically include the URL or hostname

### Use Case:
When dealing with 100+ subdomains, screenshots help you quickly:
- Identify active vs default pages
- Spot interesting web applications
- Prioritize testing targets
- Share findings visually with team members

---

## ⚠️ Critical Vulnerability Scanning (`-vuln`)

Scans for critical and high-severity vulnerabilities using Nuclei templates.

### What it does:
- Runs Nuclei with critical and high severity templates only
- Focuses on serious vulnerabilities like:
  - Log4j (Log4Shell)
  - Spring4Shell
  - .env file exposure
  - Critical RCE vulnerabilities
  - Sensitive data exposure
- Outputs findings to `vuln_scan.txt`

### Example Usage:
```bash
./0xMarvul_RECON_FLOW.sh target.com -vuln
```

### Combine with takeover check:
```bash
./0xMarvul_RECON_FLOW.sh target.com -takeover -vuln
```

### Output:
- **File**: `target.com/vuln_scan.txt`
- Contains vulnerability details with severity, matched template, and affected URLs

### Difference from `-takeover`:
- `-takeover`: Specifically checks for subdomain takeover vulnerabilities
- `-vuln`: Scans for critical/high severity vulnerabilities across all URLs

---

## 🚀 Combined Usage Examples

### Full Advanced Recon:
```bash
./0xMarvul_RECON_FLOW.sh target.com \
  -parallel \
  -moreurls \
  -cms \
  -jsendpoints \
  -screenshot \
  -secret \
  -vuln \
  -gf \
  -grep
```

### Quick Visual Assessment:
```bash
./0xMarvul_RECON_FLOW.sh target.com -screenshot -cms
```

### Deep Endpoint Discovery:
```bash
./0xMarvul_RECON_FLOW.sh target.com -moreurls -jsendpoints -secret
```

### Security-Focused Scan:
```bash
./0xMarvul_RECON_FLOW.sh target.com -cms -vuln -takeover -secret
```

---

## 📊 Expected Output Structure

After running with all new features:

```
target.com/
├── cms_scan.txt              # WordPress vulnerability scan results
├── endpoints.txt             # Extracted JS endpoints
├── screenshots/              # Visual screenshots
│   ├── site1.png
│   ├── site2.png
│   └── ...
├── vuln_scan.txt            # Critical vulnerabilities found
├── [... other standard files ...]
```

---

## ⚡ Performance Tips

1. **CMS Scanning**: Can be slow on large scans. Use only when WordPress sites are expected.
2. **Screenshots**: Most resource-intensive. Consider using on smaller target lists first.
3. **JS Endpoints**: Fast and lightweight. Recommended for most scans.
4. **Vuln Scanning**: Moderate speed. Templates are well-optimized.

---

## 🔔 Discord Notifications

All new features report their findings via Discord notifications:
- CMS vulnerabilities count
- JS endpoints extracted
- Screenshots captured
- Critical vulnerabilities found

---

## 🛠️ Installation Requirements

### CMS Analysis:
```bash
gem install wpscan
```

### JS Endpoints:
```bash
pip install linkfinder
# or
git clone https://github.com/GerbenJavado/LinkFinder.git
cd LinkFinder && python setup.py install
```

### Screenshots:
```bash
go install github.com/sensepost/gowitness@latest
```

### Vulnerability Scanning:
```bash
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
nuclei -update-templates
```

---

## 📝 Notes

- All features work independently and can be combined
- Features respect the graceful skip functionality (press ENTER to skip)
- Output files are only created when features are enabled
- Discord notifications adapt based on which features are active

---

## 🐛 Troubleshooting

**CMS scan finds no WordPress sites:**
- Verify sites are actually WordPress-based
- Check that tech_detect.txt is being generated
- Try manually visiting /wp-login.php on suspected sites

**No endpoints extracted:**
- Ensure JavaScript files were collected (check javascript.txt)
- Verify LinkFinder is properly installed
- Some sites may not expose endpoints in JS files

**Screenshots not capturing:**
- Verify gowitness is installed and in PATH
- Check if sites are accessible from your network
- Some sites may block automated screenshot tools

**Vuln scan shows no results:**
- This is actually good! No critical vulnerabilities found
- Verify Nuclei templates are updated
- Try running Nuclei manually on a known vulnerable target

---

## 🎯 Best Practices

1. Start with basic recon before using all features
2. Use `-screenshot` on smaller subdomain lists initially  
3. Combine `-cms` with `-vuln` for WordPress targets
4. Use `-jsendpoints` with `-secret` for comprehensive JS analysis
5. Review Discord notifications for quick insights
6. Archive successful scans for comparison with future runs

