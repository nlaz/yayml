# YAYML (Yet Another YAML)

YAYML is a minimal extension of the YAML format that introduces a simple syntax for attaching attributes directly to keys. This enhancement maintains YAML's readability and simplicity while providing a more expressive way to represent data structures with metadata.

## Features

- Simple attribute syntax using parentheses `()`
- Compatible with existing YAML structures
- Improved expressiveness for complex data representations

## Quick Start

1. Check out the `specs.md` file for the full YAYML specification.
2. Try out the YAYML to HTML Editor (`Improved YAYML to HTML Editor.html`) to see YAYML in action.

## Example

```yaml
user(id=42, role=admin):
  name: Jane Doe
  email: jane.doe@example.com
```

## Contributing

Contributions are welcome! Please read the specification and feel free to submit pull requests or open issues for discussions.

## License

[MIT License](LICENSE)
