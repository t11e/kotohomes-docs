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
  "_": "rest of content trimmed for brevity"
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

| Parameter               | Type             | Explanation                                                                              |
|-------------------------| ---------------- | ---------------------------------------------------------------------------------------- |
| **key**                 | string           | Authentication key                                                                       |
| **sort_by**             | string array[^5] | Sort results by `created_at`, `updated_at`, `price`, `living_area`, `year_built`, `bedrooms`, and/or `bathrooms`. Default sort direction is `asc`ending, but you can specify `asc` or `desc`  (Default: `updated_at desc`) |
| **limit**               | integer          | Number of results to return, must be between 1 and 50 inclusive and defaults to 25       |
| **mls_numbers**         | string array[^5] | Filter results based on one or more MLS numbers                                          |
| **type**                | string           | Filter results based on type of listing, one of `for_sale`, `for_rent`                   |
| **property_types**[^2]  | string array[^5] | Filter results based on property types                                                   |
| **states**              | string array[^5] | Filter results based on states                                                           |
| **zip_codes**           | string array[^5] | Filter results based on ZIP codes                                                        |
| **price_min**           | integer          | Minimum price (inclusive)                                                                |
| **price_max**           | integer          | Maximum price (inclusive)                                                                |
| **living_area_min**     | integer          | Minimum living area in square foot (inclusive)                                           |
| **living_area_max**     | integer          | Maximum living area in square foot (inclusive)                                           |
| **year_built_min**      | integer          | Minimum year built (inclusive)                                                           |
| **year_built_max**      | integer          | Maximum year built (inclusive)                                                           |
| **bedrooms_min**        | float            | Minimum bedroom count (inclusive)                                                        |
| **bedrooms_max**        | float            | Maximum bedroom count (inclusive)                                                        |
| **bathrooms_min**       | float            | Minimum bathroom count (inclusive) including fractional                                  |
| **bathrooms_max**       | float            | Maximum bathroom count (inclusive) including fractional                                  |
| **latitude**[^1]        | float            | Geographic latitude of center location                                                   |
| **longitude**[^1]       | float            | Geographic longitude of center location                                                  |
| **radius**[^1]          | float            | Distance in miles from the provide `latitude` and `longitude`                            |
| **open_houses_only**    | boolean[^3]      | Only return listings with upcoming open houses                                           |
| **territory**           | string           | Filter results based on a predefined territory id                                        |
| **created_min**         | datetime[^4]     | Minimum creation time (inclusive)                                                        |

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

[^4]: Datetimes should be in ISO 8601 format `YYYY-MM-DDThh:mm[:ss][+HH:MM|-HH:MM|Z]`, e.g: `2022-12-16T10:20:00-05:00`

[^5]: Multiple values are specified by repeating the query parameter multiple times. e.g. `?states=NY&states=NJ`

### Response

The JSON response includes a top-level `data` key for an array of result listing objects. Each listing object has the following content.



| Key                | Type     | Explanation                                                                       |
| ------------------ | -------- | --------------------------------------------------------------------------------- |
| **id**             | string   | Unique listing identifier                                                         |
| **created_at**     | datetime | UTC creation time                                                                 |
| **updated_at**     | datetime | UTC modification time                                                             |
| **type**           | string   | `for_sale` – listing is for sale                                                  |
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
| **photo_count**    | integer  | Number of photos                                                                  |
| **photo_template** | string   | URL template used to fetch photos                                                 |
| **photo_url**      | string   | Primary listing photo URL                                                         |
| ***agent***        | object   |                                                                                   |
| – **name**         | string   | Agent name                                                                        |
| – **email**        | string   | Agent email                                                                       |
| – **phone**        | string   | Agent phone number                                                                |
| ***broker***       | object   |                                                                                   |
| – **name**         | string   | Broker name                                                                       |
| – **email**        | string   | Broker email                                                                      |
| – **phone**        | string   | Broker phone number                                                               |
| – **logo_url**     | string   | Broker logo image URL                                                             |
| ***open_houses***  | list     |                                                                                   |
| - **start**        | string   | UTC start time                                                                    |
| - **end**          | string   | UTC end time                                                                      |

#### photo_template

The `photo_template` field contains a URL template that you can use to fetch any of the photos for the listing, e.g.
`https://example.com/v1/listings/li_1234567890/photo?offset={offset}&size={size}`

You are expected to replace the placeholders with valid values before this URL will work. The placeholders are wrapped
in curly braces `{}` and are:

* `{offset}` -- a positive integer less than `photo_count` and is the 0-based photo offset
* `{size}` -- a prebaked size constraint, options are:

| size     | maximum width or height |
|----------|-------------------------|
| small    | 480x360                 |
| medium   | 640x480                 |
| large    | 1024x768                |
| original | not constrained         |


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
      "photo_count": 5,
      "photo_template": "https://...",
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

## 2023-01-12

Update maximum image sizes in the listing `photo_template`.

## 2023-07-14

Add `limit` search parameter.
Add footnote describing how to search with multiple values for supported fields.

## 2022-12-21

Add `created_min` search parameter.

### 2022-07-22

Add `photo_count` and `photo_template` to listing output.

### 2022-07-11

Expand listing output to include full, 3/4, half and 1/4 bath counts.

### 2022-06-30

For search, both `zip_codes` and `mls_numbers` are accepted as valid constraints when enforcing that at least one
constraint is provided.

Add endpoint to fetch just a single listing.

### 2022-06-13

Add type search parameter to constrain search for just sale or rental listings.

Add open_house_only parameter to only return listings with upcoming open houses.
Add open_houses to the response payload when available.

Add territory parameter to only return listings within your predefined territory.

You must now always provide latitude/longitude/radius or territory.

### 2022-05-16:

Add more sort by options.

Remove `[]` from multivalue parameter names. Deprecated `[]` parameters will continue to work for now.

### 2021-07-29

Remove `site_id` parameter from public listing API since the key is sufficient.
