##URIs  

The following describes the possible URIs for each resource. For information about each URI and an example request and response, see the URI.

| Resource      | URI                                           | Verb         | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------|-----------------------------------------------|--------------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Account       | /accounts                                     | GET          | Yes      | Gets a list of all accounts.                                                                                                                                                                                                                                                                                                                                                                                                                               |
|               |                                               | POST         | Yes      | Adds an account.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|               | /accounts/{id}                                | GET          | Yes      | Gets the specified account.                                                                                                                                                                                                                                                                                                                                                                                                                                |
|               | /accounts?$filter=                            | GET          | Yes      | Gets a list of accounts that match the specified filter,criteria.,The user may use OData expressions with the following,Account properties.,·,AdvertiserId,·,BuyerId,May support getting a list of IDs.                                                                                                                                                                                                                                                    |
| Assignment    | /accounts/{id}/assignments                    | GET          | Yes      | Gets a list of all assignments that belong to the account.                                                                                                                                                                                                                                                                                                                                                                                                 |
|               |                                               | POST         | YES      | Adds an assignment to the specified account. To add an,assignment, the creative must be approved.,An assignment may be added at any time prior to the order finishing,its flight.                                                                                                                                                                                                                                                                          |
|               | /accounts/{id}/assignments/{id}               | GET          | Yes      | Gets the specified assignment.                                                                                                                                                                                                                                                                                                                                                                                                                             |
|               |                                               | PUT or PATCH | Yes      | Updates the specified assignment.                                                                                                                                                                                                                                                                                                                                                                                                                          |
|               |                                               | DELETE       | Yes      | Deletes the specified assignment.,May delete an assignment only if it has never delivered,impressions.                                                                                                                                                                                                                                                                                                                                                     |
|               | /accounts/{id}/assignments/{id}?disable       | PUT or PATCH | Yes      | Changes the status to Inactive.                                                                                                                                                                                                                                                                                                                                                                                                                            |
|               | /accounts/{id}/assignments?$filter=           | GET          | No       | Gets a list of assignments that match the specified filter,criteria.,The user may use OData expressions with the following,Assignment properties.,·,CreativeId,·,LineId,·,StartDate,·,EndDate,May support getting a list by IDs.                                                                                                                                                                                                                           |
| Creative      | /accounts/{id}/creatives                      | GET          | Yes      | Gets a list of all creatives that belong to the account.                                                                                                                                                                                                                                                                                                                                                                                                   |
|               |                                               | POST         | Yes      | Adds a creative to the account.                                                                                                                                                                                                                                                                                                                                                                                                                            |
|               | /accounts/{id}/creatives/{id}                 | GET          | Yes      | Gets the specified creative.                                                                                                                                                                                                                                                                                                                                                                                                                               |
|               |                                               | PUT or PATCH | Yes      | Updates,the properties of the creative object; however, the user may not update the,following properties.,·,ClickURL,·,CreativeAsset,·,BackupFlashAsset                                                                                                                                                                                                                                                                                                    |
|               |                                               | DELETE       | Yes      | Deletes the specified creative. May delete a creative only,if it has no assignments.                                                                                                                                                                                                                                                                                                                                                                       |
|               | /accounts/{id}/creatives?$filter=             | GET          | No       | Gets a list of creatives that match the specified filter,criteria.,The user may use OData expressions with the following,Creative properties.,·,AdQualityStatus,May support getting a list by IDs.                                                                                                                                                                                                                                                         |
| Line          | /accounts/{id}/orders/{id}/lines              | GET          | Yes      | Gets a list of all lines in the order.                                                                                                                                                                                                                                                                                                                                                                                                                     |
|               |                                               | POST         | Yes      | Adds a line to the order.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|               | /accounts/{id}/orders/{id}/lines/{id}         | GET          | Yes      | Gets the specified line from the order.                                                                                                                                                                                                                                                                                                                                                                                                                    |
|               |                                               | PUT or PATCH | Yes      | Updates the specified line.,To update a line, the line must be in the Draft state.                                                                                                                                                                                                                                                                                                                                                                         |
|               |                                               | DELETE       | Yes      | Deletes the specified line.,May delete a line only if it’s in the Draft state. Must,also delete assignments that reference the line.                                                                                                                                                                                                                                                                                                                       |
|               | /accounts/{id}/orders/{id}/lines?$filter=     |              |          | Gets a list of lines that match the specified filter,criteria.,The user may use OData expressions and method calls with,the following Line properties.,·,Name,·,BookingStatus,·,StartDate,·,EndDate,May support getting a list by IDs.                                                                                                                                                                                                                     |
|               | /accounts/{id}/orders/{id}/lines/{id}?book    | PUT or PATCH | Yes      | Begins the booking process for the line. The booking,process may be asynchronous.,To book a line, the line must:,·,Be in the Draft or Reserved state.,·,Have a creative assigned.,·,Have available impressions.,If successfully booked, the line moves to the Booked,state; otherwise, it moves to Declined and sets StateChangedReason.                                                                                                                   |
|               | /accounts/{id}/orders/{id}/lines/{id}?reserve | PUT or PATCH | Yes      | Reserves the line. The reserve process may be,asynchronous.,To reserve a line, the line must be in the Draft state.,If successfully reserved, the line moves to the Reserved,state; otherwise, it moves to Declined and StateChangedReason is set.,Supporting reserve is optional.                                                                                                                                                                         |
|               | /accounts/{id}/orders/{id}/lines/{id}?cancel  | PUT or PATCH | Yes      | Cancels the line.,To cancel a line, the line must be in the Reserved,,Booked, or InFlight state.,If successfully canceled, the line moves to the Canceled,state. If the status was InFlight, StateChangedReason is set.                                                                                                                                                                                                                                    |
|               | /accounts/{id}/orders/{id}/lines/{id}?reset   | PUT or PATCH | Yes      | Resets a line back to the Draft state.,To reset a line, the line must be in the Reserved or,Declined state.                                                                                                                                                                                                                                                                                                                                                |
| Order         | /accounts/{id}/orders                         | GET          | Yes      | Gets a list of all orders that belong to the account.                                                                                                                                                                                                                                                                                                                                                                                                      |
|               |                                               | POST         | Yes      | Adds an order to the account.                                                                                                                                                                                                                                                                                                                                                                                                                              |
|               | /accounts/{id}/orders/{id}                    | GET          | Yes      | Gets the specified order.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|               |                                               | PUT or PATCH | Yes      | Updates the specified order.                                                                                                                                                                                                                                                                                                                                                                                                                               |
|               |                                               | DELETE       | Yes      | Deletes the specified order.,May delete the order only if all lines in the order are in,the Draft state.                                                                                                                                                                                                                                                                                                                                                   |
|               | /accounts/{id}/orders?$filter=                | GET          | No       | Gets a list of orders that match the specified filter,criteria.,The user may use OData expressions and method calls with,the following Order properties.,·,Name,·,StartDate,·,EndDate,May support getting a list by IDs.                                                                                                                                                                                                                                   |
| Organization  | /organizations                                | GET          | Yes      | Gets a list of all organizations that the user has access,to.,The list may contain both advertiser and agency,organizations depending on the caller’s access. For example, if the caller is,an advertiser, the list will contain only the advertiser’s organization,objects; however, if the caller is an agency, the list will contain the,agency’s organization objects and the organization objects of the advertisers,whose accounts that they manage. |
|               |                                               | POST         | Yes      | Adds an organization.,Note that POST is not supported in the public API; it is,included here for completeness. The process of adding advertiser and agency,organizations and providing credentials is publisher defined.                                                                                                                                                                                                                                   |
|               | /organizations/{id}                           | GET          | Yes      | Gets the specified organization.                                                                                                                                                                                                                                                                                                                                                                                                                           |
|               |                                               | PUT or PATCH | Yes      | Updates the specified organization.,The caller must have permissions to update the,organization. For example, an advertiser and agency may update their,organization object but an agency may not update an advertiser’s Organization,object.                                                                                                                                                                                                              |
|               |                                               | DELETE       |          | The process of deleting an organization is publisher,defined; however, deleting an organization via the API is not supported.                                                                                                                                                                                                                                                                                                                              |
|               | /organizations?$filter=                       | Yes          | Yes      | Gets a list of organizations that match the specified,filter criteria.,The user may use OData expressions and method calls with,the following Organization properties.,·,Name,·,Status,One or more organization IDs                                                                                                                                                                                                                                        |
| Product       | /products                                     | GET          | Yes      | Gets a list of all products from the publisher’s product,catalog.                                                                                                                                                                                                                                                                                                                                                                                          |
|               | /products/{id}                                | GET          | Yes      | Gets the specified product from the publisher’s product,catalog.                                                                                                                                                                                                                                                                                                                                                                                           |
|               | /products/search                              | POST         | Yes      | Gets a list of products from the publisher’s product,catalog based on the criteria specified in the body of the request.,For a list of the filter criteria that a caller may specify,,see ProductSearch.,The body of the response contains a collection of Product objects that match the filter,criteria.                                                                                                                                                 |
| ProductAvails | /products/avails                              | POST         | Yes      | Gets the availability and pricing information for a specified list of products based on flight dates, quantity and targeting.,The body of the request contains the list of products and flight details (See ProductAvailsSearch).,The body of the response contains a collection of ProductAvails objects (one for each product specified in the request).                                                                                                 |

####/organizations

#####Description

Gets a list of Organizations that the user has access to.

#####Rules

The list will contain a single organization for advertisers; however, for agencies, the list will include the agency’s organization and the organizations of the advertisers whose accounts they manage

#####Example Request

`GET https://<host>/<path>/<version>/organizations HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1879

```json
{
  "organizations":[
    {
      "address":{
        "addressLine1":"1234 Tiger Blvd",
        "city":"Redmond",
        "country":"US",
        "postalCode":"98123",
        "state":"WA"
      },
      "contacts":[
        {
          "address":{
            "addressLine1":"1234 Tiger Blvd",
            "city":"Redmond",
            "country":"US",
            "postalCode":"98123",
            "state":"WA"
          },
          "email":"jsilver@contoso.com",
          "honorific":"Ms",
          "fax":"2065551212",
          "firstName":"Janet",
          "lastName":"Silver",
          "phone":"2065550101",
          "title":"Comptroller",
          "type":"Billing"
        }
      ],
      "fax":"2065551212",
      "id":"12345678",
      "industry":"Automotive",
      "name":"Contoso",
      "phone":"2065550100",
      "providerData":"cid=89345",
      "status":"Approved",
      "url":"http://contoso.com"
    }
  ]
}
```

####/organization/{id}

#####Description

Gets or updates the specified organization.

#####Rules

The user must have permissions to perform the requested action. For example, advertisers and agencies may get and update the Organization that they own; however, an agency may only get the organization of the advertisers whose accounts they manage.

An agency may not update an advertiser’s organization.

#####Example GET Request

`GET https://<host>/<path>/<version>/organizations/12345678 HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1879

```json
{
  "address":{
    "addressLine1":"1234 Tiger Blvd",
    "city":"Redmond",
    "country":"US",
    "postalCode":"98123",
    "state":"WA"
  },
  "contacts":[
    {
      "address":{
        "addressLine1":"1234 Tiger Blvd",
        "city":"Redmond",
        "country":"US",
        "postalCode":"98123",
        "state":"WA"
      },
      "email":"jsilver@contoso.com",
      "honorific":"Ms",
      "fax":"2065551212",
      "firstName":"Janet",
      "lastName":"Silver",
      "phone":"2065550101",
      "title":"Comptroller",
      "type":"Billing"
    }
  ],
  "fax":"2065551212",
  "id":"12345678",
  "industry":"Automotive",
  "name":"Contoso",
  "phone":"2065550100",
  "providerData":"cid=89345",
  "status":"Approved",
  "url":"http://contoso.com"
}
```

#####Example PATCH Request

`PATCH https://<host>/<path>/<version>/organizations/12345678 HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "id":"12345678",
  "contacts":[
    {
      "email":"bnicks@contoso.com",
      "honorific":"Mr",
      "fax":"2065551212",
      "firstName":"Bill",
      "lastName":"Nicks",
      "phone":"2065550105",
      "title":"Comptroller",
      "type":"Billing"
    }
  ]
}
```

#####Example PATCH Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 1879'

```json
{
  "address":{
    "addressLine1":"1234 Tiger Blvd",
    "city":"Redmond",
    "country":"US",
    "postalCode":"98123",
    "state":"WA"
  },
  "contacts":[
    {
      "address":{
        "addressLine1":"1234 Tiger Blvd",
        "city":"Redmond",
        "country":"US",
        "postalCode":"98123",
        "state":"WA"
      },
      "email":"bnicks@contoso.com",
      "honorific":"Mr",
      "fax":"2065551212",
      "firstName":"Bill",
      "lastName":"Nicks",
      "phone":"2065550105",
      "title":"Comptroller",
      "type":"Billing"
    }
  ],
  "fax":"2065551212",
  "id":"12345678",
  "industry":"Automotive",
  "name":"Contoso",
  "phone":"2065550100",
  "providerData":"cid=89345",
  "status":"Approved",
  "url":"http://contoso.com"
}
```

####/organizations?$filter=

#####Description

#####Rules

#####Example Request

#####Example Response

####/accounts

#####Description

Adds an Account or gets a list of accounts that the user has access to.

#####Rules

An advertiser or agency may add accounts to only the organization they own; an agency may not add accounts to an advertiser’s organization. If an advertiser wants an agency to manage an account on their behalf, the advertiser must add the account and set the account’s buyerId to the agency’s organization ID.

An organization may add as many accounts as needed to create a buying structure that supports their needs. For example, the organization may create a single account, an account for each region, an account for each brand, and so on.

For an advertiser, the list of accounts will include only accounts that they own. However, for an agency, the list of accounts will include the accounts that they own and the accounts that they manage on behalf of advertisers.

#####Example POST Request

`POST https://<host>/<path>/<version>/accounts HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "advertiserId":"1234987",
  "buyerId":"34587",
  "name":"Brand A",
  "providerData":"cid=934759"
}
```

#####Example POST Response

HTTP/1.1 200 OK
Location: `https://<host>/<path>/<version>/accounts/23873345`
Content-Type: application/json
Content-Length: 379

```json
{
  "advertiserId":"1234987",
  "buyerId":"34587",
  "id":"23873345",
  "name":"Brand A",
  "providerData":"cid=934759"
}
```

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 187

```json
{
  "accounts":[
    {
      "advertiserId":"1234987",
      "buyerId":"1234987",
      "id":"9876542",
      "name":"Brand B",
      "providerData":"cid=8934579"
    },
    {
      "advertiserId":"1234987",
      "buyerId":"34587",
      "id":"23873345",
      "name":"Brand A",
      "providerData":"cid=934759"
    }
  ]
}
```

####/accounts/{id}

#####Description

Gets the specified Account.

#####Rules

The user must have permissions to perform the requested action. For example, advertisers and agencies may get the accounts that they own. In addition, an agency may get the accounts that they manage on behalf of advertisers.

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345 HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 187

```json
{
  "advertiserId":"1234987",
  "buyerId":"34587",
  "id":"23873345",
  "name":"Brand A",
  "providerData":"cid=934759"
}
````

####/accounts?$filter=

#####Description

#####Rules

#####Example Request

#####Example Response

####/accounts/{id}/assignments

#####Description

Adds an Assignment or gets a list of assignments that the user has access to.

#####Rules

An advertiser or agency may add assignments to accounts that they own. In addition; an agency may add assignments to accounts that they manage on behalf of advertisers.

For advertisers, the list will include only assignments that they own. For agencies, the list will include the assignments that they own and the assignments that belong to accounts that they manage on behalf of advertisers.

#####Example POST Request

`POST https://<host>/<path>/<version>/accounts/23873345/assignments HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "creativeId":"394857",
  "lineId":"394578",
  "weight":75,
  "providerData":"cid=98374"
}
```

#####Example POST Response

HTTP/1.1 200 OK
Location: `https://<host>/<path>/<version>/accounts/23873345/assignments/34534`

Content-Type: application/json
Content-Length: 187

```json
{
  "creativeId":"394857",
  "lineId":"394578",
  "Id":"34534",
  "weight":75,
  "status":"Active",
  "providerData":"cid=98374"
}
```

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/assignments HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 387

```json
{
  "assignments":[
    {
      "creativeId":"394857",
      "lineId":"394578",
      "weight":75,
      "id":"34534",
      "status":"Active",
      "providerData":"cid=98374"
    },
    {
      "creativeId":"54345",
      "lineId":"394578",
      "weight":25,
      "id":"453365",
      "status":"Active",
      "providerData":"cid=34325"
    }
  ]
}
```

####/accounts/{id}/assignments/{id}

#####Description

Gets, updates, or deletes the specified Assignment.

#####Rules

The user must have permissions to perform the requested action. For example, advertisers and agencies may get, update, and delete the assignments that they own. In addition, an agency may get, update, and delete assignments that belong to the accounts that they manage on behalf of advertisers.

An assignment may be deleted only if it has never delivered impressions.

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/assignments/453365 HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 108

```json
{
  "creativeId":"54345",
  "lineId":"394578",
  "weight":25,
  "id":"453365",
  "status":"Active",
  "providerData":"cid=34325"
}
```

#####Example PATCH Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/assignments/453365 HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "weight":30
}
```

#####Example PATCH Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 108

```json
{
  "creativeId":"54345",
  "lineId":"394578",
  "weight":30,
  "id":"453365",
  "status":"Active",
  "providerData":"cid=34325"
}
```

####/tenants/{id}/accounts/{id}/assignments/{id}?disable

#####Description

Prevents a creative from running or stops a creative that is currently running.

#####Rules

The user must have permissions to access the assignment. For example, advertisers and agencies may disable Assignments that they own. In addition, an agency may disable assignments that belong to the accounts that they manage on behalf of advertisers.

#####Example Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/assignments/453365 HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 108

```json
{
  "creativeId":"54345",
  "lineId":"394578",
  "weight":30,
  "id":"453365",
  "status":"Inactive",
  "providerData":"cid=34325"
}
```

####/accounts/{id}/assignments?$filter=

#####Description

Gets a list of Assignments that match the specified filter criteria.

The caller may use OData expressions with the following Assignment properties.

  - CreativeId
  - LineId
  - StartDate
  - EndDate

#####Rules

The user must have permissions to access the assignment. For example, advertisers and agencies may get assignments that they own. In addition, an agency may get assignments that belong to the accounts that they manage on behalf of advertisers.

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/assignments?$filter=LineId+eq+394578 HTTP/1.1`
Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 108

```json
{
  "assignments":[
    {
      "creativeId":"394857",
      "lineId":"394578",
      "weight":75,
      "id":"65433",
      "status":"Active",
      "providerData":"cid=98374"
    },
    {
      "creativeId":"54345",
      "lineId":"394578",
      "weight":25,
      "id":"453365",
      "status":"Active",
      "providerData":"cid=34325"
    }
  ]
}
```

####/accounts/{id}/creatives

#####Description

Adds a Creative or gets a list of creatives that the user has access to.

#####Rules

An advertiser or agency may add creatives to accounts that they own. In addition; an agency may add creatives to accounts that they manage on behalf of advertisers.

For advertisers, the list will include only creatives that they own. For agencies, the list will include the creatives that they own and the creatives that belong to accounts that they manage on behalf of advertisers.

#####Example POST Request

`POST https://<host>/<path>/<version>/accounts/23873345/creatives HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "accountId":"23873345",
  "adFormatType":"Tag",
  "creativeAsset":"<third-party script goes here>",
  "geometry":{
    "height":"160",
    "width":"600"
  },
  "language":"EN",
  "maturityLevel":"General",
  "name":"My Creative",
  "providerData":"cid=54574"
}
```

#####Example POST Response

HTTP/1.1 200 OK
Location: `https://<host>/<path>/<version>/accounts/23873345/creatives/53444`

Content-Type: application/json
Content-Length: 108

```json
{
  "accountId":"23873345",
  "adFormatType":"Tag",
  "adQualityStatus":"Pending",
  "creativeAsset":"<third-party script goes here>",
  "geometry":{
    "height":"160",
    "width":"600"
  },
  "httpsCompatible":0,
  "id":"53444",
  "language":"EN",
  "maturityLevel":"General",
  "name":"My Creative",
  "providerData":"cid=54574"
}
```

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/creatives HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 187

```json
{
  "creatives":[
    {
      "accountId":"23873345",
      "adFormatType":"Tag",
      "adQualityStatus":"Approved",
      "creativeAsset":"<third-party script goes here>",
      "geometry":{
        "height":"160",
        "width":"600"
      },
      "httpsCompatible":0,
      "id":"53444",
      "language":"EN",
      "maturityLevel":"General",
      "name":"My Creative",
      "providerData":"cid=54574"
    }
  ]
}
```

####/accounts/{id}/creatives/{id}

#####Description

Gets, updates, or deletes the specified Creative.

#####Rules

The user must have permissions to perform the requested action. For example, advertisers and agencies may get, update, and delete the creatives that they own. In addition, an agency may get, update, and delete the creatives that belong to the accounts that they manage on behalf of advertisers.

A creative may be deleted only if it has no assignments.

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/creatives/53444 HTTP/1.1`
Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 308

```json
{
  "accountId":"23873345",
  "adFormatType":"Tag",
  "adQualityStatus":"Pending",
  "creativeAsset":"<third-party script goes here>",
  "geometry":{
    "height":"160",
    "width":"600"
  },
  "httpsCompatible":0,
  "id":"53444",
  "language":"EN",
  "maturityLevel":"General",
  "name":"My Creative",
  "providerData":"cid=54574"
}
```

#####Example PATCH Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/creatives/53444 HTTP/1.1`
Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "clickUrl":"http://domain.com/path"
}
```

#####Example PATCH Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 308

```json
{
  "accountId":"23873345",
  "adFormatType":"Tag",
  "adQualityStatus":"Pending",
  "clickUrl":"http://domain.com/path"
  "creativeAsset":"<third-party script goes here>",
  "geometry":{
    "height":"160",
    "width":"600"
  },
  "httpsCompatible":0,
  "id":"53444",
  "language":"EN",
  "maturityLevel":"General",
  "name":"My Creative",
  "providerData":"cid=54574"
}
```

####/accounts/{id}/creatives?$filter=

#####Description

#####Rules

#####Example Request

#####Example Response

####/accounts/{id}/orders

#####Description

Adds an Order or gets a list of orders that the user has access to.

#####Rules

An advertiser or agency may add orders to accounts that they own. In addition; an agency may add orders to accounts that they manage on behalf of advertisers.

For advertisers, the list will include only orders that they own. For agencies, the list will include the orders that they own and the orders that belong to accounts that they manage on behalf of advertisers.

#####Example POST Request

`POST https://<host>/<path>/<version>/accounts/23873345/orders HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "accountId":"23873345",
  "brand":"Four Wakes",
  "budget":50000,
  "currency":"USD",
  "endDate":"2014-12-24T18:00:00.000Z",
  "name":"My Order",
  "providerData":"cid=563364",
  "startDate":"2014-11-24T06:00:00.000Z",
}
```

Example POST Response

HTTP/1.1 200 OK
Location: 	`https://<host>/<path>/<version>/accounts/23873345/orders/1235872`
Content-Type: application/json
Content-Length: 108

```json
{
  "accountId":"23873345",
  "brand":"Four Wakes",
  "budget":50000,
  "currency":"USD",
  "endDate":"2014-12-24T18:00:00.000Z",
  "id":"1235872",
  "name":"My Order",
  "preferredBillingMethod":"Electronic",
  "providerData":"cid=563364",
  "startDate":"2014-11-24T06:00:00.000Z",
}
```

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/orders HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 187

```json
{
  "orders":[
    {
      "accountId":"23873345",
      "brand":"Four Wakes",
      "budget":50000,
      "currency":"USD",
      "endDate":"2014-12-24T18:00:00.000Z",
      "id":"1235872",
      "name":"My Order",
      "preferredBillingMethod":"Electronic",
      "providerData":"cid=563364",
      "startDate":"2014-11-24T06:00:00.000Z",
    }
  ]
}
```

####/accounts/{id}/orders/{id}

#####Description

Gets, updates or deletes the specified Order.

#####Rules

The user must have permissions to perform the requested action. For example, advertisers and agencies may get, update, and delete the orders that they own. In addition, an agency may get, update, and delete the orders that belong to the accounts that they manage on behalf of advertisers.

Only orders in the Draft booking state may be deleted.

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/orders/1235872 HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 158

```json
{
  "accountId":"23873345",
  "brand":"Four Wakes",
  "budget":50000,
  "currency":"USD",
  "endDate":"2014-12-24T18:00:00.000Z",
  "id":"1235872",
  "name":"My Order",
  "preferredBillingMethod":"Electronic",
  "providerData":"cid=563364",
  "startDate":"2014-11-24T06:00:00.000Z",
}
```

#####Example PATCH Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/orders/1235872 HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "startDate":"2014-12-05T18:00:00.000Z",
  "name":"My Better Order Name"
}
```

#####Example PATCH Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 358

```json
{
  "accountId":"23873345",
  "brand":"Four Wakes",
  "budget":50000,
  "currency":"USD",
  "endDate":"2014-12-24T18:00:00.000Z",
  "id":"1235872",
  "name":"My Better Order Name",
  "preferredBillingMethod":"Electronic",
  "providerData":"cid=563364",
  "startDate":"2014-12-05T18:00:00.000Z ",
}
```

####/accounts/{id}/orders?$filter=

#####Description

#####Rules

#####Example Request

#####Example Response

####/accounts/{id}/orders/{id}/lines

#####Description

Adds a Line to an order or gets a list of lines that the user has access to.

#####Rules

An advertiser or agency may add lines to orders that they own. In addition; an agency may add lines to orders that they manage on behalf of advertisers.

For advertisers, the list will include only lines that they own. For agencies, the list will include the lines that they own and the lines that belong to accounts that they manage on behalf of advertisers.

#####Example POST Request

`POST https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "comment":"Free form comment",
  "endDate":"2014-12-10T18:00:00.000Z",
  "frequencyCount":3,
  "frequencyInterval":"Day",
  "quantity":30000,
  "name":"My Line 1",
  "productId":"456366",
  "providerData":"cid=88873",
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
}
```

#####Example POST Response

HTTP/1.1 200 OK
Location: `https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines/345233`

Content-Type: application/json
Content-Length: 878

```json
{
  "bookingStatus":"Draft",
  "comment":"Free form comment",
  "endDate":"2014-12-10T18:00:00.000Z",
  "frequencyCount":3,
  "frequencyInterval":"Day",
  "id":"345233",
  "quantity":30000,
  "name":"My Line 1",
  "orderId":"1235872",
  "productId":"456366",
  "providerData":"cid=88873",
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
  "usesExpandables":0
}
```

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 587

```json
{
  "lines":[
    {
      "bookingStatus":"Booked",
      "comment":"Free form comment",
      "cost":39.30,
      "endDate":"2014-12-10T18:00:00.000Z",
      "frequencyCount":3,
      "frequencyInterval":"Day",
      "id":"345233",
      "quantity":30000,
      "name":"My Line 1",
      "orderId":"1235872",
      "productId":"456366",
      "providerData":"cid=88873",
      "rate":1.31,
      "rateType":"CPM",
      "startDate":"2014-12-05T06:00:00.000Z",
      "targeting":[
        {
          "target":"Age",
          "targetValues":["18-24","25-34"]
        },
        {
          "target":"Gender",
          "targetValues":["Male"]
        }
      ]
      "usesExpandables":0
    }
  ]
}
```

####/accounts/{id}/orders/{id}/lines/{id}

#####Description

Gets, updates, or deletes the specified Line.

#####Rules

The user must have permissions to perform the requested action. For example, advertisers and agencies may get, update, and delete the Lines that they own. In addition, an agency may get, update, and delete the lines that belong to the accounts that they manage on behalf of advertisers.

A line may be deleted only if it’s in the Draft state. In addition, all assignments that reference the line must be deleted.

#####Example GET Request

`GET https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines/345233 HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example GET Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 158

```json
{
  "bookingStatus":"Draft",
  "comment":"Free form comment",
  "endDate":"2014-12-10T18:00:00.000Z",
  "frequencyCount":3,
  "frequencyInterval":"Day",
  "id":"345233",
  "quantity":30000,
  "name":"My Line 1",
  "orderId":"1235872",
  "productId":"456366",
  "providerData":"cid=88873",
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
  "usesExpandables":0
}
```

#####Example PATCH Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines/345233 HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

```json
{
  "frequencyCount":NULL,
  "frequencyInterval":NULL,
}
```

#####Example PATCH Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 458

```json
{
  "bookingStatus":"Draft",
  "comment":"Free form comment",
  "endDate":"2014-12-10T18:00:00.000Z",
  "id":"345233",
  "quantity":30000,
  "name":"My Line 1",
  "orderId":"1235872",
  "productId":"456366",
  "providerData":"cid=88873",
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
  "usesExpandables":0
}
```

####/accounts/{id}/orders/{id}/lines?$filter=

#####Description

#####Rules

#####Example Request

#####Example Response

####accounts/{id}/orders/{id}/lines/{id}?book

#####Description

Books the line.

#####Rules

The user must have permissions to book the line. For example, advertisers and agencies may book Lines that they own. In addition, an agency may book lines that belong to the accounts that they manage on behalf of advertisers.

Only organizations that have an Approved or Limited status may book lines.

To book a line, the line must:

  - Be in the Draft or Reserved booking state.
  - Have a creative assigned.
  - Have available quantity/impressions

The booking process may be asynchronous. If asynchronous, set the bookingStatus field to PendingBooking until the line is booked or declined. If successfully booked, set the bookingStatus field to Booked; otherwise, set the bookingStatus field to Declined and specify why the request was declined in the StateChangedReason field.

#####Example Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines/345233?book HTTP/1.1`
Content-Type: application/json
AccessToken: `<OAuth token>`

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 458

```json
{
  "bookingStatus":"Draft",
  "comment":"Free form comment",
  "endDate":"2014-12-10T18:00:00.000Z",
  "id":"345233",
  "quantity":30000,
  "name":"My Line 1",
  "orderId":"1235872",
  "productId":"456366",
  "providerData":"cid=88873",
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
  "usesExpandables":0
}
```

####/accounts/{id}/orders/{id}/lines/{id}?reserve

#####Description

Reserves the line.

#####Rules

The user must have permissions to reserve the line. For example, advertisers and agencies may reserve Lines that they own. In addition, an agency may reserve lines that belong to the accounts that they manage on behalf of advertisers.

Only organizations that have an Approved or Limited status may reserve lines.

To reserve a line, the line must be in the Draft booking state.

The reservation process may be asynchronous. If asynchronous, set the bookingStatus field to PendingReservation until the line is reserved or declined. If successfully reserved, set the bookingStatus field to Reserved and the ReservedExpiryDate field to the date and time that the reservation expires. If the line was not reserved, set the bookingStatus field to Declined and specify why the request was declined in the StateChangedReason field.

Supporting reserve is optional.

#####Example Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines/345233?reserve HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 458

```json
{
  "bookingStatus":"Draft",
  "comment":"Free form comment",
  "endDate":"2014-12-10T18:00:00.000Z",
  "id":"345233",
  "quantity":30000,
  "name":"My Line 1",
  "orderId":"1235872",
  "productId":"456366",
  "providerData":"cid=88873",
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
  "usesExpandables":0
}
```

####/accounts/{id}/orders/{id}/lines/{id}?cancel

#####Description

Cancels the line.

#####Rules

The user must have permissions to cancel the line. For example, advertisers and agencies may cancel Lines that they own. In addition, an agency may cancel lines that belong to the accounts that they manage on behalf of advertisers.

To cancel a line, the line must be in the Reserved, Booked, or InFlight state. If successfully canceled, set the bookingStatus field to Canceled. If the previous status was InFlight, set the StateChangedReason field as appropriate (for example, “User canceled”).

#####Example Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines/345233?cancel HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 658

```json
{
  "bookingStatus":"InFlight",
  "comment":"Free form comment",
  "cost":39.30,
  "endDate":"2014-12-10T18:00:00.000Z",
  "id":"345233",
  "quantity":30000,
  "name":"My Line 1",
  "orderId":"1235872",
  "productId":"456366",
  "providerData":"cid=88873",
  "rate":1.31,
  "rateType":"CPM",
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
  "usesExpandables":0
}
```

####/accounts/{id}/orders/{id}/lines/{id}?reset

#####Description

Moves the line back to the Draft state.

#####Rules

The user must have permissions to reset the line. For example, advertisers and agencies may reset Lines that they own. In addition, an agency may reset lines that belong to the accounts that they manage on behalf of advertisers.

To reset a line, the line must be in the Reserved, Declined, or Expired booking state. If successfully reset, set the bookingStatus field to Draft.

#####Example Request

`PATCH https://<host>/<path>/<version>/accounts/23873345/orders/1235872/lines/345233?reset HTTP/1.1`

Content-Type: application/json
AccessToken: `<OAuth token>`

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 458

```json
{
  "bookingStatus":"Declined",
  "comment":"Free form comment",
  "endDate":"2014-12-10T18:00:00.000Z",
  "id":"345233",
  "quantity":30000,
  "name":"My Line 1",
  "orderId":"1235872",
  "productId":"456366",
  "providerData":"cid=88873",
  "startDate":"2014-12-05T06:00:00.000Z",
  "stateChangeReason":"The request impressions are not available.",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
  "usesExpandables":0
}
```

####/products

#####Description

Gets the list of Products from the product catalog.

#####Rules

#####Example Request

`GET https://<host>/<path>/<version>/products HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 5899

```json
{
  "products":[
    {
      "adFormatType":["Flash", "Tag", "Image"],
      "basePrice":1.31,
      "currency":"USD",
      "deliveryType":"Guaranteed",
      "description":"A description of the product for display purposes",
      "domain":"mydomain.com",
      "estimatedDailyAvails":"Hundreds of Thousands",
      "geometry":[
        {
          "height":160
          "width":600
        }
      ]
      "httpsCompatible":0,
      "icon":"http://<domain>/<path>/icon.jpg",
      "id":"456366",
      "inventoryType":["Desktop","Tablet"],
      "languages":["EN"],
      "name":"Unique Product Name",
      "maturityLevel":"General",
      "maxDuration":30,
      "minDuration":1,
      "minSpend":30.00,
      "position":"AboveFold",
      "productTags":"Foo Bar Zoo",
      "rateType":"CPM",
      "targetTypes":["2342","3355"],
      "timeZone":"Eastern Standard Time"
      "url":"http://<domain>/<path>/creativespec.aspx"
    }
  ]
}
```

####/products/{id}

#####Description

Gets the specified Product from the product catalog.

#####Rules

#####Example Request

`GET https://<host>/<path>/<version>/products/456366 HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 5899

```json
{
  "adFormatType":["Flash", "Tag", "Image"],
  "basePrice":1.31,
  "currency":"USD",
  "deliveryType":"Guaranteed",
  "description":"A description of the product for display purposes",
  "domain":"mydomain.com",
  "estimatedDailyAvails":"Hundreds of Thousands",
  "geometry":[
    {
      "height":160
      "width":600
    }
  ]
  "httpsCompatible":0,
  "icon":"http://<domain>/<path>/icon.jpg",
  "id":"456366",
  "inventoryType":["Desktop","Tablet"],
  "languages":["EN"],
  "name":"Unique Product Name",
  "maturityLevel":"General",
  "maxDuration":30,
  "minDuration":1,
  "minSpend":30.00,
  "position":"AboveFold",
  "productTags":"Foo Bar Zoo",
  "rateType":"CPM",
  "targetTypes":["2342","3355"],
  "timeZone":"Eastern Standard Time"
  "url":"http://<domain>/<path>/creativespec.aspx"
}
```

####/tenants/{id}/products/search

#####Description

Gets a list of Products from the product catalog that matches the specified filter criteria (see ProductSearch).

#####Rules

Product selection uses a logical AND between fields and a logical OR between field values. For example, the product is select if it supports the Flash OR Image OR Text ad format, AND supports USD currency, AND specifies the foo OR bar product tag.

#####Example Request

`GET https://<host>/<path>/<version>/products/search HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

```json
{
  "adFormatType":["Tag"],
  "geometry":[
    {
      "height":160
      "width":600
    }
  ]
}
```

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 5899

```json
{
  "products":[
    {
      "adFormatType":["Flash", "Tag", "Image"],
      "basePrice":1.31,
      "currency":"USD",
      "deliveryType":"Guaranteed",
      "description":"A description of the product for display purposes",
      "domain":"mydomain.com",
      "estimatedDailyAvails":"Hundreds of Thousands",
      "geometry":[
        {
          "height":160
          "width":600
        }
      ]
      "httpsCompatible":0,
      "icon":"http://<domain>/<path>/icon.jpg",
      "id":"456366",
      "inventoryType":["Desktop","Tablet"],
      "languages":["EN"],
      "name":"Unique Product Name",
      "maturityLevel":"General",
      "maxDuration":30,
      "minDuration":1,
      "minSpend":30.00,
      "position":"AboveFold",
      "productTags":"Foo Bar Zoo",
      "rateType":"CPM",
      "targetTypes":["2342","3355"],
      "timeZone":"Eastern Standard Time"
      "url":"http://<domain>/<path>/creativespec.aspx"
    }
  ]
}
```

####/products/avails

#####Description

Gets pricing and avails information (see ProductAvails) for the specified products (see ProductAvailsSearch).

#####Rules

Only organizations that have an Approved or Limited status may search for avails.

#####Example Request

`GET https://<host>/<path>/<version>/products/avails HTTP/1.1`

Accept: application/json
AccessToken: `<OAuth token>`

```json
{
  "accountId":"23873345",
  "endDate":"2014-12-10T18:00:00.000Z",
  "frequencyCount":3,
  "frequencyInterval":"Day",
  "quantity":30000,
  "productIds":["456366"],
  "startDate":"2014-12-05T06:00:00.000Z",
  "targeting":[
    {
      "target":"Age",
      "targetValues":["18-24","25-34"]
    },
    {
      "target":"Gender",
      "targetValues":["Male"]
    }
  ]
}
```

#####Example Response

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 5899

```json
{
  "avails":[
    {
      "availability":21543,
      "currency":"USD",
      "productId":"456366",
      "price":1.26
    }
  ]
}
```
