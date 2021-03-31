---
draft: true
title: Net Http (Conflicting Copy)
categories:
  - Golang
---
## net/http

###### import "net/http"

###### http.ServerMux

mux := http.NewServerMux

allows registration of routes

mux.HandleFunc()

###### handleFunc

func example(w http.ResponseWriter, r *http.Request)

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
