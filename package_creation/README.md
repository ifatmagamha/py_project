# Package Creation

A simple Python package demonstrating string manipulation operations.

## Features

- **Reverse String**: Reverse any string
- **Count Vowels**: Count vowels in a string
- **Capitalize Words**: Capitalize the first letter of each word

## Installation

```bash
pip install ifatmagamha-package-creation
```

## Usage

```python
from package_creation.string_ops import reverse_string, count_vowels, capitalize_words

# Reverse a string
result = reverse_string("hello world")
print(result)  # Output: dlrow olleh

# Count vowels
count = count_vowels("hello world")
print(count)  # Output: 3

# Capitalize words
capitalized = capitalize_words("hello world")
print(capitalized)  # Output: Hello World
```

## Development

### Setup

```bash
# Clone the repository
git clone https://github.com/ifatmagamha/py_project.git
cd py_project/package_creation

# Install dependencies
poetry install

# Run tests
poetry run python -m unittest tests.test_string_ops -v
```

### Building

```bash
poetry build
```

### Testing

```bash
poetry run python -m unittest discover tests -v
```

## Documentation

Full documentation is available at: https://ifatmagamha.github.io/py_project/

## License

MIT License

## Author

ifatmagamha (gamhaif@gmail.com)
