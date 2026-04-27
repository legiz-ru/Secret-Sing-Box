# Настройка клиента Sing-Box на Windows

### 1) В отдельной папке создайте .cmd или .bat файл с таким содержимым:

```
@echo off
chcp 65001 >nul
setlocal enabledelayedexpansion

rem Ссылка для загрузки клиентской конфигурации (замените на свою)
set URL=https://example.com/secret175subscr1pt10n/username-VLESS-CLIENT.json

set "ENABLE_DEPRECATED_LEGACY_DNS_SERVERS=true"
set "ENABLE_DEPRECATED_MISSING_DOMAIN_RESOLVER=true"
set "SING_BOX_DIR=%~dp0"
set "SING_BOX_DIR=%SING_BOX_DIR:~0,-1%"

if not exist "%SING_BOX_DIR%\sing-box.exe" (
    echo Скачивание Sing-Box...
    echo.
    for /f "tokens=2 delims= " %%u in ('curl -Ls https://api.github.com/repos/SagerNet/sing-box/releases/latest ^| findstr browser_download_url ^| findstr windows-amd64.zip') do (set "ZIP_URL=%%~u")
    curl -L -o "%SING_BOX_DIR%\sing-box.zip" "!ZIP_URL!"
    tar -xf "%SING_BOX_DIR%\sing-box.zip" --strip-components=1 -C "%SING_BOX_DIR%"
    del "%SING_BOX_DIR%\sing-box.zip"
    echo.
)

echo Sing-Box запущен
echo Не закрывайте это окно, пока Sing-Box работает
echo Нажмите Ctrl + C, чтобы отключиться
echo.

curl -s -o "%SING_BOX_DIR%\client.json" "%URL%"
"%SING_BOX_DIR%\sing-box.exe" run -c "%SING_BOX_DIR%\client.json" --disable-color
```

> [!IMPORTANT]
> Ссылку в 6-ой строчке замените на свою.

-----

### 2) Создайте ярлык для этого .cmd или .bat файла

Далее нажмите на ярлык правой кнопкой мыши и выберите «Свойства», затем перейдите во вкладку «Ярлык»:

![w3](https://github.com/user-attachments/assets/370c8c40-d861-4255-a1b0-e8f081bcae61)

Выберите «Дополнительно» и поставьте галочку «Запуск от имени администратора» (требуется для запуска tun интерфейса), потом везде нажмите OK.

-----

### 3) Для подключения к прокси просто нажмите на ярлык

Не нужно закрывать появившееся окно, пока ПК подключён к прокси.

Чтобы отключиться, нажмите на окно командной строки, а далее Ctrl + C.
