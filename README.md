# Install Dependencies
```
npm install
```

# Run Server
```
npm run dev
```

# Run Tests
```
npm run test
```


# Challenge Project API

For your assignment, we need to ensure you can be successful on our team working in the backend api.

This challenge is to build an API for the http server to receive POST requests with information about a visitor and respond with a decision, either: rejection, or accept with a corresponding target url.

Specifically you need to:

1. Set up Git for this repo locally
2. Solve the challenge locally
3. Create a new repo in your GitHub account and note the git url
4. Set your local origin to the new git url: git remote set-url origin ${git url}
5. Push your solution to the newly set origin

You must follow these steps for your solution to be accepted -- forks or other methods will not be considered.

## Decisioning

1) The decisions should be based on information about the visitor that comes in POST request body
  
  ```json
        {
            "geoState": "ca",
            "publisher": "abc",
            "timestamp": "2018-07-19T23:28:59.513Z"
        }
  ```

2) No `target` can receive more traffic per day than it allows. Here's an example `target` with max 10 accepts per day
  
  ```json
        {
            "id": "1",
            "url": "http://example.com",
            "value": "0.50",
            "maxAcceptsPerDay": "10",
            "accept": {
                "geoState": {
                    "$in": ["ca", "ny"]
                },
                  "hour": {
                    "$in": [ "13", "14", "15" ]
                  }
            }
        }
   ```

3) All remaining `targets` should be filtered by criteria they are willing to accept. The above `target` example only accepts visitors from either California or New York from 13 to 16 UTC.

4) If no `targets` are left, the request is rejected (returns `{"decision":"reject"}`), otherwise it should return the url of the remaining `target` with the highest value.

## Requirements

1. New endpoints that needs to be built:
   - **POST** `/api/targets`
      - post a target
   - **GET** `/api/targets`
      - get all targets
   - **GET** `/api/target/:id`
      - get a target by id
   - **POST** `/api/target/:id`
      - update a target by id
   - **POST** `/route`
      - post a request with information about the visitor
      - respond with a decision

2. API functional tests.
   - each endpoint should have its own test
   - write all tests in `test/endpoints.js`
   - be sure to understand how [`servertest`](https://github.com/rvagg/servertest) works

### Further Details

- All persistence should use [redis](http://redis.io).
- Source code should be in `lib` dir.
- Use redis client factory from `lib/redis.js` and do not edit this file.
