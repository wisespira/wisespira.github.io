---
layout: default
title: Family Tree Visual – Support & Help
---

# 🌳 Family Tree Visual — Support & Help

Thank you for your interest in the **Family Tree Chart** by **WorkforceVision Ltd**.  
For any issues or assistance, please contact us via email:  
📧 [IraWatt@WorkforceVision.co.uk](mailto:IraWatt@WorkforceVision.co.uk)

---

## 🧩 Product Description

The **Family Tree Visual** is an interactive Power BI custom visual designed to make exploring relationships across generations simple and engaging.

It allows users to **create and navigate family trees (genealogy charts)** directly within Power BI.

### 🎯 Ideal Use Cases:
- **Historical Research:** Museums or history departments visualizing the lineage of kings, queens, and dynasties.  
- **Genealogical Studies:** Individuals or genealogists mapping family and friend networks.  
- **Education & Science:** Perfect for visualizing genetic inheritance or kinship structures in biology or anthropology.

With features such as **interactive zooming** and **click-through navigation**, the visual provides a clear, intuitive way to understand complex relationships — making it an excellent tool for **storytelling, research, and education**.

---

## 📊 Data Requirements

To ensure your data loads correctly, please follow these rules:

1. All fields must be in **text format**, even if they contain numbers.  
2. Each **node** in the tree must have its own **row**.  
   - For example, if node `A` has child `B`, then `B` must also appear as a separate row.  
3. The **ID** field must always be populated.  
4. When listing **Spouse IDs** or **Children IDs**, separate each ID with a comma.  

🧠 *An example data table is included in the Power BI report for reference.*

---

## 🔍 Interactivity & Functionality

- When opening the visual, the **first row** in your dataset will be the **node in focus**.  
- Selecting a node displays:
  - Its **parents** (and grandparents)  
  - Its **spouses and children** (and their children)
- Clicking another node **re-centres the chart** on that node.  
- Selecting a node can also **filter other visuals** in your report, based on that node’s data row.  

You can toggle this interactive filtering **on or off** in Power BI:  
> **Modeling → Manage Relationships**

---

## 🧱 Building the Visual

The Family Tree Visual includes the following fields:

| Field | Description |
|--------|--------------|
| **ID** | Unique identifier for each node (text). Must be unique. |
| **Name** | The name of the person or entity (text). |
| **Mother ID** | The ID of the node’s mother (text). |
| **Father ID** | The ID of the node’s father (text). |
| **Spouses ID** | One or more spouse IDs, separated by commas. |
| **Children ID** | One or more child IDs, separated by commas. |
| **Image** | Base64-encoded image string (up to 32,766 characters). See [how to create 64bit text from a image file or folder of images](./64-Bit-Image-Creation-From-File-or-Folder.md) and how to
• [compress image text to meet Power BI size requirements](./Compressing-64-Bit-Image.md) |
| **Colour** | Optional hex code (e.g. `#FF5733`) or colour name for the node’s top band. Defaults to grey. |

For more details on colour codes, visit [W3Schools Colors HEX](https://www.w3schools.com/colors/colors_hexadecimal.asp).

Each row in your data table corresponds to one node in the visual.

---

## 📬 Contact

We welcome feedback, ideas, and questions.  
You can reach us at:  
📧 [IraWatt@WorkforceVision.co.uk](mailto:IraWatt@WorkforceVision.co.uk)

