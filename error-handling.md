##Error Handling

The publisher must support the following HTTP status codes.

| Status Code               | Description                                                                                                                                                                                                            |
|---------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200 Ok                    | Return for a successful GET, POST, PUT, or PATCH request.                                                                                                                                                              |
| 400 Bad Request           | Return for a POST, PUT or PATCH request that contains invalid data, or when the requested action (i.e. book) is not valid. <br>  The response must include the reasons for the error. For details, see Error Response. |
| 401 Unauthorized          | Return if the user is not authorized to make the request.                                                                                                                                                              |
| 404 Not found             | Return if the requested resource is not found.                                                                                                                                                                         |
| 500 Internal server error | Return for server-related errors.                                                                                                                                                                                      |

The API may support the following HTTP status codes.

| Status Code             | Description                                                                                                    |
|-------------------------|----------------------------------------------------------------------------------------------------------------|
| 302 Found               | Return if the resource has moved. The Location header must include the new URI.                                |
| 304 Not modified        | Return for requests that include the If-None-Match header (to support ETags) and the resource has not changed. |
| 412 Precondition failed | Return for requests that include the If-Match header (to support ETags) and the resource has changed.          |

###Error Response

If the request generates a 400 Bad Request status code, the response must contain a collection object; the collection object must contain a single field named errors. The value of errors is an array of one or more error objects. The following table defines the properties of the error object.

| Property     | Type                       | Required/Optional | Description                                                                                                                                  |
|--------------|----------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ErrorCode    | String                     | Required          | A symbolic string constant that identifies the error.                                                                                        |
| Context      | Dictionary<string, object> | Optional          | A list of Publisher-defined key/value pairs that provide additional context about the error. For example, an ID that identifies a log entry. |
| Link         | String                     | Optional          | A URL to additional help text that may help the caller solve the issue.                                                                      |
| ErrorMessage | String                     | Required          | A string that describes the error that occurred.                                                                                             |

The following shows the body of an example error response.

```json
{
	"errors": [
		{
		"context": {“logId”:”123abc”},
		"message": "The requested impressions are not available.",
		"errorCode": “ImpressionsNotAvailable”,
		"link": "https:\\<host>\help\impressions.aspx”
		},
		{
		"context": {},
		"message": "",
		"errorCode": “”,
		"link": ""
		},
	]
}
```

###Data Format

Supported mime type: application/json

###General Request/Response Rules

If the following is true, the response must not include the property.

  - The value is NULL
  - There is no default value
  - Its type is numeric or string

However, if the property is an array of any type and is NULL, the response must include the property and it must be set to an empty array.

All POST (add operations) and PUT/PATCH requests must include the resource in the response.

For POSTs (add operations), ignore properties that are set to NULL. However, for PUT/PATCH, if a property is set to NULL, remove the current value.
