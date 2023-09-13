# List Specific Line for Multiple Files

In order to list a specific line for multiple files, you can use the following command:

```bash

for file in *.yaml; do grep "NodeSku" "$file"; done

```