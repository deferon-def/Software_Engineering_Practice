### 1: Установка и настройка SSH-сервера
1.1 Заходим на виртуальную машину и запускаем тереминал  
    
- В терминале вводим команду  
`sudo apt-get install openssh-server`

1.2 Проверяем работает ли ssh-сервер. Вводим команду 

- В терминале вводим команду  
`sudo systemctl status ssh`

- Ответ должен выглядить примерно так:

- ssh.service - OpenBSD Secure Shell server  
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)  
     Active: active (running) since Thu 2025-10-30 18:35:12 UTC; 2h 49min ago  
TriggeredBy: ● ssh.socket


### 2: Подключение к удаленному серверу (Windows 10 powershell)
-  Запускаем Windows powershell (от имени администратора)  
- В терминале вводим команду  
`ssh имя пользователя@ip адрес сервера к которому подключаемся`  
- Пример
`ssh user@192.168.10.100`  
- Далее программа запросит пароль от учетной записи  
- Вводим пароль и нажимаем Enter  
- Если все ввели верно увидите что строка изменилась на заголовок вида  
`user@ubunted: $`  
- Поздравляю вы подключились к удаленному серверу по ssh

### 3: Копирование файлов с помощью SCP (Windows 10 powershell)
3.1 Копирование файлов с помощью SCP (Windows 10 powershell) на сервер
- Запускаем Windows powershell (от имени администратора)  
- В терминале вводим команду  
`scp "путь_к_файлу" пользователь@адрес_сервера:путь_на_сервере`  
Пример  
`scp "C:\Users\Deferon\Documents\04.practice_ssh.pdf" user@192.168.10.100:~ `

3.2 Копирование файлов с помощью SCP (Windows 10 powershell) с сервера
- Запускаем Windows powershell (от имени администратора)  
- В терминале вводим команду  
`scp пользователь@адрес_сервера:путь_к_файлу "путь_куда_хотим_скопировать_файл_на_нашем_компьтере"`  
Пример  
`scp "user@192.168.10.100:~.04.practice_ssh.pdf "D:\Case3_SCP `

### 4: Работа с ключами SSH  (Windows 10 powershell)
- Запускаем Windows powershell (от имени администратора) 
- Для генерации пары ключей в терминале вводим команду  
`ssh-keygen -t rsa -b 4096 -C "ваш_имя_ключа"`
- Дальше воспользуемся скриптом
```
# Настройка переменных
$Server = "IP"    # Вместо IP ведите ip вашего сервера
$User = "NAME"    # Вместо NAME введите Имя пользователя на сервере
$KeyPath = "$env:USERPROFILE\.ssh\id_rsa.pub"  # Путь к публичному ключу

# Проверка наличия ключа
if (-Not (Test-Path $KeyPath)) {
    Write-Host "Публичный ключ не найден, создаем пару ключей..."
    ssh-keygen -t rsa -b 4096 -f "$env:USERPROFILE\.ssh\id_rsa" -N ""
}

# Чтение публичного ключа
$PublicKey = Get-Content $KeyPath -Raw

# Отправка ключа на сервер вручную
ssh $User@$Server "mkdir -p ~/.ssh; echo '$PublicKey' >> ~/.ssh/authorized_keys; chmod 700 ~/.ssh; chmod 600 ~/.ssh/authorized_keys"

# Проверка подключения
Write-Host "Пробуем подключиться без пароля..."
ssh $User@$Server
```
