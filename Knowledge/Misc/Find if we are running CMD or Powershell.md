```bash
# Will output either CMD or PowerShell based on the engine it runs on
(dir 2>&1 *`|echo CMD);&<# rem #>echo PowerShell

# If PowerShell: Delimiter is ;
# If CMD: Delimiter is &
```
