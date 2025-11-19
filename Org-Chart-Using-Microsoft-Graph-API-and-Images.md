# **Building an Organization Chart in Power BI Using Microsoft Graph API (Microsoft Entra ID)**

Microsoft Entra ID (formerly Azure Active Directory) provides detailed information about your users, including names, departments, managers, job titles, and profile photos. By connecting Power BI to the Microsoft Graph API, you can automatically populate the **WorkforceVision Org Chart** visual with live employee data—eliminating the need for manual datasets or spreadsheets.

This guide explains how to connect Power BI to Microsoft Entra ID using Power Query, retrieve user and image data, and prepare the dataset for the Org Chart visual.

---

## **📌 Overview of the Process**

To build a Microsoft Entra ID–powered Org Chart, complete the following stages:

1. **Prerequisites and Azure configuration**  
2. **Create parameters in Power Query**  
3. **Retrieve user details from Microsoft Graph**  
4. **Retrieve user profile images**  
5. **Load and connect data to the Org Chart visual**

---

## **1. Prerequisites**

Before connecting to the Microsoft Graph API, the following Azure items must be set up:

### **✔ Tenant ID**
Your Microsoft Entra ID tenant identifier, required for all authentication flows.

### **✔ App Registration in Entra ID**
Power BI will authenticate using an Azure App Registration.  
Ensure this app has the correct **Microsoft Graph API permissions** (delegated or application, depending on your configuration).

### **✔ Application Credentials**
- **Client ID** – Identifies your registered application  
- **Client Secret** – Secure key used to authenticate the application  

Power BI uses these values to request a Graph API access token.

---

## **2. Creating Parameters in Power Query**

To avoid hard-coding credentials, create parameters:

- **Tenant ID**  
- **Client ID**  
- **Client Secret**

### **Steps:**
1. In Power Query, go to **Home → Manage Parameters → New Parameter**  
2. Create each parameter and paste in your Azure details  
3. Click **OK** to save

> 💡 These parameters will be referenced by all Power Query scripts.

*Image: Manage Parameters*  
*Image: Add Parameters*

---

## **3. Retrieving Microsoft Entra ID User Data**

1. Create a new **Blank Query**  
2. Rename it to **AAD Users**  
3. Open **Advanced Editor** and paste the following code:

```M
let 
    resource = "https://graph.microsoft.com",
    tokenResponse =
        Json.Document(
            Web.Contents(
                "https://login.windows.net/",
                [
                    RelativePath = #"Tenant ID" & "/oauth2/token",
                    Content = Text.ToBinary(
                        Uri.BuildQueryString(
                            [
                                client_id = #"Client ID",
                                resource = resource,
                                grant_type = "client_credentials",
                                client_secret = #"Client Secret"
                            ]
                        )
                    ),
                    Headers = [Accept="application/json"],
                    ManualStatusHandling={400}
                ]
            )
        ),
    access_token = tokenResponse[access_token],
    Source = 
        OData.Feed(
            "https://graph.microsoft.com/v1.0/users",
            [Authorization = "Bearer " & access_token],
            [ExcludedFromCacheKey={"Authorization"}, ODataVersion=4, Implementation="2.0"]
        )
in
    Source
``` 

This request retrieves core user profile information such as names, job titles, and IDs.

Authentication and Privacy Settings

When prompted:

Set authentication to Anonymous
(because the token is already generated inside the query).

Set privacy levels for:

https://login.windows.net/

https://graph.microsoft.com/

to Organizational.

Image: Credentials
Image: Privacy Settings

Once applied, your first batch of Entra ID user data will appear in Power Query.

4. Retrieve User Profile Photos

Profile photos can also be retrieved using the Microsoft Graph API.
Create another blank query and open Advanced Editor.
Paste the following M code, then rename the query to AAD User Photos:
```M
let
    // Request Access Token (v2 endpoint)
    tokenResponse =
        Json.Document(
            Web.Contents(
                "https://login.microsoftonline.com/" & #"Tenant ID" & "/oauth2/v2.0/token",
                [
                    Content = Text.ToBinary(
                        Uri.BuildQueryString(
                            [
                                client_id = #"Client ID",
                                client_secret = #"Client Secret",
                                grant_type = "client_credentials",
                                scope = "https://graph.microsoft.com/.default"
                            ]
                        )
                    ),
                    Headers=[#"Content-Type"="application/x-www-form-urlencoded"]
                ]
            )
        ),
    accessToken = tokenResponse[access_token],

    // Retrieve User IDs
    userList =
        Json.Document(
            Web.Contents(
                "https://graph.microsoft.com/v1.0/users?$select=id&$top=999",
                [Headers=[Authorization="Bearer " & accessToken]]
            )
        )[value],

    userIds = List.Transform(userList, each _[id]),

    // Function to request a user's photo
    getPhoto = (userId as text) =>
        Web.Contents(
            "https://graph.microsoft.com/v1.0/users/" & userId & "/photo/$value",
            [Headers=[Authorization="Bearer " & accessToken]]
        ),

    // Fetch each photo
    photoRecords =
        Record.FromList(
            List.Transform(userIds, each getPhoto(_)),
            userIds
        )
in
    photoRecords
```

This query returns a list of photo blobs indexed by user ID.

Prepare the Photo Data

Convert the two output columns to Text.

Rename the columns to:

Id

Image

Add a new custom column to convert the raw Base64 into a proper image URL:

Employee Image = "data:image/png;base64," & [Image]


Remove the original Image column and keep the new Employee Image field.

Ensure the column type is set to Text.

Image: Custom Column Example

5. Load Data and Connect to the Org Chart Visual

Select Close & Apply to load both datasets into Power BI.

Finally:

Create a relationship between AAD Users and AAD User Photos on the Id field.

Use the following fields for the Org Chart visual:

Id

Manager Id (from AAD users)

Display Name

Job Title

Department

Employee Image (from the image table)

Your Power BI report will now automatically build a live, Microsoft Entra–driven WorkforceVision Org Chart.

If you’d like, I can also create:

✅ A simplified “quick start” version
✅ A separate troubleshooting section
✅ A polished GitHub Pages version with headings, images, and code blocks

Just let me know!
