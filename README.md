# Weather Station Aggregation Service

## How to Open the Project

1. Navigate to the following location:
   `WeatherStationAggregationService\WeatherStationService`
2. Double-click `WeatherStationService.sln` to open the solution in Visual Studio.

## How to Run

Open **Developer Command Prompt** in Visual Studio (`Tools → Command Line → Developer Command Prompt`).

**1. Build the solution:**

```bash
dotnet build
```

**2. Start the Weather Station Service:**

```bash
dotnet run
```

**3. Open another command prompt and start the client:**

```bash
cd WeatherStationClient
dotnet run
```

When prompted, enter the number of measurements to generate. The default is `100`.

For example, enter `10` to generate 10 measurements.

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

## Run in Release Mode

For better performance, run the application using:

```bash
dotnet run -c Release
```

## Testing the API Using Postman

**1. Start the API**

Run the service from Visual Studio using `Ctrl + F5`, or start it from the Developer Command Prompt.

**2. Configure the Postman request**

- Method: `POST`
- URL: `http://localhost:5000/api/weather/stationaggregate`

**3. Add the following headers:**

| Key | Value |
|---|---|
| X-API-Key | api-key-broadridge-demo |
| Content-Type | application/x-ndjson |

**4. Configure the request body**

Select `Body → raw` and choose `Text`.

Enter the following input (one JSON object per line):

```text
{"stationName":"Hamburg","temperature":12.0}
{"stationName":"Bulawayo","temperature":8.9}
{"stationName":"Hamburg","temperature":34.2}
{"stationName":"Bulawayo","temperature":65.2}
{"stationName":"Bulawayo","temperature":75.2}
```

**5. Expected Output**

```json
{"stationName":"Bulawayo","min":8.9,"mean":49.8,"max":75.2}
{"stationName":"Hamburg","min":12.0,"mean":23.1,"max":34.2}
```

## API Key Configuration

The service requires a valid API key.

- Configure the API key in `WeatherStationService/appsettings.json` under `ApiKey`.
- Configure the same API key and `ServiceUrl` in the client-side `appsettings.json`.
- Both the service and client must use the same API key.

For this example, the API key is:

```text
api-key-broadridge-demo
```

Requests without a valid API key return:

```text
Missing or invalid API key.
```

## API Endpoint

**POST** `/api/weather/stationaggregate`

The API accepts newline-delimited JSON (NDJSON) measurements and returns the aggregated results in the same format.

## Example

**Input:**

```text
{"stationName":"Hamburg","temperature":12.0}
{"stationName":"Bulawayo","temperature":8.9}
{"stationName":"HAMBURG","temperature":34.2}
{"stationName":"st. john's","temperature":-3.5}
{"stationName":"ST. JOHN'S","temperature":-4.5}
```

**Output:**

```text
{"stationName":"Bulawayo","min":8.9,"mean":8.9,"max":8.9}
{"stationName":"Hamburg","min":12.0,"mean":23.1,"max":34.2}
{"stationName":"St. John's","min":-4.5,"mean":-4.0,"max":-3.5}
```

## Assumptions

- Station names must contain between 1 and 100 UTF-8 bytes.
- Temperatures must be between -99.9 and 99.9, with one decimal place.
- Station names are matched without considering uppercase or lowercase letters.
- Output station names use an uppercase first letter for each word.
- Results are returned in alphabetical order in JSON format.

## Design and Performance

- API key validation is handled through middleware.
- A case-insensitive dictionary is used to group measurements by station.
- Only the minimum, maximum, total and count are stored for each station.
- Temperature calculations are handled by the `StationTemperature` class in a separate file.
- Running the application in Release mode provides better performance.
