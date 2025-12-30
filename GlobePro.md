---
layout: default
title: Globe Pro — Support & Help
---

# 🧭 Globe Pro — Support & Help

Thank you for your interest in the **Globe Pro** visual by **WorkforceVision Ltd**.

Download and try the visual with our sample Power BI report directly from Microsoft AppSource:  
🔗 [Globe Pro - AppSource](https://marketplace.microsoft.com/en-us/product/power-bi-visuals/workforcevision.globepro?tab=Overview)

For any issues or technical, feature requests, or Licensing queries, please contact:  
📧 [IraWatt@WorkforceVision.co.uk](mailto:IraWatt@WorkforceVision.co.uk)

For details on how data is handled and secured, please see our  
➡️ [Data Security and Flow Overview](./data-security)


Licensing:

A license is required to use this visual. All users, both viewers and developers, need a valid license to access any features within the chart in Power BI Desktop or Power BI Service. Please see our FAQ page here: [Licenses](./Licenses).

You can explore the full functionality by downloading the sample report. Alternatively, you can begin a 1-month free trial by purchasing a license and cancelling before the trial period ends. This requires permission to assign and manage licenses in your Microsoft Admin Portal.

Once purchased a Licenses needs to be assigned to individuals or groups in your organisation in order for them to use the visual. Licenses can be moved from one user to anouther without issue. Full instructions are available here: [Microsoft License Management Guide](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/assign-licenses-to-users?view=o365-worldwide).

---
## 🧾 Data Field Reference

| Field name     | Data type                | GIS meaning        | Description / how to populate |
|----------------|--------------------------|--------------------|--------------------------------|
| Lat            | Decimal number           | Y coordinate       | Latitude of the point in decimal degrees, using WGS84 (EPSG:4326). Must be entered as latitude (Y) second. |
| Lon            | Decimal number           | X coordinate       | Longitude of the point in decimal degrees, using WGS84 (EPSG:4326). Must be entered as longitude (X) first. |
| value          | Number                   | NA                 | Height or altitude of a point when creating a radial bar from a point. |
| Lat end        | Decimal number           | Y coordinate       | Latitude of the end point of an arc in decimal degrees, using WGS84 (EPSG:4326). Must be entered as latitude (Y) second. |
| Lon end        | Decimal number           | X coordinate       | Longitude of the end point of an arc in decimal degrees, using WGS84 (EPSG:4326). Must be entered as longitude (X) first. |
| Colour         | Text (string)            | NA                 | Optional hex code (e.g. `#FF5733`). Defaults to `#ffffff`. |
| Label          | Text (string)            | NA                 | Text which will appear alongside a point on the map. |
| Id             | Text (string)            | Feature ID         | Unique identifier for the linear feature (e.g. cable, route, path). All rows with the same Id belong to the same feature. Equivalent to a feature’s id or primary key in GIS data. |
| SegmentIndex   | Whole number (integer)   | LineString index   | Identifies the line segment within the feature. Equivalent to individual LineString parts inside a MultiLineString. Use 0 for the first segment, 1 for the second, etc. |
| PointIndex     | Whole number (integer)   | Vertex order       | Defines the order of vertices along a segment. Equivalent to the implicit point order in a LineString. Points must be sequential (0, 1, 2, …) for the line to draw correctly. |


