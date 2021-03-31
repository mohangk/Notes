---
draft: true
title: Net Http
categories:
  - Golang
---
## net/http

###### import "net/http"

###### http.ServerMux

mux := http.NewServerMux

allows registration of routes and their respective functions that handle the requests

###### mux.HandleFunc()

mux.HandlerFunc("/path", example)

func example(w http.ResponseWriter, r *http.Request)

###### mux.Handle

mux

###### http.ResponseWriter

- w.Header().Set(xx,yyy)

- w. WriteHeader() - last step, its sent to the client, cant change headers after this

- Automatically sets 3 headers when sending a response
  
  - Date
  
  - Content-Length
  
  - Content-Type - uses http.DetectContentType() to determine the type. Fails for JSON (thinks its text). Will need to override JSON responses w.Header().Set("Content-Type", "application/json")

- Access the underlying headers w.Header()["blah"] = []string["blahblah"]

##### http.Request

- r.URL.Query().Get("id") --> get query params

- Use strconv.Atoi() to convert to integer

- http.Request.Path --> path of the request

##### http.Dir

##### http.Fileserver

- Automatically sets Content-type - mime.TypeByExtension()

- sanitizes directory paths with path.Clean()

- Range requests are supported
  
  - Content-Range: bytes 100-199/1975

- Last-Modified-Since, If-Modified-Since is transparently supported 

##### http.ServeFile

- Serves individual files 

##### http.Handler interface

type Handler interface {

  ServeHttp(ResponseWriter, *Request)

}



type home struct {}

func (h *home) ServeHttp(w http.ResponseWrite, r *http.Request){

    w.Write([]byte("This is my homepage"))

}



mux := http.NewServerMux()

mux.Handle("/", &home{})
