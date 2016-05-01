# Status
![Build Status](https://codeship.com/projects/5a246b70-f088-0133-9388-2640b49afa9e/status?branch=master)
[![Coverage Status](https://coveralls.io/repos/github/mtakaki/cachet-url-monitor/badge.svg?branch=master)](https://coveralls.io/github/mtakaki/cachet-url-monitor?branch=master)

cachet-url-monitor
========================
Python plugin for [cachet](cachethq.io) that monitors an URL, verifying it's response status and latency. The frequency the URL is tested is configurable, along with the assertion applied to the request response.

## Configuration

```yaml
endpoint:
  url: http://www.google.com
  method: GET
  timeout: 0.010 # seconds
  expectation:
    - type: HTTP_STATUS
      status: 200
    - type: LATENCY
      threshold: 1
    - type: REGEX
      regex: ".*<body>.*"
cachet:
  api_url: http://status.cachethq.io/api/v1/
  token: my_token
  component_id: 1
  metric_id: 1
frequency: 30
```

- **endpoint**, the configuration about the URL that will be monitored.
    - **url**, the URL that is going to be monitored.
    - **method**, the HTTP method that will be used by the monitor.
    - **timeout**, how long we'll wait to consider the request failed. The unit of it is seconds.
    - **expectation**, the list of expectations set for the URL.
        - **HTTP_STATUS**, we will verify if the response status code matches what we expect.
        - **LATENCY**, we measure how long the request took to get a response and fail if it's above the threshold. The unit is in seconds.
        - **REGEX**, we verify if the response body matches the given regex.
- **cachet**, this is the settings for our cachet server.
    - **api_url**, the cachet API endpoint.
    - **token**, the API token.
    - **component_id**, the id of the component we're monitoring. This will be used to update the status of the component.
    - **metric_id**, this will be used to store the latency of the API. If this is not set, it will be ignored.
- **frequency**, how often we'll send a request to the given URL. The unit is in seconds.

## Setting up

The application should be installed using **virtualenv**, through the following command:

```
$ git clone git@github.com:mtakaki/cachet-url-monitor.git
$ virtualenv cachet-url-monitor
$ cd cachet-url-monitor
$ source bin/activate
$ pip install -r requirements.txt
```

To start the agent:

```
$ python cachet_url_monitor/scheduler.py config.yml
```