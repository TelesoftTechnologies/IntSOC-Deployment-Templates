# IntSOC Deployment Templates Library

A comprehensive collection of machine learning deployment templates for threat hunting, anomaly detection, and network security analysis with IntSOC.

## Overview

This repository contains pre-configured JSON templates for rapid deployment of ML models in Telesoft's IntSOC platform. Templates are designed for common threat hunting scenarios including DGA detection, data exfiltration monitoring, lateral movement detection, and more.

**Quick Start:** Download a template, customize it for your environment, and load it directly into IntSOC's ML Deployment modal using the "From Template" feature.

## Features

✅ **Pre-built Threat Hunting Templates**  
✅ **Multiple Deployment Types** (Anomaly Detection, DGA, Typosquat, Custom PyTorch, Capacity Planning, FlowTime, MAC Tracker and more)  
✅ **Multi-filter Support** for complex detection rules  
✅ **Community Contributed** templates and best practices  
✅ **Easy Customization** with clear JSON schemas  

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/TelesoftTechnologies/IntSOC-Deployment-Templates.git
   cd IntSOC-Deployment-Templates
   ```

2. **Load into IntSOC:**
   - Navigate to **Settings > Machine Learning > Add Deployment**
   - Click the **"From Template"** button
   - Select your JSON template file
   - Customize fields as needed and click **"Add"**

## Template Structure

Each template is a JSON file with the following schema:

```json
{
  "name": "Template Display Name",
  "type": "deployment_type",
  "sourceField": "field_identifier",
  "sourceMetric": "metric_type",
  "filters": [
    {
      "type": "filter_type",
      "value": "filter_value"
    }
  ],
  "THRESHOLD": "threshold_value",
  "whitelist": "whitelist_file",
  "config": "{optional_config_json}"
}
```

### Supported Deployment Types

| Type | Use Case | Required Fields |
|------|----------|-----------------|
| `anomaly` | Behavioral anomaly detection | `sourceField`, `sourceMetric` |
| `dga` | Domain Generation Algorithm detection | `sourceField`, `THRESHOLD` |
| `typosquat` | Brand impersonation & typo domain detection | `sourceField`, `THRESHOLD` |
| `capacity-planning` | Network capacity forecasting | `sourceField`, `sourceMetric` |
| `flowtime` | Flow timing analysis | `THRESHOLD_SCALE` |
| `mactracker` | MAC address tracking & anomalies | `RAMPUP_MINS` |
| `custom` | Custom PyTorch/Tensorflow/Scikit-Learn models | `userFolder`, `sourceField`, `sourceMetric` |

### Source Metrics

- **`flow`** - Number of flows
- **`packet`** - Packet count
- **`octet`** - Bytes transferred
- **`value`** - Generic metric value

### Filter Types

- **`"range"`** - Range filter (e.g., `"100-1000"`)
- **`"equal"`** - Exact match filter

## Usage Tips

### Applying Multiple Filters

Chain filters together for sophisticated detection rules:

```json
{
  "name": "Advanced Threat Detection",
  "type": "anomaly",
  "sourceField": "total.bitRate",
  "sourceMetric": "flow",
  "filters": [
    {
      "type": "range",
      "value": "5000-50000"
    },
    {
      "type": "equal",
      "value": "outbound"
    },
    {
      "type": "range",
      "value": "443"
    }
  ]
}
```

### Customizing Thresholds

Adjust sensitivity based on your environment:
- **DGA/Typosquat:** `THRESHOLD` (70-95, higher = stricter)
- **FlowTime:** `THRESHOLD_SCALE` (50-100)
- **MAC Tracker:** `RAMPUP_MINS` (10-60, ramp-up period)

## Contributing

We welcome community contributions! To add a template:

1. **Fork the repository**
2. **Create a new template** in the appropriate category folder
3. **Follow the naming convention:** `descriptive-name-lowercase.json`
4. **Include documentation** describing:
   - Threat type detected
   - Use cases
   - Recommended thresholds
   - Example scenarios
5. **Test the template** in IntSOC
6. **Submit a pull request** with a clear description

## Validation

Validate your JSON template before submitting:

```bash
# Using jq
jq . your-template.json

# Using Python
python -m json.tool your-template.json
```

## IntSOC Compatibility

- **IntSOC Version:** 1.0+
- **Feature:** "From Template" button in ML Deployment modal
- **Supported Platforms:** Web UI

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

Built for the Telesoft IntSOC Security Operations Center platform. Thanks to all contributors who have shared templates!

---

**Questions?** Open an issue or start a discussion in the repository.
