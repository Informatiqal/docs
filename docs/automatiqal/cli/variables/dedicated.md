# Dedicated file

Dedicated file that store the variables and their values. The file can be of any extension. `Automatiqal` will read it as an text file.

The structure of the file is simple:

- each variable definition on new line
- `variable-name=variableValue`

Example:

```yaml
host=my-sense-instance.com
certificate=path\to\client.pem
certificate_key=path\to\client_key.pem
user_dir=USER_DIR
user_name=user_name
qvfLocation=path\to\Cars.qvf
extension_path=path\to\Test.zip
contentLib_file1=path\to\ko-fi.png
contentLib_file1_name=ko-fi.png
contentLib_file2=path\to\ko-fi-blue.png
contentLib_file2_name=ko-fi-blue.png
```

In order to use the file:

- `automatiqal --file runbook.yaml --variables path\to\my-variables.txt`
- `automatiqal --file runbook.yaml --var path\to\my-variables.txt`
- `automatiqal --file runbook.yaml -v path\to\my-variables.txt`
