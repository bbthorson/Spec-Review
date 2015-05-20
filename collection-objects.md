##Collection Objects

For GET calls that return a collection of resources, such as /accounts/{id}/orders, the response must be an object that contains an array of the requested resources. The array must be named according to the type of resource it contains. The following table identifies the property name that must be used for each collection call.

| Call                                                                      | Property Name | Resource      |
|---------------------------------------------------------------------------|---------------|---------------|
| /organizations <br> /organizations?$filter                                | organizations | Organization  |
| /accounts <br> /accounts?$filter                                          | accounts      | Account       |
| /accounts/{id}/assignments <br> /accounts/{id}/assignments?$filter        | assignments   | Assignment    |
| /accounts/{id}/creatives <br> /accounts/{id}/creatives?$filter            | creatives     | Creative      |
| /accounts/{id}/orders <br> /accounts/{id}/orders?$filter                  | orders        | Order         |
| /accounts/{id}/orders/{id}/lines <br> /accounts/{id}/orders/lines?$filter | lines         | Lines         |
| /products (POST) <br> /products/search (POST)                             | products      | Product       |
| /products/avails (POST)                                                   | avails        | ProductAvails |

The following shows an example response for /accounts.

```json
{
	"accounts": [
		{
			"advertiserId": “B7EBC7F3-FBB3-4250-99F1-8D001088434B”,
			"agencyId": "4AA837B7-1A27-421E-9DDD-CAEF1AE884B5",
			"id": “9B0878BE-7254-49BE-AFD4-B0A67C7C3D26”,
		},
		{
			"advertiserId": “16B55667-37CF-4447-A79D-88E6DAC4D7C2”,
			"agencyId": "4AA837B7-1A27-421E-9DDD-CAEF1AE884B5",
			"id": “EAC93F5D-F448-44D6-8333-4E530D14C9DA”,
		},
	]
}
```

The collection object may include additional publisher-defined properties.
If there are no resources to return, the array must be empty.
