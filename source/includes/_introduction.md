# Introduction

### Welcome to the SmartConnect API!

The API can be used to remotely interact with SmartConnect. Some of the core functions are:

* Request data from a pre-defined SmartConnect Data Source Query
* Run an Integration Process
* Retrieve Errors from SmartConnect

## Requirements

To use this version of the REST API you will need the following:

* An active SaaS subscription for [SmartConnect.com](https://smartconnect.com/)
* A valid user login to [SmartConnect.com](https://login.smartconnect.com/)

## Getting Started

Connecting to the API will require the following information. The API Settings can be access in SmartConnect.com > System API Settings

<aside class="warning">
If using JavaScript, note that the web service does not support [Cross-origin resource sharing](https://www.eonesolutions.com/help-article/no-access-control-allow-origin-header-is-present-on-the-requested-resource/)
</aside>

### Needed Variables

Variable | Description
--------- | -----------
API Url | Base Url for accessing the SmartConnect API.
Customer Id | Unique identifier for your SmartConnect tenant.
Username | The email address of a user with access to the SmartConnect tenant.
Password | Password the user uses to access the SmartConnect.com application.


![Image of API Settings](https://www.eonesolutions.com/wp-content/uploads/2020/01/API_Details.png)

<aside class="notice">
A Postman collection with examples can be [downloaded here](https://www.eonesolutions.com/wp-content/uploads/2020/01/SmartConnect.com-Web-Service-Collection.zip)
</aside>