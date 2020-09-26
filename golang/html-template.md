## html/template

import "html/template"

import "log"

### Example usage

w := StringIO // need to figure this out

files := []string{

    "./path/to/sample.tmpl",

    "./path/to/sample.layout.tmpl",

}

t, err := template.ParseFiles(files...)

if err != nil {

    log.Println("error loading files", err.Error())

}

err = t.Execute(w, nil)

if err != nil {

    log.Println("Failed to execute template", err.Error())

}

### Template markup

#### {{}template "blah" .}}

Import the named template into this space

#### {{define "blah" }} ... {{end}}

Define a new template that consists of the HTML between the defind and end tags

#### {{block "blah" .}} Optional {{end}}
