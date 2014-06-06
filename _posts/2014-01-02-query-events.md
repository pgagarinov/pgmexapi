---
category: Applications
path: '/events'
title: 'Query Events'
type: 'GET'

layout: nil
---

Returns a list of Events

# Filters

Filter parameters allow querying a subset of the available events.

The data retrieved will allways be sorted by the event creation date.

* `start_at` - the system will return a maximum of 1000 events, to retrieve the next batch of events the `start_at` parameter must be used

`start_at=1001`

* `since` - retrieve events reported after a specific date time.

`/events?since=2014-04-10T15:22:12+0000`

* `until` - retrieve events reported before a specific date time. Useful to define a time window in combination with “since”.

`/events?until=2014-04-10T15:22:12+0000`

* `location` - filter events by location id

`/events?location=1`

* `institution` - filter events by institution id

`/events?institution=1`

* `device` - filter events by device id

`/events?device=1`

* `laboratory` - filter events by laboratory id

`/events?laboratory=1`

* `gender` - filter events by gender

`/events?gender=male`

* `min_age` - filter events for people of age greater or equal than min_age

`/events?min_age=7`

* `max_age` - filter events for people of age lower or equal than max_age

`/events?max_age=7`

* `result` - filter by the events outcome.

`/events?result=positive`

* `assay_name` - filter events for a particular assay name

`/events?assay_name=MTB`

* `assay` - filter events for a particular assay ID

`/events?assay=ASSAY001`

* `uuid` - retrieves the event with a particular UUID

`/events?uuid=c4c52784-bfd5-717d-7a91-614acd972d5e`

# Sorting

* `order_by` - orders by a given field.
* Ascending by default. Append a `-` to sort descending.
* Comma separated.

### Examples

* `order_by=age` - orders ascending by age.
* `order_by=age,gender` - orders ascending by age and gender.
* `order_by=-age` - orders descending by age.
* `order_by=age,-gender` - orders ascending by age and descending by gender.

# Data Aggregation

There are two ways to accomplish data aggregation:

* through the “group_by” option in the query string
* sending a JSON in the request body with the "group_by" key in it

## Query Parameter

In the query parameter the options are limited to indicate a couple of fields to group by

`/events?group_by=gender,result`

* `year() | month() | week() | day()` - groups the given date field by the time interval specified. This parenthesis should be escaped when used in the query string.

`/events?group_by=year(created_at)`

`/events?group_by=year%29created_at%29`

## JSON in request body

The JSON allows more complex aggregations, such as age ranges.

* `age` - groups and filters by age ranges. The events are skipped if they are outside those ranges.

`{
  "group_by" : [
    { "age" : [ [0, 10], [11, 20] ] },
    ...
  ]
}`

* `location_depth` - groups by location depth, up to the third level in this case, which is a state level.

`{
  “group_by” : [
    { “location_depth" : 3 },
    ...
  ]
}`

* `time_interval` - groups by a given time interval, it could be one of: yearly, monthly, weekly, daily.

`{
  “group_by” : [
    { “time_interval” : [
      { "created_at" : "yearly" },
      ...
    ] },
    ...
  ]
}`

# Response

`Status: 200 OK`

## Without Grouping

Returns an array of events without any PII:
`[
{
  "assay" : "ASSAY001",
  "assay_name" : "MTB",
  "device_serial_number" : "123456789",
  "result" : "positive",
  "start_time" : "2014-04-24T17:16:03+0000",
  "system_user" : "jdoe",
  "age" : "21",
  "created_at" : "2014-04-24T17:16:03+0000",
  "device_id" : 2,
  "laboratory_id" : 3,
  "institution_id" : 4,
  "location_id" : 5,
  "parent_locations" : [1, 2, 3],
  "uuid" : "c4c52784-bfd5-717d-7a91-614acd972d5e"
},
{
  "assay" : "ASSAY002",
  "assay_name" : "MTB",
  "device_serial_number" : "123456789",
  "result" : "positive",
  "start_time" : "2014-04-24T17:16:03+0000",
  "system_user" : "jdoe",
  "age" : "21",
  "created_at" : "2014-04-24T17:16:03+0000",
  "device_id" : 2,
  "laboratory_id" : 3,
  "institution_id" : 4,
  "location_id" : 5,
  "parent_locations" : [1, 2, 3],
  "uuid" : "c4c52784-bfd5-717d-7a91-614acd972d5e"
},
...
]`

## With Grouping

Returns the quantity of events matching each combination of aggregated fields.

`/events?group_by=gender,result`

`[
  {
    "gender" : "male",
    "result" : "positive",
    "count" : 23
  },
  {
    "gender" : "male",
    "result" : "negative",
    "count" : 10
  },
  {
    "gender" : "female",
    "result" : "positive",
    "count" : 2
  },
  {
    "gender" : "female",
    "result" : "negative",
    "count" : 30
  },
]`

For errors responses, see the [response status codes documentation](#http-response-codes).