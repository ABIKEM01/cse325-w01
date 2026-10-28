# W01 Notes: Build .NET Applications with C#

SDK: .NET 8 (paste output of `dotnet --version` here)

## 1. Pizza list with my added pizza

Request:

```
GET http://localhost:<PORT>/pizza
```

Response (200 OK):

```json
PASTE YOUR ACTUAL RESPONSE HERE
```

My added pizza is Id 3, "Suya Chicken".

## 2. CRUD operations

### GET (single item)

```
curl -i http://localhost:<PORT>/pizza/3
```

Status: 
Response:

```
PASTE HERE
```

### POST

```
curl -i -X POST http://localhost:<PORT>/pizza \
  -H "Content-Type: application/json" \
  -d '{"name":"Hawaii","isGlutenFree":false}'
```

Status: 
Response:

```
PASTE HERE
```

### PUT

```
curl -i -X PUT http://localhost:<PORT>/pizza/4 \
  -H "Content-Type: application/json" \
  -d '{"id":4,"name":"Hawaiian","isGlutenFree":false}'
```

Status: 
Response:

```
PASTE HERE
```

### DELETE

```
curl -i -X DELETE http://localhost:<PORT>/pizza/4
```

Status: 
Response:

```
PASTE HERE
```

## 3. Sales summary function

```csharp
PASTE GenerateSalesSummaryReport FROM mslearn-dotnet-files/Program.cs HERE
```

Sample output (salesTotalDir/salesSummary.txt):

```
PASTE THE GENERATED FILE CONTENTS HERE
```