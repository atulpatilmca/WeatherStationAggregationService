# Weather Station Aggregation Service

## How to load the project: double-click the .sln file from the following location
```WeatherStationAggregationService\WeatherStationService\WeatherStationService.sln


## How to Run

Build the solution:
```bash (Tools->Command Line->Developer Command Prompt:)
dotnet build
```

Start the Weather Station Service:

```bash (Tools->Command Line->Developer Command Prompt:)
dotnet run
```

Open another command line and start the client:

```bash
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


```bash
dotnet run -c Release
```

## Using Postman tool 
```Visual Studio code , Ctr + F5 (Run)

```Postman tool
```Type : Post
```URL: http://localhost:5000/api/weather/stationaggregate

```Headers
```X-API-Key      api-key-broadridge-demo
```Content-Type   application/x-ndjson

```Body
 ```raw -> JSON 
``` Input Data 
```{"stationName":"Hamburg","temperature":12.0}
```{"stationName":"Bulawayo","temperature":8.9}
```{"stationName":"Hamburg","temperature":34.2}
```{"stationName":"Bulawayo","temperature":65.2}
```{"stationName":"Bulawayo","temperature":75.2}

```OutPut Data 
```{
```    "stationName": "Bulawayo",
```    "min": 8.9,
```    "mean": 49.8,
```    "max": 75.2
```}
```{
```    "stationName": "Hamburg",
```    "min": 12.0,
```    "mean": 23.1,
```    "max": 34.2
```}

## API Key

The service requires an API key. Add it to `WeatherStationService/appsettings.json` under `ApiKey`.


The client side  `ApiKey` and `ServiceUrl` from `/appsettings.json`.

Both the service and client use the same API key. Both having same key `api-key-broadridge-demo`

Requests without a valid key return `Missing or invalid API key.`.

## API Endpoint

**POST** `/api/weather/stationaggregate`

The API accepts JSONL data and returns the results in the same format.

## Example

**Input:**

```json
{"stationName":"Hamburg","temperature":12.0}
{"stationName":"Bulawayo","temperature":8.9}
{"stationName":"HAMBURG","temperature":34.2}
{"stationName":"st. john's","temperature":-3.5}
{"stationName":"ST. JOHN'S","temperature":-4.5}
```

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



