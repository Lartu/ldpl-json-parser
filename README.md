![](https://www.ldpl-lang.org/graphics/other/ldplsaur.png)

# LDPL JSON Parser
This is JSON parsing library for LDPL that turns JSON strings into LDPL maps
as per [RFC 6901](https://datatracker.ietf.org/doc/html/rfc6901).

Basically, a JSON is provided as a string and its keys are turned into JSON
Pointer URIs, which are used to store and access its values in a flat
`MAP OF TEXTS` as `TEXT` values. Optionally, another `MAP OF TEXTS` can be
generated to store the original types of the JSON values (`"scalar"` for JS
scalar values, `"object"` for nested JSON objects or `"array"` for JS arrays).

# Example
```lpdl
INCLUDE "jsonlib.ldpl"

DATA:
json_string IS TEXT
json_values IS MAP OF TEXTS
json_types IS MAP OF TEXTS

PROCEDURE:
STORE QUOTE IN json_string
{
    "foo": ["bar", "baz"],
    "": 0,
    "a/b": 1,
    "c%d": 2,
    "e^f": [2, 3, [4, 5]],
    "g|h": 4,
    "i\\j": 5,
    "k\"l": 6,
    "sub": {
        "sub-a": 10,
        "sub-b": "robert",
        "sub-c": [1, {"pig": "cow", "1": 2}, 3, {
            "sub-sub-a": 4,
            "sub-sub-b": ["Lala", "Po"]
        }]
    },
    " ": ":)",
    "m~n": 8
}
END QUOTE
PARSE JSON json_string VALUES INTO json_values
DISPLAY "The " json_values:"/sub/sub-c/1/pig" " goes moooo!" CRLF
# That last line prints "the cow goes moooo!"
```

# Included Statements
```
PARSE JSON <TEXT> VALUES INTO <MAP OF TEXTS> AND TYPES INTO <MAP OF TEXTS>
```
Parses the JSON string in the first parameter, storing its key URIs as the keys of the first MAP OF TEXTS and its values as the values of the first MAP OF TEXTS. Using the same key URIs, the types of the stored values are stored in the second MAP OF TEXTS.

```
PARSE JSON <TEXT> VALUES INTO <MAP OF TEXTS>
```
Parses the JSON string in the first parameter, storing its key URIs as the keys of the first MAP OF TEXTS and its values as the values of the first MAP OF TEXTS.

# License

This library is released under the MIT license.
