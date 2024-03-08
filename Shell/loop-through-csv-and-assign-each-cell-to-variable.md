# Loop through CSV and assign each cell to a variable

If you have a CSV file and you want to use each cell as a variable, you can use the following script:

```bash
#!/bin/bash



while IFS=';' read -r column1 column2 column3
do
  column3=$(echo "$column3" | tr -dc '[:print:]')
  az vm update --resource-group "${column2}" --name "${column3}" --subscription "${column1}" --set licenseType=Windows_Server

done < test.csv

```

Because I am using non-English Excel, CSV will be separated by `;` instead of `,`. If you are using English Excel, you can change `IFS=';'` to `IFS=','`.
The reason I am using `tr -dc '[:print:]'` is to remove any non-printable characters from the cell. This is because Excel sometimes adds non-printable characters to the rightmost cell eg. /n or /r. This will cause the script to fail.
