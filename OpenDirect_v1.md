#OPENDIRECT
##API SPECIFICATION VERSION 1.0

##Introduction
AOL, Yahoo!, Microsoft, and Yieldex created the OpenDirect working group in June 2013 to develop and support a standard for the programmatic selling and buying of guaranteed digital ad inventory. The OpenDirect API specification will allow buyers who want to access guaranteed inventory via automated processes avoid multiple, costly, custom integrations. This document is the culmination of those efforts.

##About the Working Group
The working group consisted of Technical Analysts, Product Managers, and System Architects from AOL, Yahoo!, MicroTsoft, and Yieldex. The group was later joined by MediaMath, Bionic, and the Interactive Advertising Bureau (IAB). The working group met via weekly conference calls and quarterly in-person working sessions to finalize version 1.0 for public comment.

##Contact Information

###IAB Contact
Ramona Gonzales
ramona@iab.net

###OpenDirect Contact
Lucas Black, AOL
lucas.black@teamaol.com

##License/Intellectual Property Notice
Upon any person or entity’s request, AOL, Yahoo!, Microsoft, Yieldex, Bionic, MediaMath, and IAB (“Contributors”) agree to offer such person or entity, under such Contributor’s necessary patent claims, a no-charge, royalty free, fully paid-up, non-exclusive license under and to such Contributor’s necessary patent claims on reasonable and non-discriminatory terms for purposes of implementing any this specification. Such license may be subject to the condition of reciprocity by the licensee with respect to, among other things, a license to be granted by such licensee to such party with respect to such licensee’s necessary patent claims and other reasonable and nondiscriminatory terms.

THE CONTRIBUTIONS AND SPECIFICATION ARE PROVIDED "AS IS." THE ENTIRE RISK AND LIABILITY WITH RESPECT TO THE IMPLEMENTATION OR ANY OTHER USE OR EXPLOITATION OF ANY CONTRIBUTION, DRAFT SPECIFICATION OR FINAL SPECIFICATION ARE ASSUMED BY THE IMPLEMENTER, USER AND EXPLOITER. EACH CONTRIBUTOR EXPRESSLY DISCLAIMS ANY WARRANTIES (EXPRESS, IMPLIED, OR OTHERWISE), INCLUDING, WITHOUT LIMITATION, IMPLIED WARRANTIES OF MERCHANTABILITY, NON-INFRINGEMENT, FITNESS FOR A PARTICULAR PURPOSE, OR TITLE, RELATED TO ANY CONTRIBUTION, DRAFT SPECIFICATION OR FINAL SPECIFICATION. IN NO EVENT WILL ANY CONTRIBUTOR BE LIABLE TO ANY OTHER CONTRIBUTOR, PERSON OR ENTITY FOR ANY LOST PROFITS OR ANY FORM OF INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES OF ANY CHARACTER FROM ANY CAUSES OF ACTION OF ANY KIND WITH RESPECT TO THIS AGREEMENT OR ANY SUBJECT MATTER OF THIS AGREEMENT, WHETHER BASED ON BREACH OF CONTRACT, TORT (INCLUDING NEGLIGENCE), OR OTHERWISE, WHETHER OR NOT ANY CONTRIBUTOR, PERSON OR ENTITY HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE, AND EVEN IF THE REMEDIES PROVIDED FOR IN THIS AGREEMENT FAIL OF THEIR ESSENTIAL PURPOSE.

##Getting Started
The OpenDirect specification defines the API that publishers should implement to support programmatic direct buying of their premium guaranteed ad inventory. Implementing the API lets buyers write a single client that can access inventory from multiple publishers without the need to write custom code for each integration. Implementing the API would make integrating with a publisher more attractive.

Version 1 of the specification provides the basic features required to support programmatic buying of guaranteed inventory. For example, the API supports getting and searching product inventory, determining pricing and availability, applying targeting and frequency constraints, creating orders, adding lines to orders, uploading creative, assigning creatives to lines, and reserving and booking inventory.

The API is a RESTful API that supports JSON and uses OAuth to authenticate users.

The following should be noted:

   - The API takes an advertiser first approach. Both advertisers and agencies must sign up with the publisher and receive credentials. Advertisers may perform their own buys or provide consent to have an agency perform buys on their behalf; for an agency to perform buys on behalf of an advertiser, the advertiser must provide consent. The process of signing up and providing consent is publisher defined.
   - To book a line, the line must have a creative assigned to it. The creative may be the actual creative that the advertiser plans to run or a placeholder creative that is later replaced with the actual creative when it becomes available. However, the line will run with whichever creative is assigned to it (the actual creative or placeholder creative).

For details about the programming elements that this specification defines, see the following sections.

|    |       |
|----|-------|
| [Resources](#resources) | Defines the resource objects. For a diagram that shows the resource relationships, see Resource Model. |
| [Common Objects](#common-objects) | Defines the objects used by one or more resource. |
| [Reference Data](#reference-data)  | Defines the reference data objects and possible values. Reference data provides enumerated values for a resource property. |
| [Collection Objects](#collection-objects) | Defines the collection objects that GET calls return. |
| [URIs](#uris) | Defines the URI and supported HTTP verbs for each resource.  |
| [Authentication](#authentication) | Defines the authentication scheme that publisher must use. |
| [Versioning](#versioning) | Defines the versioning scheme that publishers must use. |
| [Error Handling](#error-handling) | Defines the error objects that publishers must return for 400 Bad Request. |
| [Reporting](#reporting) | Defines the reporting URIs and objects. |
| [Workflow](#workflow) | Outlines the calls required to create an order. |

---

##Resources
The OpenDirect API is a RESTful API that supports JSON. This section defines the JSON resource objects used by the API. For a diagram that shows the relationships between these resources, see Resource Model.

For a list of URIs that use these resources, see URIs.

###Account
Defines an Account resource. An Account associates an advertiser with a buyer. A buyer is typically an agency but may also be the advertiser. A buyer may be associated with one or more advertisers and an advertiser may be associated with one or more buyers.

Before an agency may create accounts and perform buys on behalf of the advertiser, the advertiser must give permissions to the agency. The process of giving or removing permissions is publisher defined. Creating an account must fail if the advertiser has not given the agency permissions.

The Account owns the orders and creatives.

| Property     | Type   | Constraints                  | Add       | Update    | Publisher Requirement | Description                                                                                                                                                                                                                                                                                                                                          |
|--------------|--------|------------------------------|-----------|-----------|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AdvertiserId | String | Maximum of 36 characters.    | Required  | Read-only | Must support          | An ID that identifies the organization that is acting as the advertiser.                                                                                                                                                                                                                                                                             |
| BuyerId      | String | Maximum of 36 characters.    | Required  | Read-only | Must support          | An ID that identifies the organization that is acting as the buyer. If the advertiser is performing their own buys, AdvertiserId and BuyerId must be the same.
| Id           | String | Maximum of 36 characters.    | Read-only | Read-only | Must support          | A system-generated opaque ID that uniquely identifies this resource.                                                                                                                                                                                                                                                                                 |
| Name         | String | Maximum of 255 characters    | Required  | Optional  | Must support          | The name of the account. Used for display purposes.                                                                                                                                                                                                                                                                                                  |
| ProviderData | String | Maximum of 1,000 characters. | Optional  | Optional  | May support           | An opaque blob of provider-defined data. Providers may use this field as needed (for example, to store an ID that correlates this object with resources within their system). Note that any provider that edits this object may override the data in this field. The data should include a marker that you can identify to ensure the data is yours. |

Notes: A organization may organize accounts by brand, subsidiaries, divisions, or anything else that supports their model.

###Assignment
Defines an Assignment resource. An Assignment associates a creative with a line of the order.

A creative may be assigned to one or more lines and a line may be assigned one or more creatives.

| Property     | Type   | Constraints                       | Add       | Update    | Publisher Requirement | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------|--------|-----------------------------------|-----------|-----------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CreativeId   | String | Maximum of 36 characters.         | Required  | Read-only | Must support          | The ID of the creative to display when the line runs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Id           | String | Maximum of 36 characters.         | Read-only | Read-only | Must support          | A system-generated opaque ID that uniquely identifies this resource.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| LineId       | String | Maximum of 36 characters.         | Required  | Read-only | Must support          | The ID of the line that will display the creative.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ProviderData | String | Maximum of 1,000 characters.      | Optional  | Optional  | May support           | An opaque blob of provider-defined data. Providers may use this field as needed (for example, to store an ID that correlates this object with resources within their system). Note that any provider that edits this object may override the data in this field. The data should include a marker that you can identify to ensure the data is yours.                                                                                                                                                                                                                                                                                                                        |
| Status       | String |                                   | Read-only | Read-only | Must support          | A value that determines whether the creative serves. The following are the possible values. • Active—The creative may serve. Set at create time. • Inactive—The creative may not serve. Set by the disable verb. The status may not transition from Inactive to Active.                                                                                                                                                                                                                                                                                                                                                                                                     |
| Weight       | Byte   | Numeric value from 1 through 100. | Optional  | Optional  | Should support        | Determines how much the creative is displayed relative to the other creatives assigned to the same line. To provide even rotation, do not specify a weight. If weight is specified, all assignments that specify the same line must specify a weight and the weight of all the assignments must add up to 100.  If the weight of all assignments does not add up to 100, even rotation is applied. Assignments with heavier weight get proportionally more rotation compared to those with lesser weight. For example, if the line has 2 creatives, A and B, assigned with the same dates, and A has weight 25 and B has weight 75, B will serve three times as often as A. |

Notes: The assignment must fail if the following are true.

  - The language of the creative does not match the language of the product (the line identifies the product).
  - The specified maturity level of the creative does not match the maturity level of the product specified by the line.

A creative must be assigned to a line before the line may be booked.

To change the creative assigned to a line, first assign a new creative to the line to ensure that the line continues to deliver and then use the disable verb to set the current assignment to Inactive.

To display different creatives at different times, add a line for each creative. If weighting is used, providers should indicate in the user interface whether all assignments for a line specify a weight and that the sum of all weights is 100.

###Creative
Defines a Creative resource. The Creative provides information about an ad.

To assign a creative to a line, see Assignment.

| Property     | Type   | Constraints               | Add      | Update    | Publisher Requirement | Description                                                                                                                                 |
|--------------|--------|---------------------------|----------|-----------|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| AccountId    | String | Maximum of 36 characters. | Required | Read-only | Must support          | The ID of the account that owns the creative.                                                                                               |
| AdFormatType | String |                           | Required | Read-only | Must support          | The ad’s format. For a list of possible values, see AdFormatType. The product that the line specifies must support the specified ad format. |
| AdQualityRejectionReason | String |   | Read-only | Read-only | Must support | The reason why the creative audit did not approve the creative.                                                                                                                                                                                                                                                                                                                                       |
| AdQualityStatus          | String |   | Read-only | Read-only | Must support | A status value that indicates where in the audit process the creative is. The following are the possible values. Pending – The creative was submitted and is either waiting for review or is in the process of being reviewed. Approved – The Creative passed the review. Rejected – The creative failed the review. The AdQualityRejectionReason field contains the reason why it failed the review. |
| BackupFlashAsset | String |   | Optional | Read-only | Should support | A base64 string that contains the backup Image in case the user’s browser does not support Flash. The image must be of one of the following mime types. • GIF • JPEG • PNG The CreativeAsset property contains the Flash creative. The publisher’s documentation should indicate any size constraints. If the asset exceeds the constraint, the publisher must return error code, BackupCreativeTooLarge. |
| ClickUrl | String |   | Required | Optional | Must support | The URL of a webpage that the user is taken to if they click the ad. |
| CreativeAsset | String |   | Required | Read-only | Must support | A string that contains the creative. The AdFormatType determines whether the string is a character string or a base64 string. Image and Flash creatives, must use base64 strings and all others (tags, text, and video) use character strings.If the creative is an image, it must be of one of the following mime types. |
| Geometry           | Size    |                             | Required  | Read-only | Must support   | The size of the creative.                                                                                                                                                                                                                                                                                                                            |
| HttpsCompatible    | Boolean |                             | Optional  | Optional  | Should support | A Boolean value that determines whether the creative can,properly render on an HTML web page served over HTTPS. Defaults to False.                                                                                                                                                                                                                   |
| Id                 | String  |                             | Read-only | Read-only | Must support   | A system-generated opaque ID that uniquely identifies this resource.                                                                                                                                                                                                                                                                                 |
| Language           | String  |                             | Required  | Optional  | Must support   | The ISO 639-1 language code that identifies the language,used in the ad. For example, if the ad uses English, the ISO code would be EN.                                                                                                                                                                                                              |
| MaturityLevel,,,,, | String  |                             | Optional  | Optional  | May support    | The maturity level of the creative content. The following are the possible values. · Children · General · Mature At assignment time, the assignment must be rejected if the specified maturity level does not match the maturity level of the product specified by the line. The default is General.                                                 |
| Name               | String  |                             | Required  | Optional  | Must support   | The display name of the creative.                                                                                                                                                                                                                                                                                                                    |
| ProviderData       | String  | Maximum of 1,000 characters | Optional  | Optional  | May support    | An opaque blob of provider-defined data. Providers may use this field as needed (for example, to store an ID that correlates this object with resources within their system). Note that any provider that edits this object may override the data in this field. The data should include a marker that you can identify to ensure the data is yours. |

Notes: Updates to the creative are not allowed for V1.

###Line
Defines a Line resource. A Line specifies the ad product to book, the quantity, and when the line runs.

For information about assigning creatives to a line, see Assignment.

| Property           | Type      | Constraints                 | Add       | Update    | Publisher Requirement | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------|-----------|-----------------------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BookingStatus      | String    |                             | Read-only | Read-only | Must support          | A value that determines whether the line is booked and is capable of delivering ads. For a state diagram, see Booking State Diagram. The following are the possible booking status values.•,Draft—Indicates that a draft of the line has been saved. The line may be updated only in this state.The line remains in this state until the user deletes, reserves, or books the line.•,PendingReservation—Indicates that the reservation is in progress.If approved, the state moves to Reserved; otherwise, it moves to Declined.Any user action requested in this state must fail.•,Reserved—Indicates that the inventory for the line has been reserved. Remains in this state until the user cancels, books, resets the line or the reservation expires. The ability to reserve inventory is optional. Each publisher determines the length of time that inventory may be reserved without booking before it’s released. If the line is reserved, the ReservedExpiryDate must be set to the date and time that the reserved inventory will expire.•,PendingBooking—Indicates that the booking is in progress.If approved, the state moves to Booked; otherwise, it moves to Declined.Any user action requested in this state must fail.•,Booked—Indicates that the line is booked and the buyer is obligated to the terms. To book the line, the line must have a creative assigned to it. If a creative is not assigned, the booking must fail. The line stays in this state until the user cancels the line or the line reaches its delivery window.After the line reaches its delivery window, the line moves to the InFlight state. •,InFlight—Indicates that the line is in its delivery window.The line stays in this state until the user cancels the line or the line reaches the end of its delivery window. If the line reaches the end of its delivery window, then it moves to the Finished state; otherwise, it moves to the Stopped state.•,Finished—Indicates that the line successfully completed its flight. The line remains in this state.•,Stopped—Indicates that the user or publisher canceled the line while it was in-flight.The StateChangeReason field must specify the reason why the flight was canceled.The line remains in this state.•,Canceled—Indicates that the user canceled the line while it was in the Reserved or Booked state. The line remains in this state.•,Expired—Indicates that the reservation expired. The line remains in this state unless the user resets the line, which moves it back to the Draft state•,Declined—Indicates that booking or reservation was declined by the publisher or failed. The line remains in this state unless the user resets the line, which moves it back to the Draft state.The StateChangeReason field must specify the reason why the booking or reservation was declined or failed.If the line is reset, the StateChangeReason should be cleared. |
| Comment            | String    | Maximum of 256 characters.  | Optional  | Optional  | May support           | User notes related to this line.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Cost               | Decimal   |                             | Read-only | Read-only | Must support          | The projected cost of the line based on the specified quantity, rate and targeting. The actual cost (the amount billed) will be based on the actual number of impressions.The cost is specified in the currency specified at the order level. If the product specifies a different currency, the cost must be converted to the order’s currency.The cost is determined at the time the line is saved (added, updated, booked, or reserved).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| EndDate            | String    | Maximum of 26 characters.   | Required  | Optional  | Must support          | The date and time that the line will stop. The date and time must be specified in UTC and conform to ISO 8601.If the time is missing, 11:59 PM is assumed.The end date must be later than the start date and should be less than or equal to the order’s end date. If the end date is later than the order’s end date, the order’s end date should be extended to match the line’s end date.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| FrequencyCount     | Byte      |                             | Optional  | Optional  | Should support        | The maximum number of times that a unique user must see ads from this line during the specified interval (see FrequencyInterval).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| FrequencyInterval  | String    | Maximum of 5 characters.    | Optional  | Optional  | Should support        | The interval that FrequencyCount applies to. For example, per day or per week.For a list of possible intervals, see FrequencyCapInterval.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Id                 | String    | Maximum of 36 characters.   | Read-only | Read-only | Must support          | A system-generated opaque ID that uniquely identifies this resource.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Quantity           | Long      |                             | Optional  | Optional  | Must support          | The quantity requested for the specified date range. This value will differ based on various cost types. For CPM, for examples, the value would be impressions.The line must contain a quantity before the user may reserve or book it. If the requested quantity is not available, reserving or booking the line must fail and bookingStatus must be set to Declined.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Name               | String    | Maximum of 200 characters.  | Required  | Optional  | Must support          | The line’s display name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Should be unique.  |           |                             |           |           |                       | The ID of the product where the creatives run. An opaque blob of provider-defined data. Providers may use this field as needed (for example, to store an ID that correlates this object with resources within their system).Note that any provider that edits this object may override the data in this field. The data should include a marker that you can identify to ensure the data is yours.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| OrderId            | String    | Maximum of 36 characters.   | Read-only | Read-only | Must support          | The ID of the order that this line belongs to.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ProductId          | String    | Maximum of 36 characters.   | Required  | Read-only | Must support          | The date and time that the line will start.,The date and time must be specified in UTC and conform to ISO 8601. If the time is missing, 12:00 AM is assumed.The date and time must be greater than or equal to now and should be greater than or equal to the order’s start date. If the start date is earlier than the order’s start date, the order’s start date should be moved to match the line’s start date if the order’s start date has not past.Start dates that have past may not be updated.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ProviderData       | String    | Maximum of 1,000 characters | Optional  | Optional  | May support           | The reason why the state was changed by the publisher. The reason must be specified if:•,The publisher declined the booking or reservation.•,The publisher or user canceled the flight.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Rate               | Decimal   |                             | Read-only | Read-only | Must support          | The price per unit of impressions. For example, $10 per 1,000 impressions (CPM).  <br> The rate is determined each time the line is saved (added, updated, booked, or reserved).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| RateType           | String    | Maximum of 10 characters.   | Read-only | Read-only | Must support          | The unit of measure that Rate is expressed in. For a list of possible values, see RateType. <br> The rate type is determined at the time the line is saved (added, updated, booked, or reserved).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ReservedExpiryDate | String    | Maximum of 26 characters.   | Read-only | Read-only | Should support        | The date and time that the reserved inventory will expire. <br> If the line is reserved, the expiry date must be set.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| StartDate          | String    | Maximum of 26 characters.   | Required  | Optional  | Must support          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| StateChangeReason  | String    |                             | Read-only | Read-only | Must support          | The reason why the state was changed by the publisher.,The reason must be specified if:,·,The publisher declined the booking or reservation.,The publisher or user canceled the flight.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Targeting          | Segment[] |                             | Optional  | Optional  | Should support        | The segments used to target users and determine product,availability. For example, behavioral, age, and gender segments.,If the line includes user segments and the delivery engine,can determine whether the user matches the specified segments, it will,display the ad to the user; otherwise it will not.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| UsesExpandables    | Boolean   |                             | Optional  | Optional  | Should support        | A Boolean value that indicates whether the line will be,assigned expandable creatives. Used to determine availability.,The default is false.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

###Order
Defines an Order resource. The Order specifies the plan’s start and end dates, estimated budget, currency, and preferred billing method.

To specify the details of the order, use the Line resource.

| Property               | Type      | Constraints                                                                        | Add       | Update    | Publisher Requirement | Description                                                                                                                                                                                                                                                                                                                                                                               |
|------------------------|-----------|------------------------------------------------------------------------------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AccountId              | String    | Maximum of 36 characters.                                                          | Read-only | Read-only | Must support          | The ID of the account that identifies the advertiser and buyer that own the order.                                                                                                                                                                                                                                                                                                        |
| Brand                  | String    | Maximum of 25 characters.                                                          | Optional  | Optional  | May support           | The brand being advertised.                                                                                                                                                                                                                                                                                                                                                               |
| Budget                 | Decimal   |                                                                                    | Optional  | Optional  | Should support        | The order’s estimated budget. The budget is directional; it is not used to limit the amount of money that the order spends. To determine the projected spend based on quantity, aggregate the Cost property for each line of the order.                                                                                                                                                   |
| Contacts               | Contact[] | The list must contain unique contact types (for example only one billing contact). | Optional  | Optional  | Should support        | The list of contacts to use for this order. This list of contacts is in addition to the buyer’s and advertiser’s list of contacts.                                                                                                                                                                                                                                                        |
| Currency               | String    |                                                                                    | Required  | Optional  | Must support          | The currency that all monetary properties of the order and lines are specified in. The currency is also used for billing and reporting. For a list of possible currency ISO codes, see Currency.,The publisher may enforce that all lines of the order specify products that use the same currency.                                                                                       |
| EndDate                | String    | Maximum of 26 characters.                                                          | Optional  | Optional  | Should support        | The date and time that the order will end. The end date is directional and may be updated by the publisher to match the latest end date found in the order’s lines.<br>The date and time must be specified in UTC and conform to ISO 8601.<br>If the time is missing, 11:59 PM is assumed.,The end date must be later than the start date.<br>End dates that have past cannot be updated. |
| Id                     | String    | Maximum of 36 characters.                                                          | Read-only | Read-only | Must support          | A system-generated opaque ID that uniquely identifies this,resource.                                                                                                                                                                                                                                                                                                                      |
| Industry               | String    |                                                                                    | Optional  | Optional  | May support           | The industry associated with the order. This industry may,differ from the industry specified on the advertiser’s Organization object. For possible industries, see Industry.                                                                                                                                                                                                              |
| Name                   | String    | Maximum of 100 characters.                                                         | Required  | Optional  | Must support          | The order’s display name.,Must be unique within the account’s list of campaigns.                                                                                                                                                                                                                                                                                                          |
| PreferredBillingMethod | String    | Maximum of 10 characters                                                           | Optional  | Optional  | Should support        | The preferred billing method for this order. The following,are the possible values.,Electronic—The invoice is sent to the billing contact’s,email address.,Postal—The invoice is sent to the billing contact’s postal,address.,The default is Electronic.,If the billing contact is not specified in the order, the,billing contact comes from buyer’s list of contacts.                  |
| ProviderData           | String    | Maximum of 1000 characters                                                         | Optional  | Optional  | May support           | An opaque blob of provider-defined data. Providers may use,this field as needed (for example, to store an ID that correlates this object,with resources within their system).,Note that any provider that edits this object may override,the data in this field. The data should include a marker that you can,identify to ensure the data is yours.                                      |
| StartDate              | String    | Maximum of 26 characters.                                                          | Optional  | Optional  | Should support        |                                                                                                                                                                                                                                                                                                                                                                                           |

###Organization
Defines an organization resource. The organization resource may represent an advertiser or agency (buyer). The Account determines the role that the organization plays. The organization’s role may vary by account. For example, the organization may be an advertiser in one account and a buyer in another.

| Property          | Type      | Constraints                                                                                                          | Add       | Update    | Publisher Requirement | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------|-----------|-----------|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Address           | Address   |                                                                                                                      | Optional  | Optional  | Should support        | The organization’s corporate headquarters address.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Contacts          | Contact[] | The list must contain unique contact types (for example only one billing contact).<br>A billing contact is required. | Required  | Optional  | Must support          | A list of one or more contacts within the organization.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| DisapprovalReason | String    | Maximum of 256 characters.                                                                                           | Read-only | Read-only | Must support          | The reason why the organization was not registered.,Must be specified if Status is Disapproved.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Fax               | String    | Maximum of 20 characters.                                                                                            | Optional  | Optional  | May support           | The organization’s fax number.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Id                | String    | Maximum of 36 characters.                                                                                            | Read-only | Read-only | Must support          | A system-generated opaque ID that uniquely identifies this resource.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Industry          | String    |                                                                                                                      | Optional  | Optional  | May support           | The industry that the organization belongs to. For possible industries, see Industry.,Required for advertiser organization only.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Name              | String    | Maximum of 128 characters.<br>Cannot be an empty string. Must be unique.                                             | Required  | Optional  | Must support          | The organization’s display name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Phone             | String    | Maximum of 20 characters.                                                                                            | Optional  | Optional  | Should support        | The organization’s phone number.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ProviderData      | String    | Maximum of 1000 characters.                                                                                          | Optional  | Optional  | May support           | An opaque blob of provider-defined data. Providers may use this field as needed (for example, to store an ID that correlates this object with resources within their system).,Note that any provider that edits this object may override the data in this field. The data should include a marker that you can identify to ensure the data is yours.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Status            | String    | Maximum of 15 characters.                                                                                            | Read-only | Read-only | Must support          | A value that indicates the current state of the approval process. The approval process confirms the organization’s identity. The following are the possible values.,·,Pending—The organization is under review.,·,Approved—The organization is approved and can create and book campaigns.,·,Disapproved—The organization’s identity could not be verified. The organization may not create and book campaigns. The DisapprovalReason property must specify the reason why the organization was not approved.,·,Limited—The organization’s identity could not be verified; however, they may create and book campaigns.,This state may affect the products and pricing offered to the organization.,The organization may create campaigns in any state (except where noted); however, they may search for available inventory or reserve and book inventory only in the Approved and Limited states. |
| Url               | String    | Maximum of 1024 characters.                                                                                          | Optional  | Optional  | Should support        | A URL to the organization’s website.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

Notes: An advertiser may create one or more organizations to meet their business needs. For example, they may create a single organization and then create accounts for each brand, subsidiary, or division. Or, they may create an organization for each brand. It is up to the advertiser to determine how they use Organization and Account to meet their organizational needs.

###Product
Defines a Product resource. A Product identifies anything from an ad placement to a Run of Network product in the publisher’s product catalog.

| Property                 | Type     | Constraints                                                                                   | Publisher Requirement | Description                                                                                                                                                  |
|--------------------------|----------|-----------------------------------------------------------------------------------------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ActiveDate               | String   | Maximum of 26 characters.                                                                     | Should support        | The date and time, in UTC, that the product may become part of the bookable inventory.                                                                       |
| AdFormatTypes            | String[] |                                                                                               | Must support          | A list of ad types that the product supports. For a list of possible values, see AdFormatType.                                                               |
| BasePrice                | Decimal  |                                                                                               | Must support          | The product’s base retail price; this is not the rate card price. The actual price may be more if targeting is specified.                                    |
| Currency                 | String   |                                                                                               | Must support          | The currency that the BasePrice and MinSpend properties are specified in. For a list of possible currencies, see Currency.                                   |
| DeliveryType             | String   | Maximum of 10 characters.                                                                     | Should support        | The type of delivery. For example, exclusive or guaranteed. For a list of possible values, see DeliveryType.                                                 |
| Description              | String   | Maximum of 256 characters.                                                                    | May support           | The product’s description.                                                                                                                                   |
| Domain                   | String   | Maximum of ?? characters.                                                                     | Should support        | The product’s domain. For example, yahoo.com.                                                                                                                |
| EstimatedDailyAvails     | String   |                                                                                               | Should support        | An estimated range of available daily impressions. The ranges should be of the form: Thousands, Tens of Thousands, Hundreds of Thousands, and so on.         |
| Geometry                 | Size[]   |                                                                                               | Must support          | A list of ad format sizes that the product supports.                                                                                                         |
| HttpsCompatible          | Boolean  |                                                                                               | Should support        | A Boolean value that determines whether the product supports creatives that can properly render on an HTML web page served over HTTPS.                       |
| Icon                     | String   | Publishers should support icons that are 150x150 or less. The maximum size is 10 KB.          | May support           | URL to a thumbnail icon of the product. May be used to display next to the product in the product catalog.                                                   |
| Id                       | String   | Maximum of 36 characters.                                                                     | Must support          | A system-generated opaque ID that uniquely identifies this resource.                                                                                         |
| InventoryType            | String[] |                                                                                               | Should support        | A list of devices that the product may serve on. For a list of possible values, see Inventorytype. The default is Desktop.                                   |
| Languages                | String[] |                                                                                               | May support           | A list of creative languages that the product supports.                                                                                                      |
| LeadTime                 | Short    |                                                                                               | May support           | The number of days (n) from today that a line that reference this product can begin running; the line’s start date must be equal to or later than today + n. |
| Name                     | String   | Maximum of 38 characters.                                                                     | Must support          | The product’s display name.                                                                                                                                  |
| The name must be unique. |          |                                                                                               |                       |                                                                                                                                                              |
| MaturityLevel            | String   |                                                                                               | May support           | The maturity level of the publisher’s content. For a list of possible values, see MaturityLevel.                                                             |
| MaxDuration              | Short    |                                                                                               | Should support        | The maximum number of days that the product may be booked for. The line must enforce the duration.                                                           |
| MinDuration              | Short    |                                                                                               | Should support        | The minimum number of days that the product must be booked for. The line must enforce the duration.                                                          |
| MinSpend                 | Decimal  |                                                                                               | Should support        | The minimum amount of money that must be spent on this product in order to book it.                                                                          |
| Position                 | Byte     |                                                                                               | Should support        | The position of the ad as a relative measure of visibility or prominence. For a list of possible values, see AdPosition.                                     |
| ProductTags              | String[] | The list may contain a maximum of 500 tags. Each tag may contain a maximum of 100 characters. | May                   | List of tags used for searching the product catalog.                                                                                                         |
| RateType                 | String   |                                                                                               | Must support          | The unit of measure that BasePrice is expressed in. For a list of possible values, see RateType.                                                             |
| RetirementDate           | String   | Maximum of 26 characters.                                                                     | Should support        | The date and time, in UTC, that the product may be removed from the bookable inventory.                                                                      |
| TargetTypes              | String[] |                                                                                               | Should support        | A list of IDs that identify the types of targeting that the product supports. For example, DMA or Gender.                                                    |
| TimeZone                 | String   |                                                                                               | Should support        | The time zone that the product runs in.                                                                                                                      |
| Url                      | String   |                                                                                               | Should support        | A URL to the specification that describes the creative requirements.                                                                                        |

##Common Objects

The following objects are common to one or more resources.

###Address

Defines a postal address.

| Property     | Type   | Constraint                                                                                      | Add      | Update   | Publisher Requirement | Description                     |
|--------------|--------|-------------------------------------------------------------------------------------------------|----------|----------|-----------------------|---------------------------------|
| City         | String | Maximum of 35 alpha characters. Cannot be an empty string.                                      | Required | Optional | Must support          | The city.                       |
| Country      | String | Maximum of 2 alpha characters. Must be a valid ISO 3166-1 country code.                         | Required | Optional | Must support          | The country/region.             |
| AddressLine1 | String | Maximum of 255 alphanumeric characters. Cannot be an empty string.                              | Required | Optional | Must support          | The first line of the address.  |
| AddressLine2 | String | Maximum of 255 alphanumeric characters.                                                         | Optional | Optional | Must support          | The second line of the address. |
| PostalCode   | String | Maximum of 15 alphanumeric characters. Can include a dash and space. Cannot be an empty string. | Optional | Optional | Must support          | The postal or ZIP code.         |
| State        | String | Maximum of 35 alpha characters.  Cannot be an empty string.                                     | Optional | Optional | Must support          | The state or province.          |

###Contact
Defines an agency or advertiser contact.

| Property  | Type    | Constraints                                                   | Add      | Update    | Publisher Requirement | Description                                                                                                                           |
|-----------|---------|---------------------------------------------------------------|----------|-----------|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Address   | Address |                                                               | Optional | Optional  | May support           | The contact’s address.<br>Required if Type is Billing and the organization’s or order’s preferred billing method is paper.            |
| Email     | String  | Maximum of 254 characters.                                    | Optional | Optional  | Must support          | The contact’s email address.<br>Required if Type is Billing and the organization’s or order’s preferred billing method is electronic. |
| Honorific | String  | Maximum of 20 characters.                                     | Optional | Optional  | May support           | Honorific such as Mr. or Ms.                                                                                                          |
| Fax       | String  | Maximum of 20 characters.                                     | Optional | Optional  | May support           | The contact’s fax number.                                                                                                             |
| FirstName | String  | Maximum of 20 characters.                                     | Required | Optional  | Must support          | The contact’s first name.                                                                                                             |
| LastName  | String  | Maximum of 20 characters.                                     | Required | Optional  | Must support          | The contact’s last name.                                                                                                              |
| Phone     | String  | Maximum of 20 characters.                                     | Optional | Optional  | Must support          | The contact’s phone number.                                                                                                           |
| Title     | String  | Maximum of 30 characters                                      | Optional | Optional  | Must support          | The contact’s job title.                                                                                                              |
| Type      | String  | Maximum of 10 characters.<br> The string is case insensitive. | Required | Read-only | Must support          | The type of contact that this resource represents. For a list of possible values see ContactType.                                     |

###ProductAvails
Defines the availability and pricing information that a product availability search request returns.

| Property     | Type    | Publisher Requirement | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------|---------|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Availability | Long    | Must support          | The quantity that is available to book. <br>The number must be equal to or less than the Quantity specified in ProductAvailsSearch. For example, if Quantity is set to 500,000 and there are 500,000 impressions available, Availability must be set to 500,000. However, if there are only 250,000 impressions available, Availability must be set to 250,000. If there are no impressions available, Availability must be set to 0.<br>Note that publishers may set an artificial limit on the maximum number of available impressions.<br>This is the number of available impressions over the date range; the availability on a given date within the range may vary. |
| Currency     | String  | Must Support          | The currency that Price is specified in. The product determines the currency.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ProductId    | String  | Must support          | A system-generated opaque ID that uniquely identifies the product.<br>The ID is one of the product IDs specified in ProductAvailsSearch (see ProductIds).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Price        | Decimal | Must support          | The product’s price per unit. The product’s rate type determines the unit. For example, if RateType is CPM, the price is per 1,000 impressions.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

###ProductAvailsSearch
Defines the search criteria used to search for product availability and pricing information.

| Property          | Type      | Publisher Requirement | Description                                                                                                                                                                                                                                      |
|-------------------|-----------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AccountId         | String    | Should support        | The ID of the account that identifies the agency and advertiser. If not specified the pricing information is based on the product’s base rate.                                                                                                   |
| EndDate           | String    | Must support          | The end date of the delivery window. The date and time must be specified in UTC and must be later than StartDate.                                                                                                                                |
| FrequencyCount    | Byte      | Should support        | The maximum number of times that a unique user must see ads during the specified interval (see FrequencyInterval).<br>This field must be specified if FrequencyInterval is specified.                                                            |
| FrequencyInterval | String    | Should support        | The interval that FrequencyCount applies to. For example, per day or per week.<br>For a list of possible intervals see FrequencyCapInterval.<br>This field must be specified if FrequencyCount is specified.                                     |
| Quantity          | Long      | Must support          | The quantity requested for the specified date range. This value will differ based on various cost types. For CPM, for examples, the value would be impressions.<br>The maximum quantity that may be specified is publisher dependent.            |
| ProductIds        | String[]  | Must support          | A list of IDs that identify the products to get availability and pricing information for.<br>The maximum number of IDs that can be specified is publisher dependent.<br>The date range, quantity, and targeting apply to all specified products. |
| StartDate         | String    | Must support          | The start date of the delivery window. The date and time must be specified in UTC and must be later than now.                                                                                                                                    |
| Targeting         | Segment[] | Should support        | The segments to target. For example, behavioral, age, and gender segments.                                                                                                                                                                       |

###ProductSearch
Defines the search criteria used to search the product catalog.

| Property      | Type     | Publisher Requirement | Description                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|---------------|----------|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AdFormatTypes | String[] | Must support          | One or more ad types. Return products that support one or more of the specified formats. <br> For a list of possible values see AdFormatType.                                                                                                                                                                                                                                                                                                   |
| Currency      | String   | Must support          | The currency that the product supports. <br> Return products that support the specified currency. <br> For a list of possible currency ISO codes see Currency.                                                                                                                                                                                                                                                                                  |
| DeliveryType  | String   | Must support          | The delivery type (for example, Guaranteed). For a list of possible values see DeliveryType.                                                                                                                                                                                                                                                                                                                                                    |
| Domain        | String   | Must support          | The product’s domain. For example yahoo.com.                                                                                                                                                                                                                                                                                                                                                                                                    |
| Geometry      | Size[]   | Must support          | One or more ad sizes. Return products that support one or more of the specified sizes.                                                                                                                                                                                                                                                                                                                                                          |
| ProductTags   | String[] | May support           | One or more tags. <br> Return products that have product tags that exactly match one or more of the specified tags. <br> A match occurs if the specified tag exactly matches the product’s tag (using a case insensitive comparison). For example, the product is selected if the specified search tag is Travel and the product includes a Travel tag. However if the product includes only a European Travel tag the product is not selected. |

Note that product selection uses a logical AND between fields and a logical OR between field values. For example, the product is select if it supports the Flash OR Image OR Text ad format, AND supports USD currency, AND specifies the foo OR bar product tag.

At least one field must be specified.

###Size
Defines the geometry of a creative.

| Property | Type  | Constraints | Add      | Update   | Publisher Requirement | Description                           |
|----------|-------|-------------|----------|----------|-----------------------|---------------------------------------|
| Height   | Short |             | Required | Optional | Must support          | The height of the creative in pixels. |
| Width    | Short |             | Required | Optional | Must support          | The width of the creative in pixels.  |

###Segment
Defines the target and target values used to search for product availability and to specify targeting for a line.

| Property     | Type     | Constraints | Add      | Update   | Publisher Requirement | Description                                                     |
|--------------|----------|-------------|----------|----------|-----------------------|-----------------------------------------------------------------|
| Target       | String   |             | Required | Optional | Must support          | The target category. For example Age.                           |
| TargetValues | String[] |             | Required | Optional | Must support          | A list of target values. For example age range 18-24 and 25-34. |

The values for these fields come from the Target and TargetValue reference data.

##Reference Data
This section defines the reference data that an OpenDirect API must support. Reference data provides enumerated values for a resource property. The publisher must return only those values that they support.

###AdFormatType
Defines the possible ad formats.

| Property | Type   | Description                                                                                                         |
|----------|--------|---------------------------------------------------------------------------------------------------------------------|
| Id       | String | A system-generated opaque ID that uniquely identifies this resource. <br>The ID may contain a maximum of 36 characters. |
| Name     | String | The ad format’s display name.                                                                                       |

The API may support all or a subset of the following ad formats.

  - Flash
  - FlashExpandable
  - Image
  - Tag
  - TagExpandable
  - Text
  - Video

Tag and TagExpandable denote a third-party script.

###AdPosition

Defines the possible ad positions on a web page.

| Property | Type   | Description                                                                                                         |
|----------|--------|---------------------------------------------------------------------------------------------------------------------|
| Id       | String | A system-generated opaque ID that uniquely identifies this resource. <br>The ID may contain a maximum of 36 characters. |
| Name     | String | The ad position’s display name.                                                                                     |

The API may support all or a subset of the following ad positions.

  - AboveFold—Ad placements that are visible without scrolling.
  - BelowFold—Ad placements that are visible only if the user scrolls down the page.

###ContactType

Defines the possible types of Contacts.

| Property | Type   | Description                                                                                                             |
|----------|--------|-------------------------------------------------------------------------------------------------------------------------|
| Id       | String | A system-generated opaque ID that uniquely identifies this resource. <br> The ID may contain a maximum of 36 characters. |
| Name     | String | The type’s display name.                                                                                                |

The API must support the following contact types.

  - Billing—The person to contact with billing inquiries.
  - Buyer—The person to contact with general questions about the order.
  - Creative—The person to contact if there is an issue with one of the order’s creatives.

###Country

Defines a country that the API supports.

| Property | Type   | Description                           |
|----------|--------|---------------------------------------|
| IsoCode  | String | The country’s two-character ISO code. |

The API may support all or a subset of the countries specified in ISO 3166-1.

###Currency

Defines a currency that the API supports.

| Property | Type   | Description                              |
|----------|--------|------------------------------------------|
| IsoCode  | String | The currency’s three-character ISO code. |

The API may support all or a subset of the currencies specified in ISO 4217.

###DeliveryType

Defines the possible types of delivery.

| Property | Type   | Description                                                                                                              |
|----------|--------|--------------------------------------------------------------------------------------------------------------------------|
| Id       | String | A system-generated opaque ID that uniquely identifies this resource. <br> The ID may contain a maximum of 36 characters. |
| Name     | String | The delivery type’s display name.                                                                                        |

The API may support all or a subset of the following formats.

  - Exclusive—100% share of voice.
  - Guaranteed—Guaranteed delivery of all booked impressions.

###FrequencyCapInterval

Defines the frequency cap intervals that the API supports.

| Property | Type   | Description                                                   |
|----------|--------|---------------------------------------------------------------|
| Id       | String | A system-generated ID that uniquely identifies this resource. |
| Name     | String | The name of the interval.                                     |

The frequency interval specifies the units in which the frequency count is expressed. For example, if a line’s frequency count is 2 and interval is Day, display the ad to the same user a maximum of 2 times in the same calendar day.

The API may support all or a subset of the following intervals.

  - Day
  - Month
  - Week
  - Hour
  - LineDuration – For the life of the line based on its start and end dates.

###Industry

Defines an industry that the advertiser belongs to.

| Property      | Type       | Description                                                                        |
|---------------|------------|------------------------------------------------------------------------------------|
| Id            | String     | A system-generated ID that uniquely identifies this resource.                      |
| Name          | String     | The industry’s display name.                                                       |
| ParentId      | String     | The ID of the sub-industry’s parent. Is NULL for the top-level parent.             |
| SubIndustries | Industry[] | A list of sub-industries. The list is empty if the industry has no sub-industries. |

The API may support all or a subset of the following industries:

  - Arts & Entertainment
	  - All
	  - Books & Literature
	  - Celebrity/Fan Gossip
	  - Fine Art
	  - Humor
	  - Movies
	  - Music
	  - Television
  - Automotive
	  - All
	  - Auto Parts
	  - Auto Repair
	  - Buying/Selling Cars
	  - Car Culture
	  - Certified Pre-owned
	  - Convertible
	  - Coupe
	  - Crossover
	  - Diesel
	  - Electric Vehicle
	  - Hatchback
	  - Hybrid
	  - Luxury
	  - Mini Van
	  - Motorcycles
	  - Off-Road Vehicles
	  - Performance Vehicles
	  - Pickup
	  - Road-Side Assistance
	  - Sedan
	  - Trucks & Accessories
	  - Vintage Cars
	  - Wagon
  - Business
	  - All
	  - Advertising
	  - Agriculture
	  - Biotech/Biomedical
	  - Business Software
	  - Construction
	  - Forestry
	  - Government
	  - Green Solutions
	  - Logistics
	  - Marketing
	  - Metals
  - Careers
	  - All
	  - Career Advice
	  - Career Planning
	  - College
	  - Financial Aid
	  - Job fairs
	  - Job Search
	  - Nursing
	  - Resume Writing/Advice
	  - Scholarships
	  - Telecommuting
	  - U.S. Military
  - Education
    - All
    - 7-12 Education
    - Adult Education
    - Art History
    - College Administration
    - College Life
    - Distance Learning
    - English as a 2nd Language
    - Graduate School
    - Homeschooling
    - Homework/Study Tips
    - K-6 Education
    - Language Learning
    - Private School
    - Special Education
    - Studying Business
  - Family & Parenting
    - All
    - Adoption
    - Babies & Toddlers
    - Daycare/Preschool
    - Eldercare
    - Family Internet
    - Parenting – K-6 Kids
    - Parenting – 7-12 Kids
    - Parenting Teens
    - Pregnancy
    - Special Needs Kids
  - Food & Drink
    - All
    - American Cuisine
    - Barbecues & Grilling
    - Cajun/Creole
    - Chinese Cuisine
    - Cocktails/Beer
    - Coffee/Tea
    - Cuisine-Specific
    - Desserts & Baking
    - Dining Out
    - Food Allergies
    - French Cuisine
    - Health/Low-fat Cooking
    - Italian Cuisine
    - Mexican Cuisine
    - Vegan
    - Vegetarian
    - Wine
  - Health & Fitness
    - All
    - A.D.D.
    - AIDS/HIV
    - Allergies
    - Alternative Medicine
    - Arthritis
    - Asthma
    - Autism/PDD
    - Bipolar Disorder
    - Brain Tumor
    - Cancer
    - Cholesterol
    - Chronic Fatigue syndrome
    - Chronic Pain
    - Cold & Flu
    - Deafness
    - Dental Care
    - Depression
    - Dermatology
    - Diabetes
    - Epilepsy
    - Exercise
    - GERN/Acid Reflux
    - Headaches/Migraines
    - Heart Disease
    - Herbs for Health
    - Holistic Healing
    - IBS/Crohn’s Disease
    - Men’s Health
    - Nutrition
    - Orthopedics
    - Panic/Anxiety Disorders
    - Pediatrics
    - Physical Therapy
    - Psychology/Psychiatry
    - Senior Health
    - Sexuality
    - Sleep Disorders
    - Smoking Cessation
    - Substance Abuse
    - Thyroid Disease
    - Weight Loss
    - Women’s Health
  - Hobbies & Interest
    - All
    - Art/Technology
    - Arts & Crafts
    - Beadwork
    - Bird Watching
    - Board Games/Puzzles
    - Candle & Soap Making
    - Card Games
    - Chess
    - Cigars
    - Collecting
    - Comic Books
    - Drawing/Sketching
    - Freelance Writing
    - Genealogy
    - Getting Published
    - Guitar
    - Home Recording
    - Investors & Patents
    - Jewelry Making
    - Magic & Illusion
    - Needlework
    - Painting
    - Photography
    - Radio
    - Roleplaying Games
    - Sci-Fi & Fantasy
    - Scrapbooking
    - Screenwriting
    - Stamps & Coins
    - Video & Computer Games
    - Woodworking
  - Home & Garden
    - All
    - Appliances
    - Entertaining
    - Environmental Safety
    - Gardening
    - Home Repair
    - Home Theater
    - Interior Decorating
    - Landscaping
    - Remodeling & Construction
  - Law, Gov’t & Politics
    - All
    - Commentary
    - Immigration
    - Legal Issues
    - Politics
    - U.S. Government Resources
  - News
    - All
    - International News
    - Local New
    - National News
  - Other
  - Personal Finance
    - All
    - Beginning Investing
    - Credit/Debit & Loans
    - Financial New
    - Financial Planning
    - Hedge Fund
    - Insurance
    - Investing
    - Mutual Funds
    - Options
    - Retirement Planning
    - Stocks
    - Tax Planning
  - Pets
    - All
    - Aquariums
    - Birds
    - Cats
    - Dogs
    - Large Animals
    - Reptiles
    - Veterinary Medicine
  - Real Estate
    - All
    - Apartments
    - Architects
    - Buying/Selling Homes
  - Religion & Spirituality
    - All
    - Alternative Religions
    - Atheism/Agnosticism
    - Buddhism
    - Catholicism
    - Christianity
    - Hinduism
    - Islam
    - Judaism
    - Latter-Day Saints
    - Pagan/Wiccan
  - Science
    - All
    - Astrology
    - Biology
    - Botany
    - Chemistry
    - Geography
    - Geology
    - Paranormal Phenomena
    - Physics
    - Space/Astronomy
    - Weather
  - Shopping
    - All
    - Comparison
    - Contests & Freebies
    - Couponing
    - Engines
  - Society
    - All
    - Dating
    - Divorce Support
    - Ethnic Specific
    - Gay Life
    - Marriage
    - Senior Living
    - Teens
    - Weddings
  - Sports
    - All
    - Auto Racing
    - Baseball
    - Bicycling
    - Bodybuilding
    - Boxing
    - Canoeing/Kayaking
    - Cheerleading
    - Climbing
    - Cricket
    - Figure Skating
    - Fly Fishing
    - Football
    - Freshwater Fishing
    - Game & Fish
    - Golf
    - Horse Racing
    - Horses
    - Hunting/Shooting
    - Inline Skating
    - Martial Arts
    - Mountain Biking
    - NASCAR Racing
    - Olympics
    - Paintball
    - Power & Motorcycles
    - Pro Basketball
    - Pro Ice Hockey
    - Rodeo
    - Rugby
    - Running/Jogging
    - Sailing
    - Saltwater Fishing
    - Scuba Diving
    - Skate Boarding
    - Skiing
    - Snowboarding
    - Surfing/Body Boarding
    - Swimming
    - Table Tennis/Ping-Pong
    - Tennis
    - Volleyball
    - Walking
    - Waterski/Wakeboard
    - Work Soccer
  - Style & Fashion
    - All
    - Accessories
    - Beauty
    - Body Art
    - Clothing
    - Fashion
    - Jewelry
  - Technology & Computing
    - All
    - 3-D Graphics
    - Animation
    - Antivirus Software
    - C/C++
    - Cameras & Camcorders
    - Cell Phones
    - Computer Certification
    - Computer Networking
    - Computer Peripherals
    - Computer Reviews
    - Data Centers
    - Databases
    - Desktop Publishing
    - Desktop Video
    - Email
    - Graphics Software
    - Home Video/DVD
    - Internet Technology
    - Java
    - JavaScript
    - Linux
    - MP2/MIDI
    - Mac OS
    - Mac Support
    - Net Conferencing
    - Net for Beginners
    - Network Security
    - PC Support
    - Palmtops/PDAs
    - Portable Entertainment
    - Shareware/Freeware
    - Unix
    - Visual Basic
    - Web Clip Art
    - Web Design/HTML
    - Web Search
    - Windows
  - Travel
    - All
    - Adventure Travel
    - Africa
    - Air Travel
    - Africa
    - Air Travel
    - Australia & New Zealand
    - Bed & Breakfasts
    - Budget Travel
    - Business Travel
    - Camping
    - Canada
    - Caribbean
    - Cruises
    - Eastern Europe
    - Europe
    - France
    - Greece
    - Honeymoons/Getaways
    - Hotels
    - Italy
    - Japan
    - Mexico & Central America
    - National Parks
    - South America
    - Spas
    - Theme Parks
    - Traveling with Kids
    - United Kingdom

###InventoryType

Defines a list of devices that the product may serve on.

| Property | Type   | Description                                                                                                              |
|----------|--------|--------------------------------------------------------------------------------------------------------------------------|
| Id       | String | A system-generated opaque ID that uniquely identifies this resource. <br> The ID may contain a maximum of 36 characters. |
| Name     | String | The ad format’s display name.                                                                                            |

The API may support all or a subset of the following values.

  - App — An in-app ad
  - Desktop
  - Mobile
  - Tablet

###Language

Defines a language that the API supports.

| Property | Type   | Description                            |
|----------|--------|----------------------------------------|
| IsoCode  | String | The language’s two-character ISO code. |

The API may support all or a subset of the languages specified in ISO 639-1.

###MaturityLevel

Defines a list of maturity levels.

| Property | Type   | Description                                                                                                              |
|----------|--------|--------------------------------------------------------------------------------------------------------------------------|
| Id       | String | A system-generated opaque ID that uniquely identifies this resource. <br> The ID may contain a maximum of 36 characters. |
| Name     | String | The ad format’s display name.                                                                                            |

The API may support all or a subset of the following values.

  - Children
  - General
  - Mature

###RateType

Defines a unit of measure that a cost (i.e. BasePrice) is expressed in.

| Property | Type   | Description                                                                                                              |
|----------|--------|--------------------------------------------------------------------------------------------------------------------------|
| Id       | String | A system-generated opaque ID that uniquely identifies this resource. <br> The ID may contain a maximum of 36 characters. |
| Name     | String | The rate type’s display name.                                                                                            |

The API may support all or a subset of the following values.

  - CPM—Cost per thousand impressions
  - CPMV—Cost per thousand impressions viewed
  - CPC—Cost per click
  - CPD—Cost per day
  - FlatRate—Flat rate

###Target

Defines a target category. For example: gender or DMA targeting.

| Property | Type   | Description                                          |
|----------|--------|------------------------------------------------------|
| Id       | String | A system-generated ID that identifies this resource. |
| Name     | String | The target category.                                 |

The API must support the following target categories and may support additional categories such as zip code or postal code.

  - Age
  - Gender
  - DMA
  - Country
  - State/Province
  - Daypart
  - Weekpart
  - Behavioral

###TargetValue

Defines a target value.

| Property | Type   | Description                                                                           |
|----------|--------|---------------------------------------------------------------------------------------|
| Id       | String | A system-generated ID that uniquely identifies this resource.                         |
| Value    | String | The target value.                                                                     |
| TargetId | String | A system-generated ID that identifies the target category that this value belongs to. |


The API must support the following values per target category:

  - Age
    - Publisher-defined age ranges.
  - Gender
    - Female
    - Male
  - DMA
    - Source is Digital Envoy
  - Country
    - Source is Digital Envoy
  - State/Province
    - Source is Digital Envoy
  - Daypart
    - 0 through 23 hours
  - Weekpart
    - Sunday
    - Monday
    - Tuesday
    - Wednesday
    - Thursday
    - Friday
    - Saturday
  - Behavioral
    - Publisher-defined behavioral segments.

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

##Authentication

Publishers must support authenticating advertiser and agency users. Publishers must use OAuth 2.0 for user authentication. Publishers must support the implicit and authorization code grant flows.

Each request must include an AccessToken header that is set to the user’s access token. If the token is not valid, the request must fail with HTTP status code 401 Unauthorized.

##Versioning

Versioning occurs at the API level and is URI based. All services that make up the API must use the same version number. The version may fall anywhere in the path before the resource and must have the form vn, where n is a positive integer. For example, in the URI https://<host>/api/v1/accounts/{id}, v1 indicates version 1 of the API.

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

##Reporting

Reporting occurs at the line level. The publisher must support the following GET calls to generate a click and impression report.

| URI                                         | Description                                                       |
|---------------------------------------------|-------------------------------------------------------------------|
| /accounts/{id}/orders/{id}/lines/stats      | Aggregates the impressions and clicks for all lines in the order. |
| /accounts/{id}/orders/{id}/lines/{id}/stats | Aggregates the impressions and clicks for the specified line.     |

The following identifies the properties of the report.

| Property          | Type    | Required/Optional | Description                                                                                          |
|-------------------|---------|-------------------|------------------------------------------------------------------------------------------------------|
| Clicks            | Long    | Required          | The number of clicks to date. The value must be zero if no clicks have occurred.                     |
| CTR               | Short   | Optional          | The click through rate to date. The formula to calculate CTR is (clicks / impressions) * 100.        |
| ImpressionsServed | Long    | Required          | The number of impressions served to date. The value must be zero if no impressions have been served. |
| ReportDate        | String  | Required          | The data and time of the report. The date and time is reported in the order’s time zone.             |
| Spend             | Decimal | Optional          | The amount spent to date.                                                                            |

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
