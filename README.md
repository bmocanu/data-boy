# data-boy
A simple REST and JSON based service for ingesting and serving back data. Useful for all kinds of sensor data aggregations and dynamic web applications.

Data-boy (short DB) is a service that exposes a REST-with-JSON API that allows:
* ingestion of flexible data, without any pre-defined schema (use PUT, POST and DELETE)
* consuming that data using the same API (use GET)
* consume as little CPU and memory as possible, good candidate to run on a Raspberry PI along an e.g. Nginx to serve a web application for dashboarding
* all data and API paths are free for you to choose. There is no formatting, but there is a minimum of type identification (e.g. strings, numbers, booleans)
* (roadmap) store that data to disk so that a restart of the service will not lose the data
* (roadmap) connect it to an MQTT service, have the data moving through topics stored and accessed using the same API as for all the other data

## Ok, let's see an example

Assume that you have a service that collects some calendar events. You want to store those events and retrieve them later. You have data-boy running on port 8000. All the paths below are custom and they don't mean anything (you can make up the paths as you want).

```text
PUT /calendar/today
Content-Type: application/json

[
    {"name": "Daily sync", "time":"09:00"},
    {"name": "ABC Deployment planning", "time":"10:30"},
    {"name": "Lunch with Mary", "time":"12:30"},
    {"name": "ABC UX/UI Design alignment", "time":"16:30"}
]
```

Once you run the above, data-boy will store that data under the _/calendar/today_ path.
Now you have this web application that can function as a dashboard for your project. You want to display these events.

```html
<script>
  fetch('http://localhost:8000/calendar/today')
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      let events = data;

      for (var i = 0; i < events.length; i++){
          // do something with events[i].name
          // do something with events[i].time
      }
    })
</script>
```