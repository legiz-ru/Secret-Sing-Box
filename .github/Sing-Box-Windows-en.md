# Setting Up Sing-Box Client on Windows

### 1) In a separate folder, create a .cmd or .bat file with such content:

```
@echo off
chcp 65001 >nul
setlocal enabledelayedexpansion

rem The link to download client configuration (replace with your own)
set URL=https://example.com/secret175subscr1pt10n/username-VLESS-CLIENT.json

set "ENABLE_DEPRECATED_LEGACY_DNS_SERVERS=true"
set "ENABLE_DEPRECATED_MISSING_DOMAIN_RESOLVER=true"
set "SING_BOX_DIR=%~dp0"
set "SING_BOX_DIR=%SING_BOX_DIR:~0,-1%"

if not exist "%SING_BOX_DIR%\sing-box.exe" (
    echo Downloading Sing-Box...
    echo.
    for /f "tokens=2 delims= " %%u in ('curl -Ls https://api.github.com/repos/SagerNet/sing-box/releases/latest ^| findstr browser_download_url ^| findstr windows-amd64.zip') do (set "ZIP_URL=%%~u")
    curl -L -o "%SING_BOX_DIR%\sing-box.zip" "!ZIP_URL!"
    tar -xf "%SING_BOX_DIR%\sing-box.zip" --strip-components=1 -C "%SING_BOX_DIR%"
    del "%SING_BOX_DIR%\sing-box.zip"
    echo.
)

echo Started Sing-Box
echo Do not close this window while Sing-Box is running
echo Press Ctrl + C to disconnect
echo.

curl -s -o "%SING_BOX_DIR%\client.json" "%URL%"
"%SING_BOX_DIR%\sing-box.exe" run -c "%SING_BOX_DIR%\client.json" --disable-color
```

> [!IMPORTANT]
> Change the link in the 6th line to yours.

-----

### 2) Create a shortcut for this .cmd or .bat file

Next, right-click on the shortcut and select «Properties», then go to the «Shortcut» tab:

![w3](https://github.com/user-attachments/assets/370c8c40-d861-4255-a1b0-e8f081bcae61)

Select «Advanced» and check the «Run as administrator» box (required to start tun interface), then click OK.

-----

### 3) Click on the shortcut to connect to the server

Do not close the terminal window while connected to proxy.

To disconnect, click on the terminal window and then press Ctrl + C.
