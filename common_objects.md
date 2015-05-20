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
