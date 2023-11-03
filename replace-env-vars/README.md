# Replace env vars in file
Replaces `__TOKENS__` with environment variables in file.

## Usage
#### Inputs
- `filename` File for the replacement.

## Example
```yaml
uses: academyEX/actions/replace-env-vars@master
env:
  FOO: foo
  BAR: bar
with:
  filename: /path/to/file
```
