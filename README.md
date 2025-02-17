Graphigo
========

[![Build Status](https://travis-ci.org/fgrosse/graphigo.svg?branch=master)](https://travis-ci.org/fgrosse/graphigo)
[![GoDoc](https://godoc.org/gopkg.in/fgrosse/graphigo.v2?status.svg)](https://godoc.org/gopkg.in/fgrosse/graphigo.v2)

A simple go client for the [graphite monitoring tool][1].

## Features

- simple and clean API
- send a whole bunch of metrics and send them all with one TCP call
- supports timeouts
- supports prefixes
- all client functions are automatically noops if the client is `nil`
- good test coverage and documentation
- stable API via gopkg.in

## Installation

Use `go get` to install graphigo:
```
go get gopkg.in/fgrosse/graphigo.v2
```

No additional dependencies are required.

## Usage

```go
package main

import "gopkg.in/fgrosse/graphigo.v2"

func main() {
    c := graphigo.NewClient("graphite.your.org:2003")

	// set a custom timeout (seconds) for the graphite connection
	// if timeout = 0 then the graphigo.DefaultTimeout = 5 seconds is used
	// Setting Timeout to -1 disables the timeout
	c.Timeout = 0

	// set a custom prefix for all recorded metrics of this client (optional)
	c.Prefix = "foo.bar.baz"

	if err := c.Connect(); err != nil {
		panic(err)
	}

	// close the TCP connection properly if you don't need it anymore
	defer c.Close()

	// capture and send values using a single line
	c.SendValue("hello.graphite.world", 42)

	// capture a metric and send it any time later
	metric := graphigo.Metric{Name: "test", Value: 3.14} // you can use any type as value
	defer c.Send(metric)

	// create a whole bunch of metrics and send them all with one TCP call
	c.SendAll([]graphigo.Metric{
		{Name: "shut", Value: 1},
		{Name: "up", Value: 2},
		{Name: "and", Value: 3},
		{Name: "take", Value: 4},
		{Name: "my", Value: 5},
		{Name: "money", Value: 6},
	})
}
```

## Alternatives

- https://github.com/marpaia/graphite-golang
- https://github.com/ohlol/graphite-go

## Contributing

Any contributions are always welcome (use pull requests).
Please keep in mind that I might not always be able to respond immediately but I usually try to react within the week ☺.

[1]: http://graphite.readthedocs.org/en/latest/overview.html
[2]: https://godoc.org/gopkg.in/fgrosse/graphigo.v2
