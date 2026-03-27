# Web Application Architecture

## URL

### Components

Example:

`https://asset-check.run.app/results?ids=20251015120903_RECKITT_Nurofen_LasVegas_020_004_AT,20251015120903_FY23Q4_Pepsi_MOVIES_Sleek_Black_Fantasy_16x9_6s_1`

- Domain: `https://asset-check.run.app`
- Path/Endpoint: `/results`
- Query Parameter: `?ids=20251015120903_RECKITT_Nurofen_LasVegas_020_004_AT,20251015120903_FY23Q4_Pepsi_MOVIES_Sleek_Black_Fantasy_16x9_6s_1`

### Request/Response

Whenever we visit some type of website we refer to it as the client, or the frontend. Behind the scenes, that frontend is communicating with some type of API (Application Programming Interface) - which is running on a different server - through requests and responses.

The request is what we send from the frontend to the backend essentially saying what we want to do. The response is what comes back from the backend to our the client.

#### Request Components

##### Type/Method

- **GET**: Retrieve something
- **DELETE**: Delete something
- **POST**: Create something
- **PUT**/PATCH: Update something

##### Path

##### Request Body

##### Request Headers

#### Response Components

##### Status Code

![status_codes.tiff]

##### Response Body

##### Response Headers

