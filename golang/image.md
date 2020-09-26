###### Introduction

- Allows reading of image information

```
reader, err := os.Open("testdata/video-001.q50.420.jpeg")
if err != nil {
     log.Fatal(err)
}
defer reader.Close()

//read from string
//reader := base64.NewDecoder(base64.StdEncoding, strings.NewReader(data))
m, _, err := image.Decode(reader)
if err != nil {
    log.Fatal(err)
}
```

[The Go image package - The Go Blog](https://blog.golang.org/image)
