Building an Organization Chart in Power BI Using Microsoft Graph API (Microsoft Entra ID)

Microsoft Entra ID (formerly Azure Active Directory) stores rich information about your users, including names, job titles, departments, managers, and profile photos. By connecting Power BI to the Microsoft Graph API, you can automatically populate the WorkforceVision Org Chart visual with real employee data—without maintaining a manual dataset.

This guide walks through the full setup required to pull user details and profile images from Microsoft Entra ID into Power BI using Power Query, and then prepare the data for use in the Org Chart visual.

Overview of the Process

To build your Microsoft Entra ID–driven Org Chart, you will complete the following steps:

Prerequisites and Required Azure Setup

Create Parameters in Power Query

Retrieve Entra ID User Data (via Microsoft Graph API)

Retrieve Profile Images

Load Into Power BI and Connect to the Org Chart Visual

1. Prerequisites

Before connecting Power BI to Microsoft Entra ID, a few Azure components must be in place:

Microsoft Entra ID Tenant ID

Identifies your Azure tenant. It is required for all Graph API authentication calls.

Registered Application in Entra ID

You must create an App Registration that Power BI will use to request data from the Microsoft Graph API.
This application needs delegated or application permissions depending on your configuration.
Refer to Microsoft’s Graph API permissions documentation if you need to verify the correct scopes.

Application Credentials

Client ID – The unique ID of your registered app

Client Secret – A secure key generated for API authentication

These values are used inside Power BI to request an access token from Microsoft Entra ID.

2. Create Parameters in Power Query

To keep credentials secure and reusable, define three parameters:

Tenant ID

Client ID

Client Secret

In Power Query:

Go to Home → Manage Parameters → New Parameter

Create the three parameters and paste in your Azure values.

Save your changes.

Image: Manage Parameters
Image: Add Parameters

These parameters will be referenced throughout your Power Query scripts.

3. Retrieve Entra ID User Data (Graph API)

Create a new blank query named AAD Users, then open Advanced Editor.
Replace its contents with the following M code:

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
