# Error Processing

The Error Processing endpoints can be used to retrieve errors and process outputs.

## GetHistoryForProcess
This endpoint retrieves the last 10 runs of the specified process.

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">GET</i>
		<h6>https://{{API_Url}}/GetHistoryForProcess</h6>
	</div>
</div>

> To retrieve a list of process runs, use this code:

```shell
curl GET "{{API Url}}/GetHistoryForProcess?token={{Token}}&processId={{processId}}" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Accept: application/json' 
```

```python
import requests

url = "{{API Url}}/GetHistoryForProcess?token={{Token}}&processId={{processId}}"
headers = {
  'Content-Type': 'application/x-www-form-urlencoded',
  'Accept': 'application/json'
}

response = requests.request("GET", url, headers=headers)

print(response.text.encode('utf8'))
```

> The above command returns JSON structured like this:

```json
[
    {
        "Id": "0f8b020d-000c-4e5e-a326-ac3000f57a11",
        "MapId": "BULK SHOPIFY COLLECTIONS TO BC",
        "RunNumber": 2,
        "RunDate": "2020-09-08T14:53:44",
        "RecordCount": 1,
        "ErrorCount": 0,
        "ErrorsSaved": false
    },
    {
        "Id": "7e6ca65a-db72-44e4-883d-ac3000f51ca5",
        "MapId": "BULK SHOPIFY COLLECTIONS TO BC",
        "RunNumber": 1,
        "RunDate": "2020-09-08T14:52:25",
        "RecordCount": 1,
        "ErrorCount": 1,
        "ErrorsSaved": true
    }
]
```
### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | Token generated by the GetToken endpoint
processId | true | Unique id for Process

<aside class="warning">Currently this endpoint only returns the most recent 10 process runs</aside>

## GetErrorsForProcessRun
This endpoint retrieves the error messages from a specified process run.

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">GET</i>
		<h6>https://{{API_Url}}/GetErrorsForProcessRun</h6>
	</div>
</div>

> To retrieve a list of process errors for a prpcess run, use this code:

```shell
curl GET "{{API Url}}/GetErrorsForProcessRun?token={{Token}}&processId={{processId}}&runNumber={{runNumber}}" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Accept: application/json' 
```

```python
import requests

url = "{{API Url}}/GetErrorsForProcessRun?token={{Token}}&processId={{processId}}&runNumber={{runNumber}}"
headers = {
  'Content-Type': 'application/x-www-form-urlencoded',
  'Accept': 'application/json'
}

response = requests.request("GET", url, headers=headers)

print(response.text.encode('utf8'))
```

> The above command returns JSON structured like this:

```json
{
    "mapKey": "f6e80c0c-d250-47cb-a8db-aac700a55347",
    "mapId": "DEMO CRM TEST",
    "mapDescription": "Demo CRM Test",
    "runNumber": 147,
    "runTenants": [
        {
            "errorId": "6f0795b7-97a8-48cf-9ec0-ab98016fc798",
            "tenantServer": "crm4.dynamics.com",
            "tenantTechnical": "eoned365demo",
            "errorCount": 2,
            "errorData": {
                "currentPage": 1,
                "pageSize": 1000,
                "recordCount": 2,
                "columnList": [],
                "dataRows": [
                    {
                        "Name": "Demo1",
                        "email": "Demo1@eone.com",
                        "Title": "Developer",
                        "Phone": "701-456-7283",
                        "EONERUNIDENT": "8b796357-a224-4627-b782-7846b0178f60",
                        "EONERUNERRORS": "1 duplicates found. \r\nFor entity 'contact' where 'emailaddress1 = Demo1@eone.com,' in organization 'eoned365demo'\r\n\r\n"
                    },
                    {
                        "Name": "Demo10",
                        "email": "Demo10@eone.com",
                        "Title": "Developer",
                        "Phone": "701-456-7292",
                        "EONERUNIDENT": "d1d62875-bec9-46c6-805e-8dbbbf1e5729",
                        "EONERUNERRORS": "1 duplicates found. \r\nFor entity 'contact' where 'emailaddress1 = Demo10@eone.com,' in organization 'eoned365demo'\r\n\r\n"
                    }
                ]
            }
        }
    ]
}
```
### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | Token generated by the GetToken endpoint
processId | true | Unique id for Process
runNumber | true | Run number to retrieve

<aside class="warning">Currently this endpoint is limited to show the top 1000 errors</aside>

## GetOutputsForProcess
This endpoint generates a link to download a process output.

<aside class="notice">
If the option to "Export Processing Errors as" is set to "Save as CSV/XML" on the process Options tab, then any errors created by the process can be downloaded with this method.
</aside>

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">GET</i>
		<h6>https://{{API_Url}}/GetOutputsForProcess</h6>
	</div>
</div>

> To retrieve a list of process outputs, use this code:

```shell
curl GET "{{API Url}}/GetOutputsForProcess?token={{Token}}&processId={{processId}}" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Accept: application/json' 
```

```python
import requests

url = "{{API Url}}/GetOutputsForProcess?token={{Token}}&processId={{processId}}"
headers = {
  'Content-Type': 'application/x-www-form-urlencoded',
  'Accept': 'application/json'
}

response = requests.request("GET", url, headers=headers)

print(response.text.encode('utf8'))
```

> The above command returns JSON structured like this:

```json
[
    {
        "outputId": "7aef7d77-4427-4b13-86a2-ac1e00c44688",
        "processId": "728b5348-66f4-40fb-b56f-ac1e00c22ed9",
        "runNumber": 3,
        "created": "2020-08-21T11:54:37",
        "description": "BULK BC REFUNDS TO MAGENTO output, run number 3",
        "fileName": "BULK BC REFUNDS TO MAGENTO___3.zip",
        "outputUri": "http://smartconnectschedule.azurewebsites.net/api/getfile?token={{Token}}&key={{key}}"
    }
]
```
### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | Token generated by the GetToken endpoint
processId | true | Unique id for Process
runNumber | false | Run number to retrieve