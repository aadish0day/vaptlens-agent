# VAPTLens Agent Dataset

This directory contains the ground truth datasets, scan outputs, and evaluation corpora for the VAPTLens Agent project.

## Directory Structure

```
dataset/
├── raw/                 # Raw scanner outputs (CSV, XML, JSON) from authorized target runs
│   ├── zap/             # OWASP ZAP scan outputs
│   ├── nuclei/          # Nuclei templates scan outputs
│   ├── openvas/         # OpenVAS / Greenbone XML/CSV reports
│   └── nessus/          # Nessus Essentials scan exports
├── labeled/             # Labeled ground-truth JSON files with expert verification
└── evaluation/          # Benchmark results comparing prompt engineering strategies
```

## Schema for Labeled Ground Truth (`labeled/findings.json`)

Each entry in the labeled evaluation dataset adheres to the following structure:

```json
{
  "finding_id": "GT-001",
  "source_scanner": "nuclei",
  "raw_title": "Apache Log4j RCE (CVE-2021-44228)",
  "verified_cve_ids": ["CVE-2021-44228"],
  "ground_truth_severity": "CRITICAL",
  "ground_truth_cvss": 10.0,
  "is_true_positive": true,
  "asset_context": {
    "is_public_facing": true,
    "contains_sensitive_data": true
  },
  "verified_remediation_summary": "Update log4j-core to 2.17.1 or higher, or set log4j2.formatMsgNoLookups=true.",
  "manager_signoff": {
    "signed": true,
    "reviewer": "Ateion Security Lead",
    "date": "2026-11"
  }
}
```

## Authorized Targets
Scans must strictly be performed against systems with explicit written authorization:
1. Local Dockerized homelab instances (OWASP Juice Shop, DVWA, Metasploitable).
2. Ateion staging environment (with written management sign-off).
