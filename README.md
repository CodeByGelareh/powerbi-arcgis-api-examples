# powerbi-arcgis-api-examples
A collection of Power Query samples and techniques for calling the ArcGIS REST API directly from Power BI.
This repository provides a secure and fully automated way to connect Power BI to a protected ArcGIS FeatureServer.
It is designed for datasets that require authentication (e.g., internal maps or restricted services) and for layers that enforce the standard 2,000-record limit per request.

Prerequisites

Before using this Power BI → ArcGIS connector, ensure the following:

You have permission to access the ArcGIS service
Your ArcGIS administrator must grant you access to the FeatureServer layer.

Your ArcGIS admin must create the OAuth application
In the ArcGIS Developer dashboard, an admin needs to create Developer Credentials for you.
This generates the following:

client_id

client_secret

(Optional) long-lived token, if required

Correct Redirect URL must be set
When creating the app, the admin must add this redirect URL:

https://oauth.powerbi.com/views/oauthredirect.html


This is the official redirect endpoint used by Power BI when authenticating with external OAuth services.

The solution includes:

Automatic OAuth token generation via AutoToken()

Secure storage of credentials using Power BI Parameters

Unlimited data retrieval using a paginated FeatureServer loader

Dynamic schema expansion (no hard-coded field lists)

GitHub-safe code with no sensitive information included

🔐 1. Set Up Parameters in Power BI

Before running the code, create three parameters inside Power BI:

ArcGIS_ClientId

Type: Text

Value: your ArcGIS app’s client ID

ArcGIS_ClientSecret

Type: Text

Value: your secret

Mark as Sensitive so it never appears in code

ArcGIS_FeatureServerURL

Type: Text

Value: the base URL of your FeatureServer layer, for example:

https://your-org.arcgis.com/.../FeatureServer/0/


These parameters make your connector secure and GitHub-safe.

🔄 2. Use AutoToken() to Generate an OAuth Token

Create a new blank query and paste the AutoToken() function from this repo.

This function automatically:

Sends your client ID + secret to ArcGIS

Retrieves a fresh access token

Returns it to your code

Works on every Power BI Refresh (Desktop & Service)

You never need to manually paste tokens again.

Usage example inside any query:

token = AutoToken(),

🌍 3. Use the Paginated Loader to Retrieve All Records

ArcGIS limits each query to 2,000 rows.
To bypass this, use the provided paginated loader query.

This query:

Calls the FeatureServer using your token

Retrieves rows in batches (2000 per request)

Continues requesting pages until empty

Combines all pages into one table

Expands fields automatically

To use it:

Create a new blank query

Paste the full paginated loader code

Rename the query (e.g., FeatureServerTable)

Press Refresh

You will now have a complete dataset from your ArcGIS layer, regardless of size.
