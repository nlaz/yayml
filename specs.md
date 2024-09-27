# YAYML Specification

## 1. Introduction

**YAYML (Yet Another YAML)** is a minimal extension of the YAML format that introduces a simple syntax for attaching attributes directly to keys. This enhancement maintains YAML's readability and simplicity while providing a more expressive way to represent data structures with metadata.

---

## 2. Design Goals

- **Simplicity**: Introduce attributes with minimal changes to the standard YAML syntax.
- **Readability**: Keep configurations clean and easy to read.
- **Expressiveness**: Allow direct association of attributes with keys without additional nesting.
- **Compatibility**: Ensure that existing YAML tools can be adapted to support YYAYML with minimal effort.

---

## 3. Syntax Overview

### 3.1. Keys with Attributes

- Attach attributes to a key using parentheses `()` immediately after the key name.
- Attributes are specified as `attribute_name = attribute_value` pairs within the parentheses.
- Multiple attributes are separated by commas `,`.

**Syntax:**

```yaml
key_name(attribute1=value1, attribute2=value2, ...): value
```

### 3.2. Values

- Values follow the standard YAML types:
  - **Scalars**: Strings, numbers, booleans, null.
  - **Sequences**: Ordered lists denoted by `-`.
  - **Mappings**: Key-value pairs indented under the parent key.

### 3.3. Indentation

- Indentation denotes structure and hierarchy, following YAML's indentation rules.
- Consistent indentation is crucial for correct parsing.

---

## 4. Detailed Syntax Specification

### 4.1. Key Names

- Must conform to YAML's rules for key names.
- Can be unquoted strings if they don't contain special characters.
- If the key name contains special characters, spaces, or starts with a number, it should be quoted.

### 4.2. Attributes

- Enclosed in parentheses `()` with no space between the key name and the opening parenthesis.
- Each attribute is a key-value pair using `=`.
- Attribute values can be:
  - **Unquoted Strings**: For simple values without spaces or special characters.
  - **Quoted Strings**: Use single `'` or double `"` quotes if the value contains spaces or special characters.
  - **Numbers**: Integer or floating-point numbers.
  - **Booleans**: `true` or `false` (case-insensitive).
  - **Null**: Represented as `null` or `~`.

**Example:**

```yaml
item(id=123, name="Sample Item", active=true): value
```

### 4.3. Values

- Follow the colon `:` after the key (and attributes).
- Can be:
  - **Scalar**: Direct value following the colon.
  - **Sequence**: List of items, each prefixed with `-`.
  - **Mapping**: Indented key-value pairs under the parent key.

### 4.4. Attribute Values with Special Characters

- If attribute values contain commas `,`, parentheses `()`, or spaces, they must be enclosed in quotes.

**Example:**

```yaml
message(text="Hello, World!", language="en-US"): value
```

### 4.5. Sequences with Attributes

- Sequence items can be keys with attributes.
- Each item in a sequence is prefixed with a dash `-` and indented appropriately.

**Example:**

```yaml
items:
  - product(id=1, name="Product A")
  - product(id=2, name="Product B")
```

### 4.6. Self-Closing Elements

- If a key with attributes has no value or content, the colon `:` and value can be omitted.

**Example:**

```yaml
input(type=text, name=username)
```

---

## 5. Examples

### 5.1. Basic Mapping with Attributes

```yaml
user(id=42, role=admin):
  name: Jane Doe
  email: jane.doe@example.com
```

### 5.2. Sequences with Attributes

```yaml
servers:
  - server(ip=192.168.1.1, status=active)
  - server(ip=192.168.1.2, status=inactive)
```

### 5.3. Nested Structures

```yaml
organization(name="Tech Corp"):
  departments:
    - department(id=101, name="Research & Development"):
        employees:
          - employee(id=1001, name="Alice")
          - employee(id=1002, name="Bob")
    - department(id=102, name="Marketing"):
        employees:
          - employee(id=2001, name="Charlie")
```

### 5.4. Mixing Scalars and Complex Structures

```yaml
config(env=production):
  debug: false
  database(host=localhost, port=5432):
    username: admin
    password: secret
```

---

## 6. Parsing Rules

### 6.1. Tokenization

- **Key**: The string before the `(` or `:`.
- **Attributes**: Text inside `()` after the key.
- **Values**: Follows the `:` as in standard YAML.

### 6.2. Parsing Steps

1. **Identify the Key**: Read until `(`, `:`, or end of line.
2. **Check for Attributes**:
   - If `(` is found immediately after the key, parse attributes.
3. **Parse Attributes**:
   - Read characters within `()`, respecting nested parentheses in values.
   - Split attributes on commas `,` not within quotes.
   - Split each attribute on the first `=` not within quotes.
   - Trim whitespace around attribute names and values.
4. **Parse Value**:
   - After the `:`, parse the value according to YAML rules.

### 6.3. Handling Edge Cases

- **Attribute Values with Commas or Parentheses**:
  - Must be quoted to prevent parsing errors.
- **Empty Attributes**:
  - Attributes without a value (e.g., `disabled`) are not allowed; every attribute must have a value.

---

## 7. Differences from Standard YAML

- **Attributes Syntax**: Introduction of `()` after keys to include attributes.
- **Parsing Requirement**: YAYML requires an additional parsing step to handle attributes.
- **Compatibility**: YAYML is not directly compatible with standard YAML parsers without preprocessing.

---

## 8. Use Cases

### 8.1. Configuration Files

- Attach metadata to configuration sections.

**Example:**

```yaml
service(name="User Service", version="2.0"):
  endpoint: /api/users
  timeout: 30
```

### 8.2. Data Serialization

- Represent complex data structures with associated metadata.

**Example:**

```yaml
dataset(id=1234, type="image"):
  name: Sample Dataset
  entries:
    - entry(id=1, label="Cat")
    - entry(id=2, label="Dog")
```

### 8.3. Markup Representation (HTML, SVG)

- Define markup structures in a readable, hierarchical format.

**HTML Example:**

```yaml
div(id=container):
  h1: "Welcome"
  p(class=intro): "This is an introduction."
```

**SVG Example:**

```yaml
svg(width=100, height=100):
  circle(cx=50, cy=50, r=40, stroke=blue, fill=lightblue)
```

---

## 9. Potential Limitations

- **Tool Support**: Requires custom parsing or preprocessing to handle attributes.
- **Standard YAML Compatibility**: YAYML files are not valid YAML if they contain attributes.
- **Attribute Value Types**: Limited to simple data types; complex types may require additional syntax.

---

## 10. Integration with Existing Tools

### 10.1. Preprocessing

- Use a preprocessing step to convert YAYML into standard YAML by transforming attributes into nested mappings.

**Example Transformation:**

YAYML:

```yaml
user(id=42, role=admin):
  name: Jane Doe
```

Transformed YAML:

```yaml
user:
  __attributes__:
    id: 42
    role: admin
  name: Jane Doe
```

### 10.2. Custom Parsers

- Extend existing YAML parsers to recognize and process the attribute syntax.
- Implement additional logic to handle attributes as part of the data model.

---

## 11. Best Practices

- **Consistent Indentation**: Use spaces (commonly two or four) consistently throughout the document.
- **Quoting**: When in doubt, quote attribute values to avoid parsing errors.
- **Attribute Naming**: Use clear and descriptive names for attributes.
- **Avoid Overuse**: Use attributes when they add clarity; avoid cluttering keys with excessive attributes.

---

## 12. Examples in Context

### 12.1. Representing a Menu Structure

```yaml
menu(id=main):
  - item(name="Home", href="/")
  - item(name="About", href="/about")
  - item(name="Contact", href="/contact")
```

### 12.2. Defining API Endpoints

```yaml
api(version=1.0):
  endpoints:
    - endpoint(path="/users", method=GET):
        description: "Retrieve users"
    - endpoint(path="/users", method=POST):
        description: "Create a new user"
```

### 12.3. Workflow Definitions

```yaml
workflow(name="Data Processing", id=789):
  steps:
    - step(order=1, name="Extract Data")
    - step(order=2, name="Transform Data")
    - step(order=3, name="Load Data")
```

---

## 13. Parsing Algorithm (Pseudocode)

```pseudo
function parseYAYML(lines):
    for each line in lines:
        if line contains '(' and ')':
            key, attributes = splitKeyAndAttributes(line)
            attributeDict = parseAttributes(attributes)
        else:
            key = line before ':'
            attributeDict = {}
        value = parseValue(line after ':')
        addToDataStructure(key, attributeDict, value)
    return dataStructure

function splitKeyAndAttributes(line):
    key = text before '('
    attributes = text inside '()'
    return key, attributes

function parseAttributes(attributeString):
    attributes = {}
    tokens = split attributeString on commas not within quotes
    for token in tokens:
        name, value = split token on first '=' not within quotes
        attributes[name.trim()] = parseAttributeValue(value.trim())
    return attributes

function parseAttributeValue(value):
    if value is quoted:
        return unquote(value)
    elif value is numeric:
        return parseNumber(value)
    elif value is boolean:
        return parseBoolean(value)
    elif value is null:
        return null
    else:
        return value  // as string

function parseValue(valueString):
    // Use existing YAML parsing rules
    return parseYAMLValue(valueString)
```

---

## 14. Conclusion

YAYML enhances the YAML format by allowing attributes to be associated directly with keys in a clean and minimal syntax. This addition provides greater expressiveness and can simplify the representation of complex data structures. While YAYML requires some adjustments to parsing, it remains close to YAML's philosophy of simplicity and readability.

---

## 15. References

- **YAML Specification**: [YAML 1.2 Spec](https://yaml.org/spec/1.2/spec.html)
- **YAYML Examples and Use Cases**: Derived from common configurations and markup representations.

---

## 16. Contact and Contributions

For suggestions, contributions, or discussions about YAYML, please consider reaching out to the community or initiating collaborative projects to develop tooling and integrations.