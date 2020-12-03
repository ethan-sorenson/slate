# Authentication

SmartConnect requires the user email, password, and companyId to generate a token.
The password will need to be encrypted using RSA encrytion using [the PKCS #1 standard](https://en.wikipedia.org/wiki/PKCS_1).

<aside class="notice">
Tokens are company specific and are valid for 3 months. If your username has access to multiple companies, you will need to generate a separate token for each company.
</aside>

### Password Encryption

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>https://login.smartconnect.com/v1/public-key</h6>
	</div>
</div>

> To encrypt the password, use this code:

```shell
check other languages
```

```python
def get_encrypted_password(password):
    import xml.etree.ElementTree as ElementTree
    from Crypto.PublicKey.RSA import construct
    from Crypto.Cipher import PKCS1_v1_5
    from base64 import b64decode
    import base64
    import requests
    password = password.encode('utf-8')

    #get public key 
    response = requests.post('https://login.smartconnect.com/v1/public-key')
    key = response.text.replace('"','')
    root = ElementTree.fromstring(key)

    # decode base64 string to be used as modulus(n) and exponent(e) components
    modulus = b64decode(root[0].text)
    exponent = b64decode(root[1].text)

    n = int.from_bytes(modulus, byteorder='big')
    e = int.from_bytes(exponent, byteorder='big')

    pubkey = construct((n, e))
    pubkey = PKCS1_v1_5.new(pubkey)
    encrypted = pubkey.encrypt(password)

    # base64 encode the bytes
    return base64.b64encode(encrypted).decode()
```

```javascript
function encode_password() {
  var password = "mypassword";
  var n = "vYofWQ63vYJZTB/EW4NT5LMlfiB5LftMMQSbJgFHvfE+Z8AXnCLNplBByGLWBLMolSwJZjJdUN5gLF0V/Q9aVwOK5GWAezt8IPwMYqAgwQ2btnlhrsKLkKoTtogGj9MYz9briYn/DHZlW56aOYvwSoz1LYhnoja59cG2UvIXxVE=";
  var e = "AQAB";
  var rsa = forge.pki.rsa;
  var BigInteger = forge.jsbn.BigInteger;

  function parseBigInteger(b64) {
    return new BigInteger(forge.util.createBuffer(forge.util.decode64(b64)).toHex(), 16);
   }
  var pubKey = rsa.setPublicKey(parseBigInteger(n), parseBigInteger(e));
  var encryptedData = pubKey.encrypt(password, 'RSAES-PKCS1-V1_5');
  document.write('Encrypted Password: ' + btoa(encryptedData) +
  '<br>Unencrypted Password: ' + encodeURIComponent(btoa(encryptedData)));
}
```

```csharp
check other languages
```

> The above command returns a string:

```json
"Bqr+OK0r4f8gURRCt3RCmxfem2PF2lE5lMIZ2ZsOEu9rDmEk93wuFrFIRFPLX4Wm8HDq7HgNn/mj7TT18uQj1UpqFVoizQLs6TWgLLUdqjxqeTFamvJaAVUh392tsQDTgkHkU4UwB8MABay2lr987GJIUDd4MaC2Aj11t8XjaXCU="
```

## GetCompanies

Optionally you can retrieve a list of companies with the companies endpoint.

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>https://login.smartconnect.com/v1/companies</h6>
	</div>
</div>

> To generate a token, use this code:

```shell
curl POST "https://login.smartconnect.com/v1/companies"
  -H 'Accept: application/json' 
  -d '{
    "Email": "demo.user@eonesolutions.com",
    "Password":"Ua4r0AK8f5xuLeFGhmBtg0SP9cXYT+YUDAMbNKZ53VHAN3lQIoOTue4bIhAs+G2dPloF++5rOcDOmVhKaeAyrfQhIDw/1sHH0k90H2ubTGDCJ0zK3ewfXDXY2y3c8q6f7XShgVthACHNOpMV7BZXn2kDGQRR1WWHwJw4zBG+bGw=",
    "ClientName": "Python",
    "IpAddress": "127.0.0.1"
    }'
```

```python
import requests
import json
url = "https://login.smartconnect.com/v1/companies"

payload = json.dumps({"Email":{{email}},"Password":get_encrypted_password({{password}}),"ClientName":"Python","IpAddress":"127.0.0.1"})
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
response = requests.post(url, data = payload, headers = headers)
#Format and print response
print(json.dumps(json.loads(response.text), indent=4, sort_keys=True))
```

> The above command returns a string:

```json
{
    "UserId": "57c5d9c3-4af6-4e46-a553-44u62235a9ff",
    "Email": "demo.user@eonesolutions.com",
    "FirstName": "Demo",
    "LastName": "User",
    "Status": "Active",
    "ChangePasswordAtLogin": false,
    "EmailVerified": true,
    "Products": [
        {
            "ProductId": "8de68f74-9162-4973-84a3-43c8673d0d80",
            "Name": "SmartConnect",
            "ProductStatus": "Active",
            "Status": "Active",
            "Role": "Administrator",
            "Type": "User",
            "RegionId": "41360865-b555-419f-80bf-1484b2719e36",
            "RegionName": "North Central US",
            "AccountProductUserId": "1fc363a3-a01c-4398-bb85-aca78809a621",
            "AccountProductId": "fb60defa-6bae-4fa8-b164-c8b06f3fac3d",
            "Account": {
                "AccountId": "72b01219-a6a4-4e25-8d93-58d0584bf75f",
                "Name": "Test Demo Account",
                "EQA": false,
                "Status": "Active",
                "Currency": "USD"
            },
            "Subscription": {
                "AccountProductSubscriptionId": "652fa33c-d971-4ed7-a52f-3c8ca3ff1737",
                "Status": "Expired",
                "HasSupport": true,
                "AdditionalConnections": 0,
                "SubscriptionEndDate": "2020-11-20T00:00:00",
                "Plan": {
                    "SubscriptionPlanId": "8215427c-fab9-4c26-ae97-244b4d8512af",
                    "Name": "SmartConnect Professional",
                    "ConnectionCount": 6,
                    "HasSupport": true,
                    "PerpetualLicense": false,
                    "Status": "Active"
                },
                "ExpiredString": " - inactive subscription",
                "Expired": true
            },
            "Instances": [],
            "WebClientUrl": "appeqa.smartconnect.com",
            "ApiUrl": "smartconnect-na-scheduler.azurewebsites.net/API",
            "ConnectionCount": 6,
            "AccountProductStatusString": " - inactive subscription"
        }
    ]
}
```
## GetToken

The GetToken endpoint requires 3 parameters. To generate a token.

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>https://{{API_Url}}/GetToken</h6>
	</div>
</div>

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

> The above command returns a string:

```json
"Um5UVHZsWU1XZ09xYzE1R3dOZXhNV0Jka25FcEg3L2ZtZjVMSkxIdEZsUT0jQjE2QkIzNTMtNTFFOC00MkNBLTgxRDEtNUVCQjA0QUIxMzQ5I0MzMzhDRUI1LTBEOTctNEU1Ny05MDU4LUNFN0NBRDNEODU2RCNFVEhBTi5TT1JFTlNPTkBFT05FU09MVVRJT05TLkNPTSMyMDIwLTEwLTAyVDA4OjA1OjE1Ljg2Ng=="
```

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
email | true | The email address of a user with access to the SmartConnect tenant.
password | true | Password the user uses to access the SmartConnect.com application.
companyId | true | Customer id from the API Settings Page.

## Validate
The Validate endpoint will return a Boolean to inform the client whether the token is valid or not.

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>https://{{API_Url}}/validate</h6>
	</div>
</div>

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
> The above command returns a boolean:

```json
true
```

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | Stored token from the GetToken endpoint.
