# Authentication

SmartConnect requires the user email, password, and companyId to generate a token.
The password will need to be encrypted using RSA encrytion using [the PKCS #1 standard](https://en.wikipedia.org/wiki/PKCS_1)

<aside class="notice">
Tokens are company specific and are valid for 3 months. If your username has access to multiple companies, you will need to generate a separate token for each company.
</aside>

## Password Encryption

Password encryption will be required prior to sending the requests for "GetCompanyList" and "GetToken".
Encryption will require the most recent public-key containing the RSA module and exponent.

If you want to quickly encrypt your password for testing, [it can be done here](https://jsfiddle.net/ethan_sorenson/t0zhrq4v/)

### HTTP Request

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
    print(base64.b64encode(encrypted).decode())
```

```javascript
function encryptPassword() {
  var password = 'my password';
  var n = 'vYofWQ63vYJZTB/EW4NT5LMlfiB5LftMMQSbJgFHvfE+Z8AXnCLNplBByGLWBLMolSwJZjJdUN5gLF0V/Q9aVwOK5GWAezt8IPwMYqAgwQ2btnlhrsKLkKoTtogGj9MYz9briYn/DHZlW56aOYvwSoz1LYhnoja59cG2UvIXxVE=';
  var e = 'AQAB';
  var rsa = forge.pki.rsa;
  var BigInteger = forge.jsbn.BigInteger;

  function parseBigInteger(b64) {
    return new BigInteger(forge.util.createBuffer(forge.util.decode64(b64)).toHex(), 16);
   }
  var pubKey = rsa.setPublicKey(parseBigInteger(n), parseBigInteger(e));
  var encryptedData = pubKey.encrypt(document.getElementById(password).value, 'RSAES-PKCS1-V1_5');
  // write output
  document.write('Encrypted Password: ' + btoa(encryptedData) +
  '<br>Unencrypted Password: ' + encodeURIComponent(btoa(encryptedData)));
}
```

```csharp
string password = "my password";
string rsaXml;
System.Net.WebRequest request = System.Net.WebRequest.Create("https://login.smartconnect.com/v1/public-key");
request.Method = "POST";
request.ContentLength = 0;
System.Net.WebResponse response = request.GetResponse();

using (System.IO.StreamReader responseStream = new System.IO.StreamReader(response.GetResponseStream()))
    {
        string result = responseStream.ReadToEnd();
	    rsaXml = result.Replace("\"","");
    }

using (RSACryptoServiceProvider RSA = new RSACryptoServiceProvider(1024))
    {
        RSA.FromXmlString(rsaXml);
	    byte[] plainText = System.Text.Encoding.ASCII.GetBytes(password);
        byte[] cipherbytes = RSA.Encrypt(plainText, System.Security.Cryptography.RSAEncryptionPadding.Pkcs1);
	    // Message Box with encrypted password
        MessageBox.Show(Convert.ToBase64String(cipherbytes));
    }
```

> A request for the public key will return an xml payload:

```xml
<RSAKeyValue>
    <Modulus>vYofWQ63vYJZTB/EW4NT5LMlfiB5LftMMQSbJgFHvfE+Z8AXnCLNplBByGLWBLMolSwJZjJdUN5gLF0V/Q9aVwOK5GWAezt8IPwMYqAgwQ2btnlhrsKLkKoTtogGj9MYz9briYn/DHZlW56aOYvwSoz1LYhnoja59cG2UvIXxVE=</Modulus>
    <Exponent>AQAB</Exponent>
</RSAKeyValue>
```

> An encrypted password will look like this:

```json
"Bqr+OK0r4f8gURRCt3RCmxfem2PF2lE5lMIZ2ZsOEu9rDmEk93wuFrFIRFPLX4Wm8HDq7HgNn/mj7TT18uQj1UpqFVoizQLs6TWgLLUdqjxqeTFamvJaAVUh392tsQDTgkHkU4UwB8MABay2lr987GJIUDd4MaC2Aj11t8XjaXCU="
```

<aside class="notice">
The password is padded meaning that random data is added to ensure that encrypting the same plaintext twice will generate different ciphertext.
</aside>

## GetCompanyList

Optionally, you can retrieve a list of companies with the GetCompanyList endpoint.
This will return the CustomerId the same as shown in the SmartConnect client.

<aside class="notice">
When calling the GetCompanyList endpoint make sure the RSA encrypted password is URI encoded, it should end with '%3D'
</aside>

### HTTP Request

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-post">POST</i>
		<h6>https://{{api_url}}/gettoken</h6>
	</div>
</div>

> To retrieve a list of companies, use this code:

```shell
curl POST "{{api_url}}/GetCompanyList?email={{Username}}&password={{EncryptedPassword}}"
  -H 'Accept: application/json' 
```

```python
import requests
import json
url = "{{API Url}}/GetCompanyList?email={{Username}}&password={{EncryptedPassword}}&companyId={{Customer Id}}"

response = requests.request("POST", url)

print(response.text.encode('utf8'))
```

```javascript
var xhr = new XMLHttpRequest();
xhr.open("POST", "{{api_url}}/GetCompanyList?email={{Username}}&password={{EncryptedPassword}}");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send();
```

```csharp
System.Net.WebRequest request = System.Net.WebRequest.Create("https://{{api_url}}/GetCompanyList?email={{Username}}&password={{EncryptedPassword}}");
request.Method = "POST";
request.ContentLength = 0;
request.ContentType = "application/json";

System.Net.WebResponse response = request.GetResponse();

using(System.IO.StreamReader responseStream = new System.IO.StreamReader(response.GetResponseStream()))
{
 string result = responseStream.ReadToEnd(); 
}

return result;
```

> The above command returns JSON structured like this:

```json
[
    {
        "Id": "48caf272-d140-449c-aa3b-c5d0a03082dc",
        "Name": "Demo Company 1",
        "SubscriptionExpiry": null
    },
    {
        "Id": "2a1aec61-e7d0-4fb0-b597-d08cc2992efa",
        "Name": "Demo Company 2",
        "SubscriptionExpiry": null
    }
]
```
## GetToken

The GetToken endpoint requires 3 parameters. To generate a token.

<aside class="notice">
When calling the GetToken endpoint make sure the RSA encrypted password is URI encoded, it should end with '%3D'
</aside>

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

```javascript
var xhr = new XMLHttpRequest();
xhr.open("POST", "{{api_url}}/GetToken?email={{Username}}&password={{EncryptedPassword}}&companyId={{Customer Id}}");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send();
```

```csharp
System.Net.WebRequest request = System.Net.WebRequest.Create("https://{{api_url}}/GetToken?email={{Username}}&password={{EncryptedPassword}}&&companyId={{Customer Id}");
request.Method = "POST";
request.ContentLength = 0;
request.ContentType = "application/json";

System.Net.WebResponse response = request.GetResponse();

using(System.IO.StreamReader responseStream = new System.IO.StreamReader(response.GetResponseStream()))
{
 string result = responseStream.ReadToEnd(); 
}

return result;
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
```javascript
var xhr = new XMLHttpRequest();
xhr.open("POST", "{{api_url}}/validate?token={{Token}}");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send();
```

```csharp
System.Net.WebRequest request = System.Net.WebRequest.Create("{{api_url}}/validate?token={{Token}}");
request.Method = "POST";
request.ContentLength = 0;
request.ContentType = "application/json";

System.Net.WebResponse response = request.GetResponse();

using(System.IO.StreamReader responseStream = new System.IO.StreamReader(response.GetResponseStream()))
{
 string result = responseStream.ReadToEnd(); 
}

return result;
```

> The above command returns a boolean:

```json
true
```

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | Stored token from the GetToken endpoint.
