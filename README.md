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
cd..
cd WeatherStationClient
dotnet run
```

Enter the number of measurements to generate (default is 100). For example, enter `10`.

To enter your own measurements, type `m`.

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

headers section:

```Headers
```X-API-Key      api-key-broadridge-demo
```Content-Type   application/x-ndjson

 Body → raw → Text and enter the following data:

```text
{"stationName":"Hamburg","temperature":12.0}
{"stationName":"Bulawayo","temperature":8.9}
{"stationName":"Hamburg","temperature":34.2}
{"stationName":"Bulawayo","temperature":65.2}
{"stationName":"Bulawayo","temperature":75.2}
```

Click **Send** to get results.

**Expected Output:**

```json
{"stationName":"Bulawayo","min":8.9,"mean":49.8,"max":75.2}
{"stationName":"Hamburg","min":12.0,"mean":23.1,"max":34.2}
```

## API Key Configuration

The API requires a valid API key.

- Set the API key in `WeatherStationService/appsettings.json` under `ApiKey`.
- Set the same API key and the service URL in the client-side `appsettings.json`.
- Both the service and client should use the same API key that is `api-key-broadridge-demo` .

If the API key is missing or incorrect, the API returns:

`Missing or invalid API key.`



## API Endpoint

**POST** `/api/weather/stationaggregate`

The API accepts JSONL data and returns the results in the same format.

## Example

**Input:**

```text
{"stationName":"Hamburg","temperature":12.0}
{"stationName":"Bulawayo","temperature":8.9}
{"stationName":"HAMBURG","temperature":34.2}
{"stationName":"st. john's","temperature":-3.5}
{"stationName":"ST. JOHN'S","temperature":-4}

**Output:**

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

## Assumptions

- Station names must be between 1 and 100 UTF-8 bytes.
- Temperatures must be between -99.9 and 99.9, with one fractional digit.
- Station names are matched without considering uppercase or lowercase letters.
- Output names shows uppercase first letters for each word.
- Results are pinted in JSON format in alphabetically order.


## Design and Performance

- API key validation is handled by middleware.
- A case-insensitive dictionary groups measurements by station.
- Only the minimum, maximum, total and count are stored for each station.
- Temperature calculations are handled by the `StationTemperature` class in a separate file.
- Run in Release mode,for good performance

