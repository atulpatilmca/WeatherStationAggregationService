# Weather Station Aggregation Service

## How to Open the Project

Go to the following folder and open the solution file in Visual Studio:

`WeatherStationAggregationService\WeatherStationService\WeatherStationService.sln`

## How to Run the Application

Open Visual Studio Developer Command Prompt (`Tools → Command Line → Developer Command Prompt`).

Build the solution:

```bash
dotnet build
```

Run the Weather Station Service:

```bash
dotnet run
```

Open another command prompt and run the client:

```bash
cd WeatherStationClient
dotnet run
```

The client will ask how many measurements you want to generate. The default is 100. Enter a number, for example `10`.

If you want to enter your own measurements, type `m`.

## Sample Output

```json
{"stationName":"Bulawayo","min":-37.5,"mean":10.7,"max":49.3}
{"stationName":"Dublin","min":-46.3,"mean":0.7,"max":47.1}
{"stationName":"Hamburg","min":-37.8,"mean":4.1,"max":43.3}
{"stationName":"New York","min":-44.8,"mean":-3.6,"max":41.0}
{"stationName":"Palembang","min":-35.8,"mean":2.8,"max":49.8}
{"stationName":"Rio De Janeiro","min":-38.6,"mean":9.1,"max":34.1}
{"stationName":"St. John's","min":-48.5,"mean":-7.7,"max":48.3}
{"stationName":"Tokyo","min":-43.8,"mean":-5.0,"max":47.2}
```

To run the application in Release mode:

```bash
dotnet run -c Release
```

## Testing the API Using Postman

Start the API from Visual Studio using `Ctrl + F5`.

Open Postman and create a new request with the following details:

- **Method:** POST
- **URL:** `http://localhost:5000/api/weather/stationaggregate`

Add these headers:

| Key | Value |
|---|---|
| X-API-Key | api-key-broadridge-demo |
| Content-Type | application/x-ndjson |

Go to **Body → raw → Text** and enter the following data:

```text
{"stationName":"Hamburg","temperature":12.0}
{"stationName":"Bulawayo","temperature":8.9}
{"stationName":"Hamburg","temperature":34.2}
{"stationName":"Bulawayo","temperature":65.2}
{"stationName":"Bulawayo","temperature":75.2}
```

Click **Send** to get the aggregated results.

**Expected Output:**

```json
{"stationName":"Bulawayo","min":8.9,"mean":49.8,"max":75.2}
{"stationName":"Hamburg","min":12.0,"mean":23.1,"max":34.2}
```

## API Key Configuration

The API requires a valid API key.

- Set the API key in `WeatherStationService/appsettings.json` under `ApiKey`.
- Set the same API key and the service URL in the client-side `appsettings.json`.
- Both the service and client should use the same API key.

For this example, the API key is:

`api-key-broadridge-demo`

If the API key is missing or incorrect, the API returns:

`Missing or invalid API key.`

## API Endpoint

**POST** `/api/weather/stationaggregate`

The API accepts newline-delimited JSON (NDJSON) data and returns the minimum, average and maximum temperature for each weather station.

## Example

**Input:**

```text
{"stationName":"Hamburg","temperature":12.0}
{"stationName":"Bulawayo","temperature":8.9}
{"stationName":"HAMBURG","temperature":34.2}
{"stationName":"st. john's","temperature":-3.5}
{"stationName":"ST. JOHN'S","temperature":-4
