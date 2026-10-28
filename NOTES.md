# W01 Notes: Build .NET Applications with C#

SDK: .NET 8 (pinned with `global.json`)

All API requests below were sent through Swagger UI at `http://localhost:5227/swagger` on Oct 28, 2026, in the order shown: GET all, POST, GET by id, PUT, DELETE.

## 1. Pizza list with my added pizza

Request:

```
curl -X 'GET' \
  'http://localhost:5227/Pizza' \
  -H 'accept: text/plain'
```

Status: **200 OK**

Response:

```json
[
  {
    "id": 1,
    "name": "Classic Italian",
    "isGlutenFree": false
  },
  {
    "id": 2,
    "name": "Veggie",
    "isGlutenFree": true
  },
  {
    "id": 3,
    "name": "Suya Chicken",
    "isGlutenFree": false
  }
]
```

Ids 1 and 2 are the module's original seed data. Id 3, "Suya Chicken", is the pizza I added to the `Pizzas` list in `ContosoPizza/Services/PizzaService.cs`.

## 2. CRUD operations

### GET (all pizzas)

Same request and response as section 1.

Status: **200 OK**

### POST (create a pizza)

Request:

```
curl -X 'POST' \
  'http://localhost:5227/Pizza' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{"name": "Hawaii", "isGlutenFree": false}'
```

Status: **201 Created**

Response:

```json
{
  "id": 4,
  "name": "Hawaii",
  "isGlutenFree": false
}
```

Response headers included `location: http://localhost:5227/Pizza/4`, which points to the new pizza.

### GET (single pizza, confirming the POST)

Request:

```
curl -X 'GET' \
  'http://localhost:5227/Pizza/4' \
  -H 'accept: text/plain'
```

Status: **200 OK**

Response:

```json
{
  "id": 4,
  "name": "Hawaii",
  "isGlutenFree": false
}
```

### PUT (update a pizza)

Request:

```
curl -X 'PUT' \
  'http://localhost:5227/Pizza/4' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{"id": 4, "name": "Hawaiian", "isGlutenFree": false}'
```

Status: **204 No Content**

Response: empty body. This renamed pizza 4 from "Hawaii" to "Hawaiian".

### DELETE (remove a pizza)

Request:

```
curl -X 'DELETE' \
  'http://localhost:5227/Pizza/4' \
  -H 'accept: */*'
```

Status: **204 No Content**

Response: empty body. Pizza 4 was removed from the list.

## 3. Sales summary function

From `mslearn-dotnet-files/Program.cs`:

```csharp
void GenerateSalesSummaryReport(IEnumerable<string> salesFiles, string storesDirectory, string reportPath)
{
    // Force US formatting so the report shows "$1,234.56" no matter what
    // culture the machine running it is set to.
    var culture = CultureInfo.GetCultureInfo("en-US");

    var fileTotals = new List<(string Name, double Total)>();

    foreach (var file in salesFiles)
    {
        string salesJson = File.ReadAllText(file);
        SalesData? data = JsonConvert.DeserializeObject<SalesData?>(salesJson);

        // Relative path, because every store's file is called sales.json
        // and bare file names would be impossible to tell apart.
        var name = Path.GetRelativePath(storesDirectory, file);
        fileTotals.Add((name, data?.Total ?? 0));
    }

    var grandTotal = fileTotals.Sum(f => f.Total);

    var report = new StringBuilder();
    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");
    report.AppendLine($" Total Sales: {grandTotal.ToString("C", culture)}");
    report.AppendLine();
    report.AppendLine(" Details:");

    foreach (var (name, total) in fileTotals)
    {
        report.AppendLine($"  {name}: {total.ToString("C", culture)}");
    }

    File.WriteAllText(reportPath, report.ToString());
}
```

Generated report (`mslearn-dotnet-files/salesTotalDir/salesSummary.txt`):

```
Sales Summary
----------------------------
 Total Sales: $2,012.20

 Details:
  sales.json: $88.88
  201\sales.json: $501.22
  202\sales.json: $1,234.22
  203\sales.json: $99.00
  204\sales.json: $88.88
```