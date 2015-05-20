##Workflow
The following describes the calls that a client would make to get product avails and pricing, create an order and add lines to it, upload creatives and associate them with a line, and get a performance report. For a diagram that shows the flow, see Workflow Diagram.

####Onboarding a Provider

A provider is a partner who writes the client that agencies and advertisers use to buy premium guaranteed ad inventory from the publisher. Onboarding the provider is a manual process that is publisher dependent.

####Adding an Agency Organization

Agencies sign up directly with the publisher. An agency may create one or more organizations. Each user should have their own credentials.

####Adding an Advertiser Organization

Advertisers sign up directly with the publisher. An advertiser may create one or more organizations. For example, they may create a single organization and then create accounts for each brand, subsidiary, or division. Or, they may create an organization for each brand. It is up to the advertiser to determine how they use Organization and Account to meet their organizational needs.

Each user should have their own credentials.

####Getting an OAuth 2.0 Access Token

Providers must use OAuth 2.0 to authenticate the user. Each API call requires an AccessToken header that is set to the OAuth access token.

The provider may choose to use either the implicit grant flow or authorization code grant flow depending on their usage. For one time or short term access, use the implicit grant flow. The token is short lived and will expire in minutes or seconds as determined by the authentication service. Web applications should not use the implicit flow.

For repeat or long term access, use the authorization code grant flow. The authentication service returns an access token, refresh token, and expiration time. Before the access token expires, use the refresh token to get a new access token.

####Adding an Account

An advertiser may create one or more accounts based on how they organize their buys. For example, they could create accounts for each brand, subsidiary, or division. The account identifies the advertiser and buyer. If the advertiser performs their own buys, the account would identify them as the advertiser and buyer.

If the advertiser grants an agency permission to perform buys on their behalf, the account would identify the agency as the buyer. The agency must have permissions to create accounts and perform buys on behalf of the advertiser. The process of granting an agency permission to manage an advertiser’s accounts is publisher defined.

In addition to defining the relationship between the advertiser and buyer, an account also owns orders and creatives.

To create an account, POST a request to /accounts. The body of the request is an Account resource object. The Account object contains the buyer’s ID and the advertiser’s ID. The response includes the Location header that contains the URI to the new account.

####Get Product Inventory, Availability and Pricing

The following provides several options for getting product inventory details. Typically, you’d use the first two options to present a product catalog and the last option to add and book a line.

  - To get a product catalog to display to the user, send a GET request to /products. The response includes a collection object that contains an array of Product objects. The Product object contains the product’s base rate and estimated daily impressions (for example, hundreds of thousands). Providers should not use the avails search method (option 3) to determine estimated avails.
  - To get a specific product from the catalog, send a GET request to /products/{id}. The response contains a Product object.
  - To search the product catalog, send a POST request to /products/search. The body of the request is a ProductSearch object that contains the search criteria. For example, the client may search the catalog for products that use a specific ad format. The response includes a collection object that contains an array Product objects that match the search criteria. If no products match the search criteria, the array is empty.
  - To get product availability and pricing information for specific products, send a POST request to /products/avails. You should make this call only to determine actual availability just before adding and booking a line; you should not use this call to present availability as part of a product catalog.
  - The body of the request is a ProductAvailsSearch object. The client must specify a date range, quantity, list of product IDs and may optionally specify frequency and targeting information. To get custom rates and availability for an advertiser, include the account ID, which identifies the advertiser and agency.

The response includes a collection object that contains an array of ProductAvails objects. Each ProductAvails object contains the available quantity and pricing information for a product. The number of available impressions returned will be either the specified quantity, if the requested quantity is available, or less if there is fewer quantity available.

Note that the caller should not use this call to determine the maximum available impressions. Instead, they should use /products or /products/search which returns the estimated daily availability and base pricing details. If they use the avails search for product catalog purposes, they will likely display inaccurate pricing information to the user. For example, the pricing for 500,000,000 impressions may be less than the pricing for 100,000 impressions, which may lead the user to mistakenly believe that they’re getting the impressions for  5.00 CPM instead of 15.00 CPM.

####Creating an Order

An order is the parent container for lines. To add an order, send a POST request to /accounts/{id}/orders. The body of the request is an Order object which specifies directional start and end dates, estimated budget, currency, and preferred billing method. The response includes the Location header that contains the URI to the new order.

####Adding Lines to the Order

A line specifies the ad product to book, quantity, targeting details, and a date range of when the line runs. To add a line to the order, send a POST request to /accounts/{id}/orders/{id}/lines. The body of the request is a Line resource object. Typically, the client should specify the same details on the line that were used to search for product availability.

The response includes the Location header that contains the URI to the new line. The state of the line is Draft.

The line may be updated only in the Draft state. To update a line, send a PATCH or PUT request to /accounts/{id}/orders/{id}/lines/{id}. The body of the request is either a full or partial Line resource object depending on whether the publisher supports PUT or PATCH.

####Uploading a Creative and Assigning It to a Line

To upload a creative, send a POST request to /accounts/{id}/creatives. The body of the request is a Creative resource object. The Creative object specifies the creative’s format, size, language, and the creative itself.

The creative must pass editorial review before it may be assigned to a line. To determine whether the creative passed editorial review, send a GET request to /accounts/{id}/creative/{id}. The response contains a Creative object. The creative passed editorial review if AdQualityStatus is set to Approved.

To assign the creative to a line after it passes editorial review, send a POST request to /accounts/{id}/assignments. The body of the request is an Assignment object. The Assignment object specifies the creative ID and line ID. If you assign more than one creative to a line, the creatives are rotated evenly. To control the rotation, set the optional weight property.
Note that a line must have a creative assigned to it before it may be booked.

####Reserving, Booking, and Canceling a Line

To reserve, book, or cancel a line, send a PATCH or PUT request to the following URIs, respectively.

  - /accounts/{id}/orders/{id}/lines/{id}?reserve
  - /accounts/{id}/orders/{id}/lines/{id}?book/
  - accounts/{id}/orders/{id}/lines/{id}?cancel

Each call initiates an asynchronous process to perform the work. To determine whether the request succeeded, send a GET request to /accounts/{id}/orders/{id}/lines/{id} to get the specified line. Access the BookingStatus property to verify that the status changed accordingly. For example, if the request was reserve, confirm that BookingStatus is Reserved. If the reservation or booking process failed, the status will be Declined. To determine why the request was declined, access the StateChangeReason property.

####Reporting Clicks and Impressions

See Reporting.

##Workflow Diagram

The following diagram illustrates the calls required to add an order. The diagram does not include the one time calls to get and cache reference data.

![Workflow Diagram](/img/OpenDirect_v1_workflow_diagram.gif)

##Booking State Diagram

The following diagram shows the state changes of a Line resource. For details about each state, see BookingStatus.

![Booking State Diagram](/img/OpenDirect_v1_booking_state_model.jpg)

##Resource Model

The following diagram shows the relationships between the OpenDirect resources. For details about the resource objects, see Resource.

![Resource State Diagram](/img/OpenDirect_v1_resource_state_model.gif)

##vNext

The following requirements were not included in v1 of the specification but may be addressed in a future version.

| Scenario                                                                          | Reason for postponing | Consider in version |
|-----------------------------------------------------------------------------------|-----------------------|---------------------|
| Provide visibility of ad units on a webpage.                                      |                       |                     |
| Include creative agency ID in Account.                                            |                       |                     |
| Update a creative.                                                                |                       |                     |
| Add start/end dates to Assignment to support ad rotation by date.                 |                       |                     |
| Change Line.RateType to Optional when other types are supported (default to CPM). |                       |                     |

##Scenarios

The following are the scenarios used to determine the resource model that the specification would use.

| Scenario                                                                                                                                                                                  | Example                                                                                                                                                                                                                                                                           | Priority     | Notes                        |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|------------------------------|
| An advertiser must be able to create one or more accounts and manage media buys at the account level.                                                                                     | Advertiser A has multiple brands such as Brand A and Brand B. Advertiser A must be able to logon to a provider’s tool and create an account for Brand A and an account for Brand B. Advertiser A must be able to create a media plan and buy directly without agency involvement. | Must Have    |                              |
| An advertiser must be able to logon to any provider’s tool and see all of their orders regardless of whether the advertiser or an agency created them.                                    |                                                                                                                                                                                                                                                                                   | Nice To Have |                              |
| An advertiser must be able to assign third-party service providers, such as creative agencies, to an account so they can provide creatives.                                               |                                                                                                                                                                                                                                                                                   | Nice to Have | vNext                        |
| An advertiser must be able to change the agency that manages an account without having to recreate the data.                                                                              | Currently, Agency X manages Advertiser A’s Brand B account. Advertiser A decides that they now want Agency Y to manage the Brand B account and removes Agency X and assigns Agency y.                                                                                             | Nice to Have | Cannot perform mid campaign. |
| An agency must be able to create and manage their own accounts and manage any advertisers’ accounts that they’ve been granted permissions to manage.                                      |                                                                                                                                                                                                                                                                                   | Must Have    |                              |
| An advertiser must be able to logon to any provider’s tool that they have access to and view all of their (the advertiser’s) accounts.                                                    |                                                                                                                                                                                                                                                                                   | Must Have    |                              |
| An agency must be able to logon to any provider’s tool that they have access to and view all of their (the agency’s) accounts and the accounts that they manage on behalf of advertisers. |                                                                                                                                                                                                                                                                                   | Must Have    |                              |
| A publisher must be able to identify the advertiser and agency before providing products, avails, and rates.                                                                              |                                                                                                                                                                                                                                                                                   | Must Have    |                              |
| A publisher must be able to send an invoice to the advertiser, agency, or tool provider based on the advertiser’s preference (specified at the account level).                            |                                                                                                                                                                                                                                                                                   | Must Have    |                              |
| A publisher must be able to exclude specific advertisers from websites in order to honor whitelists.                                                                                      |                                                                                                                                                                                                                                                                                   | Must Have    |                              |
| A publisher must be able to determine revenue at the account level.                                                                                                                       | Publisher A must be able to determine revenue at the account level so it can calculate sales reps’ commissions.                                                                                                                                                                   | Must Have    |                              |
| An agency must be able to create and manage campaigns for multiple advertisers through a single account (buy products in bulk and resell to advertisers).                                 |                                                                                                                                                                                                                                                                                   | Out of Scope |                              |
| An agency must be able to create and manage advertiser orders without the publisher knowing the advertiser’s identity.                                                                    |                                                                                                                                                                                                                                                                                   | Out of Scope |                              |
