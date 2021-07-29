# Kotohomes Listing API

This API is solely intended for retrieving small sets of real estate listing data that matches a given set of search filters, which in turn may be used for the generation of dynamic editorial content in a non-live fashion.



## Authentication

All API requests must provide a valid `key` in the query string. Missing or invalid credentials as well as inactive accounts will yield an HTTP error code **403**.



## Request

The endpoint for all listing search requests is: https://api.kotohomes.com/v1/listings

The parameters listed below may be added to the query string to filter the results.

| Parameter                | Type         | Explanation                                                                              |
| ------------------------ | ------------ | ---------------------------------------------------------------------------------------- |
| **key** – _required_     | string       | Authentication key                                                                       |
| **sort_by**              | string       | Sort results by `updated_at` or `created_at` in descending order (Default: `updated_at`) |
| **mls_numbers[]**        | string array | Filter results based on one or more MLS numbers                                          |
| **property_types[]**     | string array | Filter results based on property types                                                   |
| **states[]**             | string array | Filter results based on states                                                           |
| **zip_codes[]**          | string array | Filter results based on ZIP codes                                                        |
| **price_min**            | integer      | Minimum price (inclusive)                                                                |
| **price_max**            | integer      | Maximum price (inclusive)                                                                |
| **living_area_min**      | integer      | Minimum living area in square foot (inclusive)                                           |
| **living_area_max**      | integer      | Maximum living area in square foot (inclusive)                                           |
| **year_built_min**       | integer      | Minimum year built (inclusive)                                                           |
| **year_built_max**       | integer      | Maximum year built (inclusive)                                                           |
| **bedrooms_min**         | float        | Minimum bedroom count (inclusive)                                                        |
| **bedrooms_max**         | float        | Maximum bedroom count (inclusive)                                                        |
| **bathrooms_min**        | float        | Minimum bathroom count (inclusive) including fractional                                  |
| **bathrooms_max**        | float        | Maximum bathroom count (inclusive) including fractional                                  |
| **latitude** *           | float        | Geographic latitude of center location                                                   |
| **longitude** *          | float        | Geographic longitude of center location                                                  |
| **radius** *             | float        | Distance in miles from the provide `latitude` and `longitude`                            |

(*) When location-based search is used, all 3 parameters must be provided.





## Response

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




## Errors

| HTTP Status Code   | Explanation                                                                       |
| ------------------ | --------------------------------------------------------------------------------- |
| 200                | Successfull request, with results in response body.                               |
| 400                | Problem with query parameters. Check response body for clues.                     |
| 403                | `key` is invalid.                                                   |
| 500                | Server side error. Try again later.                                               |


## Example

Request for 25 most recently updated listings for site that are in either NY or NJ.

```bash
$ curl "https://api.kotohomes.com/v1/listings?key=key_456&states[]=NY&states[]=NJ"
```


```json
{
  "data": [
    {
      "id": "li_2hAk61Qi6hFGw6nfrRmG",
      "created_at": "2021-04-25T17:35:23.809Z",
      "updated_at": "2021-04-27T13:14:16.772Z",
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
      "photo_url": "https://..."
    },
    /* rest removed for brevity */
   ]
}
```

