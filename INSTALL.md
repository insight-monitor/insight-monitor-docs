---
type: process
domain: architecture
priority: high
status: raw
audience: developer
---

# Installation Guide

For the decision between approaches, see [[DECISION-Installation-Approach]].

## Prerequisites
- Git installed
- Node.js 18+ or Python 3.10+ (depending on implementation)
- 8GB RAM minimum (16GB recommended for local AI inference)
- Windows, macOS, or Linux

## Option 1: GUI installation (recommended)

### Download
1. Go to the Releases page
2. Download the latest installer for your OS
3. Run the installer

### Configuration wizard
On first launch, the app guides you through:

1. **Role setup** — Select or describe your role (Employee, Student, Freelancer)
2. **Schedule setup** — Define your working/study hours
3. **App classification** — Tag the applications you use (work-critical, optional, personal)
4. **Sensitivity settings** — Define keywords or domains for enhanced privacy
5. **Data storage** — Choose local-only or optional cloud sync

After configuration, monitoring starts automatically.

## Option 2: Clone and run locally (advanced)

```bash
git clone https://github.com/insight-monitor/insight-monitor.git
cd insight-monitor

# Install dependencies
npm install
# or
pip install -r requirements.txt

# Configure
cp config.example.yaml config.yaml
# Edit config.yaml with your preferences

# Run
npm run dev
# or
python main.py
```

### Configuration file reference
```yaml
user:
  role: "Senior Developer"
  schedule:
    start: "09:00"
    end: "18:00"
    breaks: ["12:00-13:00"]
  app_classification:
    work_critical: ["code", "terminal", "github"]
    work_optional: ["youtube", "slack"]
    personal: ["spotify", "instagram"]
  sensitivity:
    keywords: ["therapy", "lawyer", "doctor"]
    domains: ["bank.com", "healthcare.gov"]
```

## Post-installation
- Review the dashboard to verify signals are being collected
- Adjust context settings if classifications seem off
- Check the audit log to confirm redaction rules work as expected
