[Q1] Which of the following is NOT a valid JSON object?

```JSON
{ "name": "Smiley",
  "age": 20,
  "phone": "888-123-4567"
  "email": "smiley@xyz.com",
  "happy": true }
```

[Q2] Which of the following is NOT a valid JSON array?

```JSON
[ 1, 2, "dog", "cat", true, false, [1, "dog", null],
  {["pet":"dog"], ["fun":true]} ]
```

[Q3] Consider the following JSON Schema specification:

```JSON
{
  "type": "object",
  "properties":
    { "ItemID": { "type":"string", "pattern":"Item*" },
      "ItemName": { "type":"string" },
      "Price": { "type":"integer", "minimum":10, "maximum":100 },
      "Sellers": { "type":"array", "maxItems":3,
                   "items": { "type":"string" }},
      "Ratings": { "type":"array",
                   "items":
                      { "type": "object",
                        "properties": {"Rater":
                                       {"type": "string", "optional": true},
                                       "Score":
                                       {"type": "integer", "minimum":1,
                                        "maxiumum":5}}}},
      "AvgRating": { "type":"number", "optional":true },
      "FreeShipping": {"type":"boolean" }
    }
}
```

Select, from the choices below, the JSON data that is valid according to the JSON Schema specification above.

``` JSON
{ "ItemID": "Item123",
  "ItemName": "desk",
  "Price": 50,
  "Sellers": ["Amy","Ben"],
  "Ratings": [{"Rater":"Amy", "Score":5}, {"Score":1},
              {"Rater":"Carl", "Score":4}],
  "FreeShipping": true }
correct
```

[Q4] Consider the following JSON data:

``` JSON
{ "A": [1,1,2,2],
  "B": {
    "C":3,
    "D":4
  },
  "E":[5,6,true],
  "F": {
    "G": [null,7]
    }
}

```

Which of the following could NOT be included as part of a JSON Schema specification that is satisfied by the JSON data above? Assume that every letter ("A", "B", "C", ...) appears in the JSON Schema specification exactly once.

```JSON
"B": {"type":"object", "additionalProperties":false,
      "properties": {"C": {"type":"integer"}}}
```
