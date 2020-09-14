# Authentication

SmartConnect requires a URL parameter with a token for authorization. A token will need to be requested from the server prior to calling any other endpoints.

<aside class="notice">
Tokens are company specific and are valid for 3 months. If your username has access to multiple companies, you will need to generate a separate token for each company.
</aside>

## GetToken

> To generate a token, use this code:

```shell
curl POST "{{API Url}}/GetToken?email={{Username}}&password={{EncryptedPassword}}&companyId={{Customer Id}}"
```

```python
import requests

url = "{{API Url}}/GetToken?email={{Username}}&password={{EncryptedPassword}}&companyId={{Customer Id}}"

response = requests.request("POST", url)

print(response.text.encode('utf8'))
```
The GetToken endpoint requires 3 parameters. To generate a token.

> The above command returns a string:

```json
"Um5UVHZsWU1XZ09xYzE1R3dOZXhNV0Jka25FcEg3L2ZtZjVMSkxIdEZsUT0jQjE2QkIzNTMtNTFFOC00MkNBLTgxRDEtNUVCQjA0QUIxMzQ5I0MzMzhDRUI1LTBEOTctNEU1Ny05MDU4LUNFN0NBRDNEODU2RCNFVEhBTi5TT1JFTlNPTkBFT05FU09MVVRJT05TLkNPTSMyMDIwLTEwLTAyVDA4OjA1OjE1Ljg2Ng=="
```

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">POST</i>
		<h6>https://{{API Url}}/GetToken</h6>
	</div>
</div>

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
email | true | The email address of a user with access to the SmartConnect tenant.
password | true | Password the user uses to access the SmartConnect.com application.
companyId | true | Customer id from the API Settings Page.

## Validate

> To validate stored token, use this code:

```shell
curl POST "{{API Url}}/validate?token={{Token}}"
```

```python
import requests

url = "{{API Url}}/validate?token={{Token}}"

response = requests.request("POST", url)

print(response.text.encode('utf8'))
```
The Validate endpoint will return a Boolean to inform the client whether the token is valid or not.

> The above command returns a boolean:

```json
true
```

### HTTP Request
`POST {{API Url}}/validate`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | Stored token from the GetToken endpoint.
