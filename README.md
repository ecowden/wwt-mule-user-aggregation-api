# Mule Playground 01

**Goal**: Learn about Mulesoft's tools by building a contrived project from scratch that touches on key patterns, such as aggregation and transformation.

## Phase 1: The User Aggregator

Create an endpoint, `GET /aggregates`, which fetches multiple Users from our existing user API.

**Activities & Patterns Exercised**

* Invoking WWT APIs
* Aggregation of data between multiple requests
* Data Transformation

**Request**

The request should allow a comma-separated list of user names to fetch. E.g., `/GET /aggregates?usernames=cowdene,sharmab,doesnotexist`

**Response**

The response should transform users into a JSON structure that looks like,

```json
{
  "found": [
    {
      "username": "cowdene",
      "id": "101",
      "firstName": "Evan",
      "lastName": "Cowden",
      "joinedName": "Evan Cowden"
    },
    {
      "username": "sharmab",
      "id": "202",
      "firstName": "Bob",
      "lastName": "Sharma",
      "joinedName": "Bob Sharma"
    },
  ],
  "missing": [
    {
      "username": "doesnotexist"
    }
  ]

}
```

**Key Criteria**:

* **Found vs. missing** users should be categorized as such. The service should not fail if one or more usernames don't match users. Instead, categorize requested users into `found` and `missing` properties.
* **Extra fields** should be dropped. The userservice gives us more data than we want, and we want to deliberately strip it from the response.
* **`joinedName` should be a composite** of `firstName` and `lastName`. Yes, I know that the userservice gives us a `fullName` property, but let's practice transformation by creating a new field from two existing ones.
* **Multiple, parallel requests** to the userservice. We should make multiple calls, one for each given username, and those requests should be in parallel. Yes, I know that the userservice has an endpoint where we can retrieve this data in a single bulk request, but let's not use it.
* **Malformed requests should render an error response**. That is, if the request doesn't have a username query parameter, we should respond with a `400 BAD REQUEST` status code and a descriptive message.
