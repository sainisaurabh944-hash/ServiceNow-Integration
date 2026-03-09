# ServiceNow Weather API Integration

## Overview

This project demonstrates integration between **ServiceNow** and **WeatherAPI.com** using **Outbound REST Message**.
When a user enters a location and clicks **Get Weather**, ServiceNow retrieves weather data and populates the form automatically.

## Technologies

* ServiceNow
* Outbound REST Message
* WeatherAPI
* JavaScript (Server-side)

## Features

* Custom ServiceNow table for weather data
* REST API integration
* Dynamic form population
* UI Action button to trigger API call

## Table Fields

| Field       | Description          |
| ----------- | -------------------- |
| Location    | City entered by user |
| Temperature | Current temperature  |
| Region      | State / region       |
| Latitude    | Location latitude    |
| Longitude   | Location longitude   |

## API Used

WeatherAPI

Endpoint:
http://api.weatherapi.com/v1/current.json?key=${key}&q=${place}

* key → d2d2ef02d91343a6885123601260903
* q → location(Dynamic)

## UI Action Script

```javascript
// Create REST message object using the configured REST Message record and HTTP method
var rm = new sn_ws.RESTMessageV2('Weather Api', 'Default GET');

// Pass the location entered on the form as query parameter (place)
rm.setStringParameterNoEscape('place', current.u_location);

// Pass the Weather API key as query parameter
rm.setStringParameterNoEscape('key', 'd2d2ef02d91343a6885123601260903');

// Execute the REST API call
var response = rm.execute();

// Get the response body returned from the API
var responseBody = response.getBody();

// Convert JSON response string into JavaScript object
var obj = JSON.parse(responseBody);

// Capture the HTTP response status code
var status = response.getStatusCode();

// Populate the ServiceNow record fields using API response data
current.u_region = obj.location.region;
current.u_latitude = obj.location.lat;
current.u_longitude = obj.location.lon;
current.u_temperature_in_celsius = obj.current.temp_c;

// Update the current record in the database
current.update();

// Redirect user back to the same record after execution
action.setRedirectURL(current);

// Display success message on the form
gs.addInfoMessage("Weather fetched successfully");
```

## Result

User enters location → clicks **Get Weather** → ServiceNow calls weather API → fields auto-populate.

## Use Cases

* Learning ServiceNow integrations
* REST API practice
