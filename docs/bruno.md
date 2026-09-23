---
layout: page
title: Getting Started
hide:
   #- navigation
   - toc
---

## Getting started with the Bruno collection

You can be up and running in a few minutes: download the archive for your release, open it in **Bruno**, select an environment, and set the variables for your UEM tenant.

### Open the collection in Bruno

1. Download the **Bruno ZIP** for the UEM release you use (see the [REST API reference by release](index.md#rest-api-reference-by-release) table).
2. Extract the archive, then in Bruno choose **Open Collection** and select the collection folder (the one that contains `bruno.json` if present), or open the folder you use for that release.
   <img src="../images/open-collection.png" alt="Opening collection in Bruno" style="display:block; margin-top:0.5rem;" />
3. In the **Environments** list, select the environment that matches your use case (for example a local or example environment). The collection ships with **variable placeholders** for OAuth and API host values.
   <img src="../images/select-environment.png" alt="Selecting development environment" style="display:block; margin-top:0.5rem;" />
4. On the upper right corner of the Bruno UI, select your environment and press configure. You can see a few variables that will be used by the API collection.
   <img src="../images/configure-env-variables.png" alt="Configuring environment variables" style="display:block; margin-top:0.5rem;" />

### Collect Environment Variable Values
Before making the first API call, you must collect these five items:

- REST API URL
- Oauth token URL
- Oauth client ID
- Oauth client secret

#### REST API URL

To get these first two values, follow these steps:

1. In the Workspace ONE UEM admin console, navigate to **Groups and Settings** > **All Settings** > **System** > **Advanced** > **API** > **Rest API**. Make sure you're in Customer OG or below.
2. Copy the hostname part of the REST API URL (e.g. localhost).  
   <img src="../images/rest-api-url.png" alt="REST API URL settings" style="display:block; margin-top:0.5rem;" />
3. Back in Bruno, add this value in the **environment** for the collection (variable names may match `YOUR_API_SERVER` or similar—use the names your Bruno environment defines):
   1. YOUR_API_SERVER
   <img src="../images/add-env-variables.png" alt="Adding aw-tenant-code and baseUrl variables" style="display:block; margin-top:0.5rem;" />

#### OAuth token URL

See the [Datacenter and Token URLs](https://docs.omnissa.com/bundle/WorkspaceONE-UEM-Console-BasicsVSaaS/page/UsingUEMFunctionalityWithRESTAPI.html#datacenter_and_token_urls_for_oauth_20_support) KB and copy the Token URL for your region.

**Note:** You might also want to create a dedicated REST API admin role for your environment, which is described in that same article. The example in this brief guide does not cover a dedicated REST API admin role, and uses the console admin role instead.

#### OAuth client ID and client secret

In Workspace ONE UEM admin console, follow these steps.

1. Navigate to **Group & Settings** > **Configurations** > **OAUTH client management** and click **Add**.
   <img src="../76200-1119-175655-9.png" alt="OAUTH client management" style="display:block; margin-top:0.5rem;" />
2. Copy the client ID and secret to a text file.
   <img src="../76200-1119-175655-10.png" alt="Copy client ID" style="display:block; margin-top:0.5rem;" />
   <img src="../76200-1119-175655-11.png" alt="Copy client secret" style="display:block; margin-top:0.5rem;" />
3. Add these to the Auth section of the Collection.
   <img src="../images/configure-auth.png" alt="Configuring OAuth2" style="display:block; margin-top:0.5rem;" />
4. To obtain an access token, scroll to the bottom and click **Get Access Token**.
   <img src="../images/get-access-token-successful.png" alt="Get Access Token successful" style="display:block; margin-top:0.5rem;" />

Now that you have successfully authenticated to Workspace ONE UEM, you have unlocked the access to all the API calls included in the collection.

### Real-World Example: Find Devices and Apply Tag

Let's see how to achieve this simple goal without touching the UEM console: find Windows devices for user Wannes and apply a tag to those devices.

First, start with a request that returns the devices you are looking for. The API call `{{ baseUrl }}/mdm/devices/search` sounds like the correct one.

To find out the purpose of a specific API call, see the documentation section. You will find a short description and additional details for mandatory and optional parameters.
<img src="../images/where-to-find-api-documentation.png" alt="Where to find documentation" style="display:block; margin-bottom:0.5rem;" />
<img src="../images/device-search-example.png" alt="Successful search example" style="display:block;" />

1. Copy the values of the `LocationGroupId` and device `Id` (located near the end) attributes in the response body, you will use these values later.
2. The next step would be to obtain the available tags from UEM. This requires the following API: `{{baseUrl}}/system/groups/:id/tags`, which is located under System API V1 > Tags.
   In the documentation section of this API, the PATH variable mentioned is mandatory, so the Organization Group ID value obtained with the previous call needs to be added to the Params tab.  
   <img src="../images/get-tags-docs.png" alt="Documentation for get tags from OG api" style="display:block; margin-top:0.5rem;" />
   <img src="../images/get-tags-based-on-og-example.png" alt="Get tag based on OG example" style="display:block; margin-top:0.5rem;" />

3. In the response body, you will find the available tags for the given Organization Group. Note down the Id value of the tag, you will need it for the final API call to assign a tag.
   Lastly, execute the API call that applies the selected tag to the device, using `{{baseUrl}}/mdm/tags/:tagid/adddevices`, located in MDM API V1 > Tags.
4. As shown in Bruno, the tag ID is a mandatory value. Enter the tag ID from the previous call in the request **Params** (or path variables).  
   <img src="../images/add-devices-to-tag-docs.png" alt="Add devices to tag docs" style="display:block; margin-top:0.5rem;" />
5. In the Body tab, add the device ID obtained with the first API call.  
   <img src="../images/add-devices-to-tag-example.png" alt="Add device ID to tag example" style="display:block; margin-top:0.5rem;" />
6. Click send and check the UEM console for the result.  
   <img src="../images/add-devices-successful.png" alt="UEM console with added tag shown" style="display:block; margin-top:0.5rem;" />

### Error Handling

In case of an error, review **response** output and any **script / console** output Bruno shows for the request.

![Errors and Dev tools view](images/errors-and-dev-tool.png)

### Additional Considerations

Now that you know which API calls to use to reach our goal, you can proceed to automate the process if needed. Bruno can **generate code snippets** for requests in several languages from the request view, where supported.

![Export api](images/export-api.png)

Below is a real-world example of a script built for a customer. In short, it does the following things:

1. Retrieves all available tags in a specific Organization Group.
2. Retrieves all devices enrolled in the last X number of days.
3. Checks the first five characters of the Asset Number that the user entered during enrollment.
4. If it can find a tag that matches these 5 characters, that tag is assigned. If no match is found, a fallback tag is assigned.

These tasks that are not natively available in the Workspace ONE UEM console, but can easily be tackled using the REST APIs described here (client examples below use PowerShell; adapt as needed for your toolchain).

```js
################################################################################## Parameters

#################################################################################

# Define the OAuth endpoint, client ID, and client secret

$tokenUrl = "<provide Token URL>"

$clientId = "<provide Oauth Client ID>"

$clientSecret = "<provide Oauth Client Secret>"

$api_server_url = "<provide API Server hostname (asXXXX)>"

$aw_tenant_code = "<provide API key>"

$LocationGroupID = "<LG_ID>"

$numberOfDays = 1

 

#################################################################################

# request token

#################################################################################

# Define the OAuth request parameters

$body = @{

    grant_type    = "client_credentials"

    client_id     = $clientId

    client_secret = $clientSecret

}

 

# Make the OAuth token request

$oauth_token = Invoke-RestMethod -Uri $tokenUrl -Method Post -ContentType "application/x-www-form-urlencoded" -Body $body

 

#################################################################################

# Get available tags in given Organization group

#################################################################################

$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"

$headers.Add("Accept", "application/json;version=1")

$headers.Add("Authorization", "Bearer $($oauth_token.access_token)")

 

$availableTags = Invoke-RestMethod "https:// $api_server_url.awmdm.com/api/system/groups/$LocationGroupID/tags" -Method 'GET' -Headers $headers

 

# Store tags in a dictionary for fast lookup

$tagDict = @{}

foreach ($tag in $availableTags.Tags) {

    $tagDict[$tag.TagName] = $tag

}

 

# Add the special tag for empty or invalid asset numbers

$emptyOrInvalidTagName = "emptyorinvalidassetnumber"

$emptyOrInvalidTagID = $tagDict[$emptyOrInvalidTagName].Id

 

#################################################################################

# get devices enrolled in the last X days

#################################################################################

$startDate = (Get-Date).AddDays(-$numberOfDays).ToString("yyyy/MM/dd")

 

$recentlyEnrolled = Invoke-RestMethod "https:// $api_server_url.awmdm.com/api/system/users/enrolleddevices/search?enrolledsince=$startDate" -Method 'GET' -Headers $headers

 

#################################################################################

# Match AssetNumber (first 5 characters) with TagName and assign tag

#################################################################################

 

foreach ($device in $recentlyEnrolled.EnrolledDeviceInfoList) {

    $tagID = $null

    if (![string]::IsNullOrEmpty($device.AssetNumber) -and $device.AssetNumber.Length -ge 5) {

        $assetNumberPrefix = $device.AssetNumber.Substring(0, 5)

        if ($tagDict.ContainsKey($assetNumberPrefix)) {

            $tagID = $tagDict[$assetNumberPrefix].Id

        }

    }

   

    # If no matching tag found, use the emptyOrInvalidTagID

    if ($null -eq $tagID) {

        $tagID = $emptyOrInvalidTagID

    }

 

    $deviceID = $device.DeviceID

 

    $body = @{

        BulkValues = @{

            Value = @($deviceID)

        }

    } | ConvertTo-Json -Compress

 

    $response = Invoke-RestMethod "https:// $api_server_url.awmdm.com/api/mdm/tags/$tagID/adddevices" -Method 'POST' -Headers $headers -Body $body

 

    # Output the response for each device

    $response | ConvertTo-Json

}
```
