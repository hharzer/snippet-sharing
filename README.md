<p align="center">
  <img src="https://raw.githubusercontent.com/QwerMike/snippet-sharing/master/docs/logo.png" width="100">
  <h1 align="center">Snippet Sharing</h1>
</p>

*Snippet Sharing is a service for sharing code/text snippets. It allows you to paste a text snippet, and then you can share the link to it with somebody.*

## Info
You can paste a text snippet, and then you will get two links.  
First link allows you to modify and delete the snippet.  
The second link grants only read-only access.

## Links
- [Functional specifications][1]
- [Architecture][2]
- [SOLID compliant][3]

[1]: https://github.com/QwerMike/dr-guess/blob/master/docs/func-spec-uml.svg
[2]: https://github.com/QwerMike/dr-guess/blob/master/docs/architecture.md
[3]: https://github.com/QwerMike/dr-guess/blob/master/docs/solid-compliant.md

## Storage
For storage we use Firebase Realtime Database, which:
* is cloud-hosted;
* NoSQL;
* always up-to-date, as data is synced across all clients in realtime;
* remains available even if the app goes offline;
* is available from client devices - can be accessed directly from a mobile device or web browser;
* has data validation through the Firebase Realtime Database Security Rules on r/w operations;
* scales across multiple database instances.

We store snippets, which have:
* data - text or code, which cannot be empty;
* title - optional title for the snippet;
* author - optional author of the snippet;
* type - the type of text or code set by the author (currently supports most popular programming and markup languages);
* public_id - unique identifier, which grants view-only access to the snippet;
* private_id - unique identifier, which grants full access to the snippet.

## Resiliency
There are four endpoints in the API, conforming to basic CRUD operations.
Snippets can be viewed with read only or edit rights.
All endpoints have their timeout set to sixty seconds.
All errors are handled properly on the client side.

|    Endpoint    |                       Resiliency                     |
| -------------- |:----------------------------------------------------:|
| createSnippet  | error if no value is in the request body             |
| getSnippet     | error if no snippet with that id                     |
| updateSnippet  | error if no snippet with edit permission by that id  |
| deleteSnippet  | error if no snippet with edit permission by that id  |

## Security
The service is completely anonymous, no user data is stored, hence, there cannot be any breaches.
When creating a snippet the author gets two links - one with edit permission 
and one with view only permission.
Links are generated by Firebase and cannot be bruteforced.

## Monitoring
Monitoring results cannot be found [here](https://github.com/QwerMike/dr-guess/blob/master/docs/monitoring).
Firebase has built-in tools for basic monitoring of:
- Crashes;
- Connections (realtime & overall history);
- Storage usage;
- Performance
  * App startup metrics;
  * Network response metrics;
  * Database load.
- App health;
- Error groups;
- Logs.

They provide metrics for:
- Database;
- Storage;
- Hosting;
- Cloud Functions.

## Telemetry
We use Application Insights ([react-appinsights][4]), which provides us with:
* Request rates, response times, and failure rates - which pages are most popular, at what times of day, see which pages perform best;
* Dependency rates, response times, and failure rates - check if external services are slowing down our app;
* Exceptions - both server and browser exceptions are reported;
* Page views and load performance - reported by users' browsers;
* AJAX calls from web pages - rates, response times, and failure rates;
* React-specific metrics:
  * Tracking of router changes;
  * React components usage statistics.

![ai-app-map](https://raw.githubusercontent.com/QwerMike/snippet-sharing/master/docs/ai_app_map.png)
![ai-search](https://raw.githubusercontent.com/QwerMike/snippet-sharing/master/docs/ai_search.png)

[4]: https://github.com/Azure/react-appinsights
