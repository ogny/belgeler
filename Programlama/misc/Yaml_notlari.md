YAML is a cross-programming-language data serialization language whose primary
focus is Human Friendliness. 

First, always use 2 character indentation. YAML accepts any number, but 2
spaces is always best, and it's the way YAML is dumped canonically.

Ornek:
Single line sequence-in-sequence:
  good:
  - - - foo
    - bar
  bad:
    -
      -
        - foo
      - bar
Any YAML can be a YAML string, by using literal style:
  this: |
    foo:
      bar: baz
  is the same as: "foo:\n  bar: baz\n"
Quote escaping:
  double: " backslash \"escaping\" for double quotes "
  single: ' two chars ''escaping'' for single quotes '
Flow style for small lists or dicts:
  colors: [red, white, blue]  # Like JSON without all the double quotes
  fred: {sex: male, age: 42}
Looks like confused JSON:
  set notation: {foo, bar, baz}
  ordered map: [ one: 1, two: 2, three: 3]
Sets as map keys:   # Dice rolls.
  [1, 6]: 3
  [3, 4]: 5
