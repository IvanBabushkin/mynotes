# Первоначальная настройка Ubuntu Server после установки
## Настройка сети (netplan)

#### Файл конфигурации сети
```console
sudo nano /etc/netpan/<filename>.yaml
```
#### Пример файла конфигурации static IPv4
```yaml
network:
  ethernets:
    ens18:
      dhcp4: false
      addresses:
      - X.X.X.X/X
      gateway4: X.X.X.X
      mtu: 1400
      nameservers:
        addresses:
        - 1.1.1.1
        - 8.8.8.8
  version: 2
```
#### Применить конфигурацию
```console
sudo netplan generate
sudo netpan apply
```

## Обновление пакетов до последней версии

```console
sudo apt update
sudo apt upgrade -y
```

## Установка дополнительных пакетов
```console
sudo apt install -y openssh-server mc htop software-properties-common fail2ban openntpd curl tzdata iotop
sudo reboot now
```

## Конфигурация sshd
```console
nano /etc/ssh/sshd_config
```
#### Добавить параметры
```console
# Включение протокола sshd v2
Protocol 2
# Авторищация по ключу
PermitRootLogin without-password
PubkeyAuthentication yes
# Запрет авторизации по паролю
PasswordAuthentication no

```
#### Перезапук службы с новыми настройками
```console
sudo systemctl restart sshd.service
```

## Очистка
```console
sudo apt-get purge $(dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | head -n -1)
```

```console
sudo apt-get autoclean
sudo apt-get autoremove
```
```console
sudo dpkg -l | awk '/^rc/ {print $2}' | xargs sudo dpkg --purge
```


