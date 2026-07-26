<div align="center">
  <img width="300" height="300" alt="Aegis"
       src="https://github.com/user-attachments/assets/5a23d553-6e37-4f45-8540-b9685fa995c8" />
</div>

# Aegis

Aegis - утилита для автоматизации запуска стресс-тестов, сервисных проверок ПК и обновления NVIDIA-драйверов. Программа запускает внешние приложения по аргументам командной строки, выполняет действия в нужной последовательности, может отправлять скриншот экрана в Telegram и вести отдельные журналы работы.

## Возможности

- Автоматизация AIDA64 для запуска стресс-тестов CPU, FPU, cache, memory, disks и GPU.
- Автоматизация Victoria для тестирования до трех выбранных томов.
- Автоматизация FurMark для стресс-теста видеокарты.
- Автоматическое определение разрешения экрана для FurMark.
- Автоматическое обновление NVIDIA драйвера с установкой панели управления NVIDIA.
- Отложенная отправка скриншота экрана в Telegram с именем ПК и локальным IP-адресом.
- Автоматическое создание `settings.config`, если файл отсутствует.
- Раздельное логирование в `logs\Aegis.log` и `logs\AegisGPU.log`.

## Команды запуска

| Ключ | Описание |
| --- | --- |
| `/AIDA` | Запуск AIDA64 и выбранных стресс-тестов. |
| `/VICTORIA` | Запуск Victoria и тестирование выбранных томов. |
| `/FURMARK` | Запуск FurMark для стресс-теста видеокарты. |
| `/TIME` | Отложенная отправка скриншота в Telegram. |
| `Display.Driver` | Обновление NVIDIA Display Driver в тихом режиме. |
| `HDAudio.Driver` | Дополнительно установить NVIDIA HD Audio Driver. |
| `-clean` | Выполнить чистую установку NVIDIA-драйвера. |
| `NVIDIA.Panel` | Установить только NVIDIA Control Panel без установки видеодрайвера. |

## AIDA64

Команда `/AIDA` поддерживает параметры:

| Параметр | Описание |
| --- | --- |
| `-CPU` | Стресс-тест CPU. |
| `-FPU` | Стресс-тест FPU. |
| `-CACHE` | Стресс-тест cache. |
| `-MEMORY` | Стресс-тест memory. |
| `-DISKS` | Стресс-тест disks. |
| `-GPU` | Стресс-тест GPU. |

Пример:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /aida -cpu -fpu -cache -memory -disks -gpu
```

## Victoria

Команда `/VICTORIA` поддерживает выбор томов по букве диска:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /victoria -C -D -E
```

Можно указать до трех томов за один запуск. Aegis учитывает, что в Victoria сначала отображаются физические диски, а ниже - тома, поэтому выбор нужного тома выполняется с учетом количества физических дисков в системе.

## FurMark

Команда `/FURMARK` запускает FurMark для стресс-теста видеокарты:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /furmark
```

Ключи `-fullhd`, `-2k` и `-4k` больше не используются: разрешение монитора определяется автоматически.

## NVIDIA Driver

Aegis может найти NVIDIA-видеокарту, определить текущую версию драйвера, найти актуальную версию на стороне NVIDIA, скачать установщик, распаковать его через встроенный 7-Zip и запустить установку.

Тихий режим `-s` и запрет автоматической перезагрузки `-noreboot` используются всегда автоматически, указывать их вручную не нужно.

Примеры:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver
```

Обновит только видеодрайвер NVIDIA в тихом режиме.

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver HDAudio.Driver
```

Обновит видеодрайвер и NVIDIA HD Audio Driver.

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver HDAudio.Driver -clean
```

Обновит видеодрайвер и аудиодрайвер с чистой установкой.

## NVIDIA Control Panel

Если NVIDIA Control Panel не установлена, Aegis установит её через `winget` из Microsoft Store:

```bat
winget install "NVIDIA Control Panel" --id 9NF8H0H7WMLT -s msstore --accept-package-agreements --accept-source-agreements
```

Панель можно установить отдельно, без проверки и установки видеодрайвера:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" NVIDIA.Panel
```

## Временные папки NVIDIA

Во время обновления NVIDIA-драйвера используются временные папки:

```text
%TEMP%\AegisGPU
C:\<версия_драйвера>
```

После успешной установки Aegis удаляет временную папку и распакованную папку драйвера, например `C:\610_74`. Если установка завершилась с ошибкой, распакованная папка остаётся для диагностики.

## Скриншот в Telegram

Команда `/TIME` задает задержку перед созданием скриншота и отправкой его в Telegram.

Поддерживаемые форматы:

| Пример | Описание |
| --- | --- |
| `/time 30s` | Отправить через 30 секунд. |
| `/time 30m` | Отправить через 30 минут. |

Пример:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /furmark /aida -cpu -fpu /time 30m
```

В Telegram отправляется скриншот экрана и подпись:

```text
🖥 Имя ПК: Windows
🌐 Локальный IP: 192.168.0.100
```

## settings.config

Если рядом с `Aegis.exe` нет файла `settings.config`, программа создаст его автоматически со стандартными путями.

Формат файла:

```ini
[Applications]
AIDA=C:\Gizmo Utilities\AIDA64_portable\AIDA64ExtremePortable.exe
Furmark=C:\Gizmo Utilities\FurMark2\furmark.exe
Victoria=C:\Gizmo Utilities\Victoria537\Victoria537\Victoria.exe
```

Если файл есть, Aegis использует пути из него. Если какого-то значения нет, будет использован стандартный путь, заложенный в программе.

## Логи

Во время работы Aegis создает папку:

```text
logs
```

Внутри неё находятся журналы:

```text
logs\Aegis.log
logs\AegisGPU.log
```

`Aegis.log` содержит общий журнал работы программы. `AegisGPU.log` содержит отдельный журнал установки NVIDIA-драйвера и NVIDIA Control Panel.

## Updater.exe

Updater.exe - отдельный компонент для обновления Aegis.

- Проверяет актуальную версию Aegis.exe на GitHub.
- Сравнивает версию на сервере с локальной версией.
- Загружает и устанавливает обновление при необходимости.
- Поддерживает тихий режим через `/s` или `/silent`.

Пример:

```bat
"C:\Gizmo Utilities\Aegis\Updater.exe" /silent
```

## Пример полного запуска

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /furmark /aida -cpu -fpu -memory /victoria -C /time 30m
```

Команды выполняются поочередно, в том порядке, в котором они указаны в аргументах запуска.

## Расположение

Рекомендуемое расположение программы:

```text
C:\Gizmo Utilities\Aegis
```

## Roadmap

- Добавить более подробную отчетность в Telegram.
- Добавить поддержку OCCT.
- Добавить больше диагностической информации в лог-файл.
