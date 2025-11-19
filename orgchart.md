---
layout: default
title: Drag & Drop Org Chart Support and Help
---

# 🧭 Drag & Drop Org Chart — Support & Help

Thank you for your interest in the **Drag & Drop Organization Chart** by **WorkforceVision Ltd**.

Download and try the visual with our sample Power BI report directly from Microsoft AppSource:  
🔗 [Org Chart Drag and Drop - AppSource](https://marketplace.microsoft.com/en-us/product/power-bi-visuals/workforcevision.org-chart-drag-and-drop?tab=Overview)

For any issues or technical queries, please contact:  
📧 [IraWatt@WorkforceVision.co.uk](mailto:IraWatt@WorkforceVision.co.uk)

For details on how data is handled and secured, please see our  
➡️ [Data Security and Flow Overview](./data-security)

Advanced Guides:  
• [Create Reporting Line Slicer/Filter Using Power Query (M)](./Reporting-Lines)  
• [Create a Supervisor/Team Slicer/Filter for the Org Chart Visual](./team-members)

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
🧠 **Each object represents one dotted-line connection.**  
Each object contains the following properties:

| Property | Description |
|-----------|--------------|
| **ManagerID** | The Employee ID the dotted line connects to. |
| **Comment** | Optional text to describe the relationship. |
| **Color** | Optional colour for the dotted line (e.g. `#FF0000`). |

Validate your JSON structure using online tools such as:  
- [JSONLint](https://jsonlint.com)  
- [JSONChecker](https://jsonchecker.com)

---

## 🖼 Base64 Image Guidelines

Images used in the Org Chart visual must be **Base64-encoded text**.

### ✅ To ensure correct rendering:
1. Remove the prefix `data:image/png;base64,` before pasting.
2. Use UTF-8 encoding and plain text (no special formatting).
3. Validate using [Base64 Image Decoder](https://www.base64decode.net/base64-image-decoder).

### 💡 Notes
- **Valid Base64 characters:** `A–Z`, `a–z`, `0–9`, `+`, `/`, and `=` (padding).  
- If the image does not render, it’s likely malformed or exceeds Power BI’s 32,766-character limit.  
- You can reduce file size by compressing or resizing the image before encoding.

---

## 🔀 Moving Nodes

You can reorganize nodes to reflect new reporting structures.

### Steps:
1. Click **Organize** at the top of the visual.  
2. **Left-click and drag** a node to a new position.  
3. The node and its entire reporting line will move together.  
4. Use **Undo / Redo** to revert or repeat changes.  
5. Click **Cancel** to discard unsaved changes.

---

## ➕ Adding / ➖ Removing Nodes

Easily update your organization’s structure using the built-in **Add** and **Remove** functions.

### To Add or Remove:
1. Click **Add** or **Remove** on the visual toolbar.  
2. Complete the form and click **Submit**.  
3. If **Submit** doesn’t work:
   - The **Employee ID** may already exist, or  
   - The **Manager ID** does not exist in the dataset.  
4. Removing a node also removes its entire reporting line.  
   - Move subordinates to a new manager first if you want to retain them.

---

## 🔎 Searching

Use the **Search Bar** to locate any employee by name.

- Supports **partial matches** (e.g. typing “Ann” finds “Anna” or “Annette”).  
- When there are **10 or fewer results**, they’ll be displayed automatically for easy selection.

---

## 🎨 Conditional Formatting

Conditional formatting dynamically updates node colors based on numeric comparisons between two **Cumulative Fields**.

### Behavior:
- **Above Comparison Color** → First value is higher.  
- **Below Comparison Color** → First value is lower.  
- **Equal Comparison Color** → Values are equal or within the defined threshold.

### To Configure:
1. Enable **Conditional Formatting** in the visual’s **Formatting** settings.  
2. Define your **Threshold Percentage**.  
3. Refresh or move nodes — formatting will automatically update.

---

## 🧾 Exporting as PDF

Easily export your chart for printing or sharing.

### Steps:
1. Press **CTRL + P**  
2. Choose **Print as PDF**  
3. Select **Landscape Orientation**  
4. Click **More Settings → Tick “Background Graphics”**  
5. Click **Save / Print**

Your exported PDF will include all node colors and backgrounds exactly as shown in Power BI.
