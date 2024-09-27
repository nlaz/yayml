# YAYML (Yet Another YAML)

YAYML is a minimal extension of the YAML format that introduces a simple syntax for attaching attributes directly to keys. This enhancement maintains YAML's readability and simplicity while providing a more expressive way to represent data structures with metadata.

## Features

- Simple attribute syntax using parentheses `()`
- Compatible with existing YAML structures
- Improved expressiveness for complex data representations

## Quick Start

1. Check out the `specs.md` file for the full YAYML specification.
2. Try out the [YAYML to HTML Editor](https://nlaz.github.io/yayml/editor.html) to see YAYML in action.

## Example

```yaml
div(id=container, class=main):
  h1: Welcome to YAYML
  p(class=intro): This is a simple example of YAYML syntax.
  ul:
    - First item
    - Second item
    - Third item
  button(onclick="alert('hello world');"): Button
```

## Contributing

Contributions are welcome! Please read the specification and feel free to submit pull requests or open issues for discussions.

## License

[MIT License](LICENSE)
