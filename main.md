
# Devices API Documentation
  

**Version:** 2.0.0
  

## Base URL
  

Use the following base URLs based on your geographic region:
  

- **US:** `https://dnaspaces.io/api/location/v2`

- **EU:** `https://dnaspaces.eu/api/location/v2`

  

## Table of Contents

  

1. [Introduction](#introduction)

2. [Authentication & Security](#authentication--security)

3. [Active Devices Endpoints](#active-devices-endpoints)

- [Get Active Devices](#get-active-devices)

- [Get Active Devices Count](#get-active-devices-count)

- [Get Floors with Active Devices](#get-floors-with-active-devices)

  

---

  

## Introduction
  




---
## Authentication & Security

  

All API requests require an **Authorization** header with a valid Bearer token. For example:

  

Authorization: Bearer {your_access_token}

  

If the token is missing, expired, or invalid, the API will return a **401 Unauthorized** response.

  

---

  

## Active Devices Endpoints

  

### Get Active Devices

  

- **Method:** GET

- **Path:** `/v2/devices`

- **Description:**

Retrieves a list of active devices. You can filter the results using various query parameters such as device type, device ID, IP address, and location identifiers like floor, building, and campus. Pagination is supported (default page is 1 with 1000 items per page).

  

- **Query Parameters:**

  

- `deviceType`: CLIENT, TAG, BLE, ROGUE_AP, ROGUE_CLIENT or INTERFERER

- `deviceId`: The device unique identifier, for example the device macAddress.

- `ipAddress`: IP address of the connected device. Available for associated devices only.

- `floorId`: Unique identifier for a floor from the map import process

- `buildingId`: Unique identifier for a building from the map import process

- `campusId`: Unique identifier for a campus from the map import process

- `associated`: `true|false.` Whether or not a device has connected to a network.

- `manufacturer`: Manufacturer of the device.(e.g., Aeroscout Ltd.)

- `mapElementId`: Indicate the map element unique identifier.

- `mapElementLevel`: Indicate the map element level, valid value is "campus", "building" and "floor".

- `page`: The page number requests for. Start from 1 and default value is 1.

- `username`: user name of the connected user. Available for associated devices only.

- `limit`: The maximum number of items that may be returned for a single request. Default value is 1000. Other allowed values are from 1000 to 20K. Anything above 20k is sent will be converted to 10k limit.

- `searchType`: Indicate if exact match is required. Valid value is "wildcard"

  

- **Responses:**

  

- **200 OK:**

```json

{

"results": [

{

"tenantId": "200",

"macAddress": "00:12:b8:0a:c6:20",

"deviceType": "TAG",

"campusId": "5cb06b3ba1d943669963417c4330c6c7",

"buildingId": "98c86b09665a4ffab9647709d966f3a6",

"floorId": "3b3aad61352e4bc09cdea0119ee3d9f3",

"lhfloorId": "3b3aad61352e4bc09cdea0119ee3d9f3",

"hierarchy": "San Jose->SJC-17->1st Floor",

"locationHierarchy": "root-node->San Jose->SJC-17->1st Floor",

"hierarchyIds": [

"0c872fbb-0545-45fd-b7e2-6ce6ff6f08af",

"2d3da06b-f4f2-4b3e-9e56-492665ae78ac"

],

"source": "NOTIFICATION",

"isMacHashed": false,

"deviceId": "00:24:b1:02:e7:10",

"coordinates": [33.401928, 101.87378],

"geoCoordinates": [49.26188380661546, -123.24809010788138],

"confidenceFactor": 120,

"computeType": "RSSI",

"firstLocatedAt": "2018-05-25T17:30:12.403Z",

"lastLocatedAt": "2018-05-27T21:14:44.005Z",

"changedOn": "1527456440322",

"associated": false,

"manufacturer": "Aeroscout Ltd.",

"maxDetectedRssi": {

"apMacAddress": "f0:7f:06:35:8d:00",

"band": "IEEE_802_11_B",

"slot": 2,

"rssi": -81,

"antennaIndex": 0,

"lastHeard": 1

},

"numDetectingAps": 3,

"apList": [

{

"apMacAddress": "3c:08:f6:fb:29:50",

"bands": ["IEEE_802_11_A"]

}

]

}

],

"querystring": "string",

"success": true,

"morePage": false

}

```

- **400 Bad Request / 401 Unauthorized / 403 Forbidden / 500 Internal Error:**

```json

{

"code": 0,

"message": "string"

}

```

  

- **cURL Example:**

  

```bash

curl -X GET "{{baseUrl}}/v2/devices?floorId=776c2d22-e8a4-46d4-9ad9-fdde33a3c4bc&buildingId=9f50562c-4d26-42e4-b5b3-f4d56a6c2aa2&campusId=23141525-e33f-43c0-8909-4cbe75feba39&mapElementLevel=floor" \

-H "Authorization: Bearer {your_access_token}" \

-H "Content-Type: application/json"

```

  

### Get Active Devices Count

  

- **Method:** GET

- **Path:** `/v2/devices/count`

- **Description:**

Retrieves the total number of active devices that match the provided filtering criteria. This endpoint uses similar query parameters as the Get Active Devices endpoint.

  

- **Query Parameters:**

  

- `deviceType`: CLIENT, TAG, BLE, ROGUE_AP, ROGUE_CLIENT or INTERFERER

- `deviceId`: The device unique identifier, for example the device macAddress.

- `ipAddress`: IP address of the connected device. Available for associated devices only.

- `floorId`: Unique identifier for a floor from the map import process

- `buildingId`: Unique identifier for a building from the map import process

- `campusId`: Unique identifier for a campus from the map import process

- `associated`: `true|false.` Whether or not a device has connected to a network.

- `manufacturer`: Manufacturer of the device.(e.g., Aeroscout Ltd.)

- `mapElementId`: Indicate the map element unique identifier.

- `mapElementLevel`: Indicate the map element level, valid value is "campus", "building" and "floor".

- `page`: The page number requests for. Start from 1 and default value is 1.

- `username`: user name of the connected user. Available for associated devices only.

- `limit`: The maximum number of items that may be returned for a single request. Default value is 1000. Other allowed values are from 1000 to 20K. Anything above 20k is sent will be converted to 10k limit.

- `rogueApClients`:

- `searchType`: When using deviceType=ROGUE_AP, this will return rogue APs that have connected devices.

- `apMacAddress`: The mac address of the Access Point (AP). Available for associated devices only.

  

- **cURL Example:**

  

```bash

curl -X GET "{{baseUrl}}/v2/devices/count?floorId=776c2d22-e8a4-46d4-9ad9-fdde33a3c4bc&buildingId=9f50562c-4d26-42e4-b5b3-f4d56a6c2aa2&campusId=23141525-e33f-43c0-8909-4cbe75feba39&mapElementLevel=floor" \

-H "Authorization: Bearer {your_access_token}" \

-H "Content-Type: application/json"

```

  

- **Responses:**

- **200 OK:**

```json

{

"results": {

"total": 1012

},

"querystring": {

"tenantId": "1"

},

"success": true

}

```

- **400 Bad Request / 401 Unauthorized / 403 Forbidden / 500 Internal Error:**

```json

{

"code": 0,

"message": "string"

}

```

  

### Get Floors with Active Devices

  

- **Method:** GET

- **Path:** `/v2/devices/floors`

- **Description:**

Retrieves a list of floors with active devices.

  

- **Responses**

- **200 OK:**

```json

{

"results": [

{

"floorId": "719d02e2b00e4535b6e095c0d757e44b",

"count": 11000

}

],

"success": true

}

```

- **400 Bad Request / 401 Unauthorized / 403 Forbidden / 500 Internal Error:**

```json

{

"code": 0,

"message": "string"

}

```

- **cURL Example:**

  

```bash

curl -X GET "{{baseUrl}}/v2/devices/floors" \

-H "Authorization: Bearer {your_access_token}" \

-H "Content-Type: application/json"

```
