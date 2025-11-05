---
layout: default
title: Drag & Drop Org Chart Support and Help
---

# 🧭 Drag & Drop Org Chart — Support & Help

Thank you for your interest in the **Drag & Drop Organization Chart** by **WorkforceVision Ltd**.

For any issues or technical queries, please contact:  
📧 [IraWatt@WorkforceVision.co.uk](mailto:IraWatt@WorkforceVision.co.uk)

For details on how data is handled and secured, please see our  
➡️ [Data Security and Flow Overview](./data-security)

---

## 🏢 Overview

Effective strategic workforce planning and organizational design are crucial for any business.  
This visualization helps you achieve these goals by offering:

### 🔑 Key Features
1. **Comprehensive Visualization** – Visualize thousands of employees in a single chart with clear hierarchy and structure.  
2. **Customizable Node Colors** – Match your company branding or highlight departments and roles.  
3. **Enhanced Node Details** – Display roles, contact info, and performance data directly on each node.  
4. **Interactive Drag & Drop** – Restructure teams dynamically using drag-and-drop functionality.  
5. **Built-In Search** – Quickly locate any employee by name for faster navigation.

---

## 🎯 Common Use Cases
- **Organizational Restructures** – Plan and visualize changes clearly before implementation.  
- **Mergers & Acquisitions** – Align organizational structures and identify overlaps.  
- **Efficiency Reviews** – Identify bottlenecks, duplicate roles, and workload imbalances.

Use this tool to **optimize design, support strategic workforce planning, and drive business growth.**

---

## 🧾 Data Field Reference

| Field | Description |
|--------|--------------|
| **Employee ID** | Unique identifier for each employee (text or number). Must be unique. |
| **Manager ID** | ID of the employee’s manager. Leave blank for top-level nodes. Cannot be the same as Employee ID. |
| **Name** | Employee name (text). |
| **Job Title** | Employee title or position. |
| **Colour** | Optional hex code or named colour (e.g. `#FF5733` or `red`). Defaults to grey. |
| **Extra Details** | Additional information (e.g. location, start date). Multiple lines supported. |
| **Picture** | Base64-encoded image string (up to 32,766 characters). |
| **Secondary / Cumulative Field** | Numeric value aggregated across subordinate nodes, displayed top-left on each card. |

---

## 🔗 Dotted Line Reporting (JSON Format)

Use the **Dotted Line** field to define non-hierarchical relationships between employees.

Example JSON:
```json
[{ "ManagerID": "E1", "Comment": "Replacing", "Color": "#FF0000" }]


For multiple connections:

[
  { "ManagerID": "E173", "Comment": "Applied for Role" },
  { "ManagerID": "E63", "Comment": "Mentorship Link" }
]
```

🧠 Each object represents one dotted-line connection.
Properties: ManagerID, Comment, and Color (all as text).

Validate JSON using an online tool such as JSONLint
 or JSONChecker
.

🖼 Base64 Image Guidelines

Images must be Base64-encoded text. To ensure correct rendering:

Remove the prefix data:image/png;base64, before pasting.

Use UTF-8 encoding and plain text (no special characters).

Validate using Base64 Image Decoder
.

Valid Base64 characters: A–Z, a–z, 0–9, +, /, and = (padding).
If the image does not render, it’s likely malformed or too large.

🔀 Moving Nodes

Click Organize at the top of the visual.

Left-click and drag a node to a new position.

The entire reporting line moves with it.

Use Undo / Redo to revert or repeat changes.

Click Cancel to discard pending edits.

➕ Adding / ➖ Removing Nodes

Click Add or Remove at the top of the visual.

Complete the form and click Submit.

If Submit doesn’t work:

The Employee ID may already exist, or

The Manager ID provided doesn’t exist.

Removing a node deletes its reporting line — move subordinates first if needed.

🔎 Searching

Use the Search Bar to locate employees by name.
Partial matches are supported.
If there are 10 or fewer possible matches, they’ll be listed automatically.

🎨 Conditional Formatting

Conditional formatting enables dynamic color updates based on numeric comparisons between two Cumulative Fields.

When enabled:

Above Comparison Color → First value is higher.

Below Comparison Color → First value is lower.

Equal Comparison Color → Values are equal or within the threshold percentage.

To configure:

Enable Conditional Formatting in the visual’s formatting settings.

Define your threshold percentage.

Refresh or move nodes — formatting will update automatically.

🧾 Exporting as PDF

To export the chart:

Press CTRL + P

Choose Print as PDF

Select Landscape orientation

Click More Settings → Tick “Background Graphics”

Click Save / Print
