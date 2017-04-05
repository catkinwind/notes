* http: multiple response.WriteHeader calls

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Println(r.URL)
        go HandleIndex(w, r)
    })

    fmt.Println("Starting Server...")
    log.Fatal(http.ListenAndServe(":5678", nil))
}

func HandleIndex(w http.ResponseWriter, r *http.Request) {
    w.WriteHeader(200)
    w.Write([]byte("Hello, World!"))
}
```

If a handle function doesn't set response status before calling to *Write*,
Go will automatically set it to 200 (HTTP OK). If the handle function does not
write anything to the response, it's also treated succuessfully handling of 
the request and 200 will also be send back.

If http.ResponseWriter#WriteHeader is called twice, this warning will come out.
We should check whether some *return* is missing or some asynchronouse call is
used improperly.

* References:
http://stackoverflow.com/questions/27972715/multiple-response-writeheader-calls-in-really-simple-example

