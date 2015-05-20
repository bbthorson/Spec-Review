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
