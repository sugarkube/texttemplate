# Text template
This is a fork of golang 1.12's `text/template` package that adds an extra option to ignore missing keys. With the `"missingkey=ignore"` option enabled the templater will output more or less the original tag (whitespace around delimiters will not be preserved) if missing map keys are found in the input data. This makes it handy for iteratively processing a template for which you haven't got all the data upfront. Without this option, Go's `text/template` package will output `<no value>` by default. 

Enable this option by setting `Option("missingkey=ignore")` on a `Template` instance.

E.g. given the template:
```
x={{.x | printf "num %d"}} {{.y | printf "y=%s"}}
{{- if .z }}
z={{ .z }}
{{- end }}
nested={{ .a.b.c }}
{{- range $item := .items }}
  id={{ $item.Id }}, name={{ $item.Name }}
{{- end }}
end
```
and the variables:
```
data := map[string]interface{}{
  "x": 99,
  "items": []struct {
    Id   int
    Name string
  }{
    {Id: 3},
    {Id: 4, Name: "testname"},
  },
}
```
This will output:
```
x=num 99 {{ .y | printf "y=%s" }}
nested={{ .a.b.c }}
  id=3, name=
  id=4, name=testname
end
```
Note: The `if` condition has still been evaluated.
