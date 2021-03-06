# Listing API

This API is solely intended for retrieving small sets of real estate listing data that matches a given set of search filters, which in turn may be used for the generation of dynamic editorial content in a non-live fashion.



## Authentication

All API requests must provide a valid `key` in the query string. Missing or invalid credentials as well as inactive accounts will yield an HTTP error code **403**.



## Listing Get

The endpoint for all listing get requests is one of:

* https://api.enclosurehomes.com/v1/listings/{id}
* https://api.kotohomes.com/v1/listings/{id}

Mandatory parameters are:

* `key` -- your API key

### Response

The JSON response is the listing content. Each listing object has the same content as documented in the `Listing Search`
endpoint below.

### Errors

| HTTP Status Code   | Explanation                                                                       |
| ------------------ | --------------------------------------------------------------------------------- |
| 200                | Successful request, with results in response body.                                |
| 400                | Problem with query parameters. Check response body for clues.                     |
| 403                | `key` is invalid.                                                                 |
| 404                | There is no such listing.                                                         |
| 500                | Server side error. Try again later.                                               |

### Example

```shell
curl "https://api.enclosurehomes.com/v1/listings/li_123456789xxxx?key=xxx123456789xxx"
# or
curl "https://api.kotohomes.com/v1/listings/li_123456789xxxx?key=xxx123456789xxx"
```


```json
{
  "id": "li_2hAk61Qi6hFGw6nfrRmG",
  "created_at": "2021-04-25T17:35:23Z",
  "updated_at": "2021-04-27T13:14:16Z",
  "type": "for_sale",
  "mls_number": "NYRS-98765",
  "status": "active",
  "property_type": "residential",
  "description": "Quaint condo in the heart of the city ...",
  "price": 500000,
  "year_built": 1930,
  "street_address": "20 W 34th St",
  "unit_number": "2012B",
  "city": "New York",
  "state": "NY",
  "zip_code": "10001",
  "lot_size": 2,
  "living_area": 2350,
  "url": "https://...",
  "longitude": -73.98562148465531,
  "latitude": 40.74892817707063,
  "bedrooms": 3,
  "bathrooms": 2.5,
  "agent": {
    "name": "Listing Agent Name",
    "email": "...",
    "phone": "5555555554"
  },
  "broker": {
    "name": "Listing Broker Name",
    "phone": "5555555555",
    "email": "...",
    "logo_url": "http://..."
  },
  "photo_url": "https://...",
  "open_houses": [
    {"start": "2021-05-01T20:00:00Z", "end": "2021-05-01T21:00:00Z"},
    {"start": "2021-05-05T20:00:00Z", "end": "2021-05-05T21:00:00Z"},
  ]
}
```


## Listing Search

The endpoint for all listing search requests is one of:

* https://api.enclosurehomes.com/v1/listings
* https://api.kotohomes.com/v1/listings

The parameters listed below may be added to the query string to filter the results.

Mandatory parameters are:

* `key` -- your API key
* at least one constraint:
  * `latitude`, `longitude`, and `radius`
  * `territory`
  * `zip_codes`
  * `mls_numbers`

| Parameter               | Type         | Explanation                                                                              |
|-------------------------| ------------ | ---------------------------------------------------------------------------------------- |
| **key**                 | string       | Authentication key                                                                       |
| **sort_by**             | string array | Sort results by `created_at`, `updated_at`, `price`, `living_area`, `year_built`, `bedrooms`, and/or `bathrooms`. Default sort direction is `asc`ending, but you can specify `asc` or `desc`  (Default: `updated_at desc`) |
| **mls_numbers**         | string array | Filter results based on one or more MLS numbers                                          |
| **type**                | string       | Filter results based on type of listing, one of `for_sale`, `for_rent`                   |
| **property_types**[^2]  | string array | Filter results based on property types                                                   |
| **states**              | string array | Filter results based on states                                                           |
| **zip_codes**           | string array | Filter results based on ZIP codes                                                        |
| **price_min**           | integer      | Minimum price (inclusive)                                                                |
| **price_max**           | integer      | Maximum price (inclusive)                                                                |
| **living_area_min**     | integer      | Minimum living area in square foot (inclusive)                                           |
| **living_area_max**     | integer      | Maximum living area in square foot (inclusive)                                           |
| **year_built_min**      | integer      | Minimum year built (inclusive)                                                           |
| **year_built_max**      | integer      | Maximum year built (inclusive)                                                           |
| **bedrooms_min**        | float        | Minimum bedroom count (inclusive)                                                        |
| **bedrooms_max**        | float        | Maximum bedroom count (inclusive)                                                        |
| **bathrooms_min**       | float        | Minimum bathroom count (inclusive) including fractional                                  |
| **bathrooms_max**       | float        | Maximum bathroom count (inclusive) including fractional                                  |
| **latitude**[^1]        | float        | Geographic latitude of center location                                                   |
| **longitude**[^1]       | float        | Geographic longitude of center location                                                  |
| **radius**[^1]          | float        | Distance in miles from the provide `latitude` and `longitude`                            |
| **open_houses_only**    | boolean[^3]  | Only return listings with upcoming open houses                                           |
| **territory**           | string       | Filter results based on a predefined territory id                                        |

[^1]: When location-based search is used, all 3 parameters must be provided.

[^2]: Valid values for `property_types`:

    acreage
    apartment
    cattle_ranch
    commercial
    community
    condominium
    duplex
    farm
    fishing_land
    foreclosure
    garage
    home_with_acreage
    house
    hunting_land
    lots_land
    mobile_manufactured
    mountain_land
    multifamily
    new_construction
    ranch
    ranch_horse
    recreational_land
    residential
    retail
    senior_community
    single_family
    timberland
    townhouse
    unspecified
    vacation_home
    waterfront

[^3]: Valid values for boolean parameters: `true`, `false`


### Response

The JSON response includes a top-level `data` key for an array of result listing objects. Each listing object has the following content.



| Key                | Type     | Explanation                                                                       |
| ------------------ | -------- | --------------------------------------------------------------------------------- |
| **id**             | string   | Unique listing identifier                                                         |
| **created_at**     | datetime | UTC creation time                                                                 |
| **updated_at**     | datetime | UTC modification time                                                             |
| **type**           | string   | `for_sale` ??? listing is for sale                                                  |
| **mls_number**     | string   | MLS number                                                                        |
| **status**         | string   | `active` or `pending`                                                             |
| **property_type**  | string   | Type of property: i.e. `residential`                                              |
| **description**    | string   | Remarks about the listing                                                         |
| **price**          | integer  | Price is USD                                                                      |
| **year_built**     | integer  | Year dwelling was built                                                           |
| **street_address** | string   | Street address for listing                                                        |
| **unit_number**    | string   | Listing unit number                                                               |
| **city**           | string   | City listing is located in                                                        |
| **state**          | string   | State listing is located in                                                       |
| **zip_code**       | string   | ZIP code for listing                                                              |
| **lot_size**       | float    | Lot size in acres                                                                 |
| **living_area**    | integer  | Living area in square foo                                                         |
| **url**            | string   | Link to listing details page                                                      |
| **longitude**      | float    | Longitude of listing location                                                     |
| **latitude**       | float    | Latitude of listing location                                                      |
| **bedrooms**       | float    | Number of full and fractional bedrooms                                            |
| **bathrooms**      | float    | Number of full and fractional bathrooms                                           |
| **bathrooms_full**          | integer  | Number of full bathrooms                                                 |
| **bathrooms_three_quarter** | integer  | Number of three quarter  bathrooms                                       |
| **bathrooms_half**          | integer  | Number of half bathrooms                                                 |
| **bathrooms_quarter**       | integer  | Number of quarter bathrooms                                              |
| **photo_url**      | string   | Primary listing photo URL                                                         |
| ***agent***        | object   |                                                                                   |
| ??? **name**         | string   | Agent name                                                                        |
| ??? **email**        | string   | Agent email                                                                       |
| ??? **phone**        | string   | Agent phone number                                                                |
| ***broker***       | object   |                                                                                   |
| ??? **name**         | string   | Broker name                                                                       |
| ??? **email**        | string   | Broker email                                                                      |
| ??? **phone**        | string   | Broker phone number                                                               |
| ??? **logo_url**     | string   | Broker logo image URL                                                             |
| ***open_houses***  | list     |                                                                                   |
| - **start**        | string   | UTC start time                                                                    |
| - **end**          | string   | UTC end time                                                                      |



### Errors

| HTTP Status Code   | Explanation                                                                       |
| ------------------ | --------------------------------------------------------------------------------- |
| 200                | Successful request, with results in response body.                                |
| 400                | Problem with query parameters. Check response body for clues.                     |
| 403                | `key` is invalid.                                                                 |
| 500                | Server side error. Try again later.                                               |


### Example

Request for 25 most recently updated listings in a location.

```shell
curl "https://api.enclosurehomes.com/v1/listings?key=xxx123456789xxx&latitude=38.8977&longitude=-77.0365&radius=20"
# or
curl "https://api.kotohomes.com/v1/listings?key=xxx123456789xxx&latitude=38.8977&longitude=-77.0365&radius=20"
```


```json
{
  "data": [
    {
      "id": "li_2hAk61Qi6hFGw6nfrRmG",
      "created_at": "2021-04-25T17:35:23Z",
      "updated_at": "2021-04-27T13:14:16Z",
      "type": "for_sale",
      "mls_number": "NYRS-98765",
      "status": "active",
      "property_type": "residential",
      "description": "Quaint condo in the heart of the city ...",
      "price": 500000,
      "year_built": 1930,
      "street_address": "20 W 34th St",
      "unit_number": "2012B",
      "city": "New York",
      "state": "NY",
      "zip_code": "10001",
      "lot_size": 2,
      "living_area": 2350,
      "url": "https://...",
      "longitude": -73.98562148465531,
      "latitude": 40.74892817707063,
      "bedrooms": 3,
      "bathrooms": 2.5,
      "bathrooms_full": 2,
      "bathrooms_three_quarter": null,
      "bathrooms_half": 1,
      "bathrooms_quarter": null,
      "agent": {
        "name": "Listing Agent Name",
        "email": "...",
        "phone": "5555555554"
      },
      "broker": {
        "name": "Listing Broker Name",
        "phone": "5555555555",
        "email": "...",
        "logo_url": "http://..."
      },
      "photo_url": "https://...",
      "open_houses": [
        {"start": "2021-05-01T20:00:00Z", "end": "2021-05-01T21:00:00Z"},
        {"start": "2021-05-05T20:00:00Z", "end": "2021-05-05T21:00:00Z"},
      ]
    }
   ]
}
```

## Changelog

### 2022-07-11

Expands listing output to include full, 3/4, half and 1/4 bath counts.

### 2022-06-30

For search, both `zip_codes` and `mls_numbers` are accepted as valid constraints when enforcing that at least one
constraint is provided.

Adds endpoint to fetch just a single listing.

### 2022-06-13

Adds type search parameter to constrain search for just sale or rental listings.

Adds open_house_only parameter to only return listings with upcoming open houses.
Adds open_houses to the response payload when available.

Adds territory parameter to only return listings within your predefined territory.

You must now always provide latitude/longitude/radius or territory.

### 2022-05-16:

Added more sort by options.

Removed `[]` from multivalue parameter names. Deprecated `[]` parameters will continue to work for now.

### 2021-07-29

Removed `site_id` parameter from public listing API since the key is sufficient.
