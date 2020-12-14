- [1. PowerShell Profile with Permanent Alias](#1-powershell-profile-with-permanent-alias)
- [2. Add environment path from power shell](#2-add-environment-path-from-power-shell)
- [3. Run VSCode from PowerShell](#3-run-vscode-from-powershell)
- [4. Edit the PowerShell Profile with VSCode](#4-edit-the-powershell-profile-with-vscode)



# 1. PowerShell Profile with Permanent Alias

Want to create a new alias for your PowerShell which saves permanently?
Then you have to save your aliases in your profile file, which is in 6 [locations](https://devblogs.microsoft.com/scripting/understanding-the-six-powershell-profiles/)

# 2. Add environment path from power shell

```Powershell
    $Env:Path += ";c:\temp"
```
For More Check out [here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.1)

# 3. Run VSCode from PowerShell

Open the directory in powershell and type:
```PowerShell
    code .
```
# 4. Edit the PowerShell Profile with VSCode

```PowerShell
    code $Profile
```