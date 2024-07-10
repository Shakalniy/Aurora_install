# Установка Platform SDK + Flutter Aurora на Linux (Ubuntu) или WSL

## Установка Aurora PSDK
```shell
aurora-cli psdk --install
```
Выбираем версию PSDK для установки (5.0.0.60).

#### Перезапускаем терминал
```shell
bash  
```
#### Проверка установки
```shell
aurora_psdk sdk-assistant list
```

## Установка Flutter Aurora

#### Обновление зависимостей
```shell
sudo apt-get update
```
#### Получение зависимостей
```shell
sudo apt-get install curl git git-lfs unzip bzip2
```
#### Создание локальной папки для хранения Flutter
```shell
mkdir -p ~/.local/opt
```
#### Копирование Flutter на компьютер
```shell
git clone https://gitlab.com/omprussia/flutter/flutter.git ~/.local/opt/flutter
echo "alias flutter-aurora=$HOME/.local/opt/flutter/bin/flutter" >> ~/.bashrc
exec bash  
``` 
#### Активация Flutter Авроры
```shell
flutter-aurora config --enable-aurora
``` 
#### Проверка установки
```shell
flutter-aurora doctor
```

Если получаете ошибку embedder:

<img width="623" alt="image" src="https://github.com/smmarty/utils_aurora/assets/48598325/dff15e8c-18c9-4b7f-9b99-8e8ae3fdff37">

Необходимо установить embedder вручную.
#### Проверка установки Flutter
```shell
aurora-cli embedder --install
```
Выбираете версию для установки

#### Перезапускаем терминал
```shell
bash
```

## Установка приложения на Аврору

#### Создание приложения для Авроры
```shell
flutter-aurora create --platforms=aurora --template=app --org=com.example example
```

#### Добавление поддержки Авроры
```shell
flutter-aurora create --platforms=aurora --org=com.example .
```

#### Сборка приложения
```shell
flutter-aurora build aurora --release
```

#### Для удобства, копируете полную ссылку на файл RPM
Данная файл, это и есть ваше приложение. Расширение файла .rpm.
Его местоположение после сборки аходится:
<папка проекта>/build/aurora/arm/release/RPMS/
пример:
com.example.mobile-0.1.0-1.armv7hl.rpm

#### Подпись приложения
Скачать тестовый сертификат и ключ можно на сайте.
https://developer.auroraos.ru/doc/software_development/guides/package_signing#public_certificates

После того, как скачали сертификат и ключ, ложите их в папку sign в корневой каталог пользователя. Далее подписываем приложение командой:
```shell
aurora_psdk rpmsign-external sign --key <ПУТЬ к КЛЮЧУ> --cert <ПУТЬ к СЕРТИФИКАТУ> <ПУТЬ К ПАКЕТУ .rpm>
```

#### Копирование приложения в смартфон
```shell
scp $PATH_TO_APP  defaultuser@192.168.2.15:/home/defaultuser/Downloads
```

#### Подключение к смартфону
```shell
ssh defaultuser@192.168.2.15
```

#### Переход в режим ROOT 
Так, как в этом режиме вы получаете полный доступ. Будьте аккуратны, что бы не поломать смартфон.
```shell
devel-su
```
Вводим пароль который вы установили в при активации терминала

#### Установка пакета
```shell
pkcon install-local /home/defaultuser/Downloads/*.rpm -y
```
