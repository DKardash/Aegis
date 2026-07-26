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
- Автоматическое обновление NVIDIA драйвера с установкой панели управления NVIDIA.
- Установка родного разрешения и максимальной герцовки мониторов со сбросом цветовых настроек NVIDIA.
- Отложенная отправка скриншота экрана в Telegram с именем ПК и локальным IP-адресом.
- Редактирование путей до программ стресс-тестов в `settings.config`. Если файл отсутствует - он создатся после первого запуска программы.
- Раздельное логирование в `logs\Aegis.log`, `logs\AegisGPU.log` и `logs\AegisDisplay.log`.

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
| `Display.Normalize` | Установить родное разрешение и максимальную герцовку, сбросить цветовые настройки NVIDIA. |

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

## NVIDIA Driver

Aegis может обнаружить видеокарту, определить текущую версию драйвера, найти актуальную версию на стороне NVIDIA, скачать установщик, распаковать его через встроенный 7-Zip и запустить установку.

Ключи `-s` и `-noreboot` используются всегда автоматически, указывать их вручную не нужно.

Примеры:

Обновит только видеодрайвер NVIDIA в тихом режиме.

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver
```

Обновит видеодрайвер и NVIDIA HD Audio Driver.

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver HDAudio.Driver
```

Обновит видеодрайвер и аудиодрайвер с чистой установкой.

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver HDAudio.Driver -clean
```

## NVIDIA Control Panel

Панель управления можно установить отдельно, без установки видеодрайвера:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" NVIDIA.Panel
```

## Нормализация разрешения, герцовки и цвета

Команда `Display.Normalize` выполняет для каждого активного монитора:

- устанавливает родное разрешение;
- выбирает максимально доступную герцовку для этого разрешения;
- сбрасывает цветовые настройки NVIDIA (яркость, контрастность, гамму, цифровую интенсивность и оттенок).

Пример:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Normalize
```

## Скриншот в Telegram

Команда `/TIME` задает задержку перед созданием скриншота и отправкой его в Telegram (токен жестко указан в коде).

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

Логирование работы программы находится по путям:

```text
logs\Aegis.log
logs\AegisGPU.log
logs\AegisDisplay.log
```

`Aegis.log` содержит общий журнал работы программы. 
`AegisGPU.log` содержит отдельный журнал установки NVIDIA-драйвера и панели управления NVIDIA.
`AegisDisplay.log` содержит отдельный журнал установки разрешения, герцовки, а также цветовых настроек в панели управления NVIDIA.

## Updater.exe

Updater.exe - отдельный компонент для обновления Aegis.

- Проверяет актуальную версию Aegis.exe на GitHub.
- Сравнивает версию на ПК с локальной версией.
- Загружает и устанавливает обновление при необходимости.
- Поддерживает тихий режим через `/s` или `/silent`.

Пример:

```bat
"C:\Gizmo Utilities\Aegis\Updater.exe" /silent
```

## Пример полного запуска ПО для тестирования в Furmark, AIDA (CPU, FPU, Memory) и Victoria (диск C). С уведомлением в телеграм через 30 минут.

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /furmark /aida -cpu -fpu -memory /victoria -C /time 30m
```

Команды выполняются поочередно, в том порядке, в котором они указаны в аргументах запуска.

## Расположение

Рекомендуемое, но не обязательное расположение программы:

```text
C:\Gizmo Utilities\Aegis
```

## Roadmap

- Добавить более подробную отчетность в Telegram (Furmark, AIDA, Victoria).
- Добавить поддержку OCCT.
