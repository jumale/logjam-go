# Logjam client for Go

This package provides integration with the [Logjam](https://github.com/skaes/logjam_core) monitoring tool for Go web-applications.

It buffers all log output for a request and sends the result to a configured AMQP broker when the request was finished.

## Requirements
This package depends on [github.com/pebbe/zmq4](https://github.com/pebbe/zmq4) which requires ZeroMQ version 4.0.1 or above. 
Make sure you have it installed on your machine.

E.g. for mac:
```bash
brew install zmq
```

## How to use it
Install via `go get github.com/xing/logjam-go` and then inside your code create a handler wrapper like this:

```go
// Wrap HTTP handler with logjam logging
func handlerWithLogjam(handler http.Handler) http.Handler {
	return logjam.NewMiddleware(handler, &logjam.Options{
		AppName: "MyApp",
		EnvName: "production",
		Logger:  log.New(os.Stderr, "API", log.LstdFlags),
	})
}
```

Then wrap your HTTP handler before passing it to server:

```go
    r := mux.NewRouter()
    ...
    http.ListenAndServe(":80", handlerWithLogjam(r))
```

This example uses the Gorilla Mux package but it should also work with any HTTP handler implementation.

You also need to set environment variable to point to the actual logjam broker instance:

`export LOGJAM_BROKER=my-logjam-broker.host.name`

## Hot to contribute?
Please fork the repo and create a pull-request for us to merge.
