# GetSHAValues
A simple reg key that adds a context menu to retrieve file SHA values with powershell. Simply import / merge the reg key to get the context menu. Intended for Windows 10.


    Windows Registry Editor Version 5.00

    [HKEY_CLASSES_ROOT\*\shell\Get SHA Values]
    @="Get SHA Values with PowerShell..."
    "Icon"="\"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\""

    [HKEY_CLASSES_ROOT\*\shell\Get SHA Values\command]
    @="powershell.exe -WindowStyle hidden $FileHashes = 'SHA1', 'SHA256', 'SHA384', 'SHA512' | ForEach-Object -Begin {$OutputObject = New-Object System.Collections.ArrayList} -Process {$FileHash = Get-FileHash -Path '%1' -Algorithm $_; $FileSize = (Get-Item '%1').length / 1KB; $OutputObject.Add([PSCustomObject]@{ Algorithm = $_; Hash = $FileHash.Hash; Path = $FileHash.Path; SizeKB = $FileSize })} -End {$OutputObject | Out-GridView -Title 'Get SHA Values' -PassThru}"
