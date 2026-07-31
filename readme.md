<div align="center">
  <img
    width="300"
    height="300"
    alt="Aegis"
    src="https://github.com/user-attachments/assets/5a23d553-6e37-4f45-8540-b9685fa995c8"
  />

  <h1>
    <a href="https://github.com/DKardash">
      <img
        width="26"
        height="26"
        alt="DKardash"
        src="https://avatars.githubusercontent.com/u/177488736?v=4"
      />
    </a>
    <a href="https://github.com/DKardash">DKardash</a><a href="https://github.com/DKardash/Aegis">/Aegis</a>
  </h1>
</div>

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

## Основные команды

| Команда | Назначение |
| --- | --- |
| `/AIDA` | Запустить AIDA64 с выбранными стресс-тестами. |
| `/VICTORIA` | Запустить Victoria и протестировать выбранные тома. |
| `/FURMARK` | Запустить стресс-тест видеокарты в FurMark. |
| `/TIME` | Сделать и отправить скриншот в Telegram через заданное время. |
| `Display.Driver` | Обновить NVIDIA Display Driver в тихом режиме. |
| `HDAudio.Driver` | Дополнительно установить NVIDIA HD Audio Driver. |
| `-clean` | Выполнить чистую установку NVIDIA-драйвера. |
| `NVIDIA.Panel` | Установить только панель управления NVIDIA. |
| `Display.Normalize` | Нормализовать разрешение, герцовку и цветовые настройки NVIDIA. |

## Быстрый пример

Запуск FurMark, стресс-тестов CPU, FPU и памяти в AIDA64, тестирование диска `C:` в Victoria и отправка скриншота в Telegram через 30 минут:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /furmark /aida -cpu -fpu -memory /victoria -C /time 30m
```

> [!NOTE]
> Команды выполняются последовательно — в том порядке, в котором они указаны в аргументах запуска.

## Подробная документация

<details>
<summary><strong>Стресс-тесты и диагностика: AIDA64, Victoria и FurMark</strong></summary>

### AIDA64

Команда `/AIDA` поддерживает следующие параметры:

| Параметр | Назначение |
| --- | --- |
| `-CPU` | Стресс-тест CPU. |
| `-FPU` | Стресс-тест FPU. |
| `-CACHE` | Стресс-тест cache. |
| `-MEMORY` | Стресс-тест memory. |
| `-DISKS` | Стресс-тест disks. |
| `-GPU` | Стресс-тест GPU. |

Пример запуска всех доступных стресс-тестов:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /aida -cpu -fpu -cache -memory -disks -gpu
```

### Victoria

Команда `/VICTORIA` позволяет выбрать тома по букве диска:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /victoria -C -D -E
```

За один запуск можно указать до трёх томов. Aegis учитывает, что в Victoria сначала отображаются физические диски, а затем тома, поэтому позиция нужного тома рассчитывается с учётом количества физических дисков в системе.

### FurMark

Команда `/FURMARK` запускает FurMark для стресс-теста видеокарты:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /furmark
```

</details>

<details>
<summary><strong>Обновление драйвера NVIDIA</strong></summary>

Aegis обнаруживает установленную видеокарту, определяет текущую версию драйвера, находит актуальную версию на стороне NVIDIA, скачивает установщик, распаковывает его с помощью встроенного 7-Zip и запускает установку.

Ключи `-s` и `-noreboot` применяются автоматически — указывать их вручную не требуется.

Обновить только NVIDIA Display Driver:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver
```

Обновить NVIDIA Display Driver и HD Audio Driver:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver HDAudio.Driver
```

Выполнить чистую установку NVIDIA Display Driver и HD Audio Driver:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Driver HDAudio.Driver -clean
```

### NVIDIA Control Panel

Панель управления NVIDIA можно установить отдельно, без установки видеодрайвера:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" NVIDIA.Panel
```

</details>

<details>
<summary><strong>Нормализация параметров дисплея</strong></summary>

Команда `Display.Normalize` выполняет следующие действия для каждого активного монитора:

- устанавливает родное разрешение;
- выбирает максимально доступную герцовку для этого разрешения;
- сбрасывает цветовые настройки NVIDIA: яркость, контрастность, гамму, цифровую интенсивность и оттенок.

Пример:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" Display.Normalize
```

</details>

<details>
<summary><strong>Отправка скриншота в Telegram</strong></summary>

Команда `/TIME` задаёт задержку перед созданием и отправкой скриншота экрана в Telegram.

| Пример | Результат |
| --- | --- |
| `/time 30s` | Отправить скриншот через 30 секунд. |
| `/time 30m` | Отправить скриншот через 30 минут. |

Пример совместного запуска со стресс-тестами:

```bat
"C:\Gizmo Utilities\Aegis\Aegis.exe" /furmark /aida -cpu -fpu /time 30m
```

В Telegram отправляются скриншот и подпись:

```text
🖥 Имя ПК: Windows
🌐 Локальный IP: 192.168.0.100
```

> [!IMPORTANT]
> В текущей реализации Telegram-токен указывается непосредственно в коде программы.

</details>

<details>
<summary><strong>Настройка путей через settings.config</strong></summary>

Если рядом с `Aegis.exe` нет файла `settings.config`, программа автоматически создаст его при первом запуске со стандартными путями.

Формат файла:

```ini
[Applications]
AIDA=C:\Gizmo Utilities\AIDA64_portable\AIDA64ExtremePortable.exe
Furmark=C:\Gizmo Utilities\FurMark2\furmark.exe
Victoria=C:\Gizmo Utilities\Victoria537\Victoria537\Victoria.exe
```

Если файл существует, Aegis использует указанные в нём пути. Для отсутствующих значений применяются стандартные пути, заложенные в программе.

</details>

<details>
<summary><strong>Логи</strong></summary>

Журналы работы сохраняются в каталоге `logs`:

```text
logs\Aegis.log
logs\AegisGPU.log
logs\AegisDisplay.log
```

| Файл | Содержимое |
| --- | --- |
| `Aegis.log` | Общий журнал работы программы. |
| `AegisGPU.log` | Установка NVIDIA-драйвера и NVIDIA Control Panel. |
| `AegisDisplay.log` | Изменение разрешения, герцовки и цветовых настроек NVIDIA. |

</details>

<details>
<summary><strong>Обновление Aegis через Updater.exe</strong></summary>

`Updater.exe` — отдельный компонент для обновления Aegis. Он:

- проверяет актуальную версию `Aegis.exe` на GitHub;
- сравнивает её с установленной версией;
- при необходимости загружает и устанавливает обновление;
- поддерживает тихий режим через `/s` или `/silent`.

Пример:

```bat
"C:\Gizmo Utilities\Aegis\Updater.exe" /silent
```

</details>

## Расположение

Рекомендуемое, но не обязательное расположение программы:

```text
C:\Gizmo Utilities\Aegis
```

## Roadmap

- интеграция систем мониторинга температур CPU и GPU;
- более подробная отчётность в Telegram по результатам тестов FurMark, AIDA64 и Victoria (с максимальными температурами CPU и GPU во время тестов и генерацией подробного отчета по результатам тестов ПК);
- обновление Zapret от Flowseal через ключи (Zapret.Update & /Zapret, либо какой-то другой интуитивно понятный ключ);
- рассмотреть перенос Token и ChatID в конфигурационный файл `settings.config` с шифрованием внесенных данных;
- поддержка `OCCT`.
