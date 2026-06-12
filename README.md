Install Winget via PowerShell (if needed, i.e. on Windows Server 2022)
-----------------------------

    Write-Host "Installing WinGet PowerShell module from PSGallery..."
    Install-PackageProvider -Name NuGet -Force | Out-Null
    Install-Module -Name Microsoft.WinGet.Client -Force -Repository PSGallery | Out-Null
    Write-Host "Using Repair-WinGetPackageManager cmdlet to bootstrap WinGet..."
    Repair-WinGetPackageManager -AllUsers


Fix Source Error on Winget option 1
-----------------------------

    winget source reset --force
    winget source update

Fix Source Error on Winget option 2
-----------------------------

    Add-AppxPackage -Path '.\Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle'
    winget -v

Fix Source Error on Winget option for server 2022
-----------------------------

    $r=Invoke-RestMethod 'https://api.github.com/repos/microsoft/winget-cli/releases/latest' -Headers @{'User-Agent'='PowerShell'}; $b=$r.assets|?{$_.name -match 'Microsoft\.DesktopAppInstaller.*\.msixbundle'}|select -First 1; $d=$r.assets|?{$_.name -match 'DesktopAppInstaller_Dependencies.*\.zip'}|select -First 1; $l=$r.assets|?{$_.name -match 'License.*\.xml'}|select -First 1; if(-not $b -or -not $d -or -not $l){Write-Error 'Missing required assets';exit 1}; Invoke-WebRequest $b.browser_download_url -OutFile $b.name;Invoke-WebRequest $d.browser_download_url -OutFile $d.name;Invoke-WebRequest $l.browser_download_url -OutFile $l.name;Expand-Archive -Path $d.name -DestinationPath .\Dependencies -Force;Get-ChildItem .\Dependencies -Recurse -File | ?{$_.Extension -match '\.msixbundle$|\.msix$|\.appx$'} | % { Write-Information "Installing $($_.FullName)"; Add-AppxPackage $_.FullName }; Add-AppxProvisionedPackage -Online -PackagePath .\$($b.name) -LicensePath .\$($l.name) -Verbose



Install Windows Terminal (if needed)
-----------------------------

    winget install --id Microsoft.WindowsTerminal -e


Install PowerShell 7
-----------------------------

    winget install Microsoft.PowerShell


Open Terminal

Install IosevkaTermNerdFontMono-Regular.ttf Nerd Font

Set Font etc under Settings

    winget install JanDeDobbeleer.OhMyPosh
    
restart shell to reload PATH

Set Execution Policy for Current User
------------------------------------
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser


Find Path for Profile
-----------------------------

    echo $profile 

Files
-----------------------------

Copy jandedobbeleer.omp.json to home dir (~)  or c:\users\andy

Copy Microsoft.PowerShell_profile.ps1 to the $profile path


Troubleshooting
----------------    

1. Check that the ps1 file in $profile is 'unblocked'
2. Check that the folder under Documents is PowerShell (tip - I copy paste WindowsPowerShell folder if there and then rename to PowerShell) and place the profile.ps1 file inside
3. Make sure you have run Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
4. Try opening the PowerShell 7 app itself in Admin mode - and run the Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser again.
5. Make sure the jandedobbleleer.omp.json is in your user folder (c:\users\username)



