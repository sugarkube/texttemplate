# Text template
This is a fork of golang 1.12's `text/template` package that adds an extra option to ignore missing keys. By default missing keys are rendered in templates as the string `<no value>`. This fork makes templates output (almost) the original tag (whitespace around delimiters will not be preserved).

E.g. given the template:
```
x={{ .x }}, y={{ .y }}
```
and the variables:
```
map[string]interface{}{
    "x": 10
}
```
This will output:
```
x=10, y={{ .y }}
```

It's not perfect and currently struggles with unset subkeys of maps. This fork includes the changes described in [this issue](https://github.com/golang/go/issues/23488) and in [this changeset](https://go-review.googlesource.com/c/go/+/88596/).
