# Atmosfear 3 (ver. 1.0) for IX-Ray Platform

## Overview

This is an official adaptation of the Atmosfear 3 weather mod for the IX-Ray platform. Atmosfear is a set of modifications for the game S.T.A.L.K.E.R.: Call of Pripyat, which changes and expands the standard weather system, making it more realistic and diverse. The visibility range in some weather cycles has been significantly increased (and reduced in others), so that on a clear day you can see literally the entire location

## Remarks

> [!WARNING]
>
> Supported only on __IX-Ray Platform__!
> Not compatible with other weather mods!

## Requirements

Installation:

1. Install Shadow of Chernobyl, Clear Sky, or Call of Pripyat
2. Install a matching IX-Ray build with platform-aware addons and CoP-format weather support
3. Extract the archive into the game folder, preserving all four modules in `ixr_addons/ixray-atmosfear-3/`
4. Start the game and play

Link to the current __IX-Ray__ [release](https://github.com/ixray-team/ixray-1.6-stcop/releases/latest)

## Modules and platform behavior

| Directory | Platform | Contents |
| --- | --- | --- |
| `af3-common` | `soc`, `cs`, `cop` | Unified weather cycles, textures, sounds, animations, graph probabilities, default parameters and renderer settings; portable SoC/CS weather implementation and weather options |
| `af3-soc` | `soc` | Outdoor weather assignments, native sound/particle adapters, native save format, `USE_DIFFUSE_SKY_COLOR = 0` |
| `af3-cs` | `cs` | Outdoor weather assignments, introduction conditions and native scripted surge/save handling |
| `af3-cop` | `cop` | Original AF3 managers, options, tasks, UI, level ambients and particle library |

The common folder and manifest `name` are both `af3-common`; all platform modules use `depends: af3-common`. Install the common module together with the matching platform module. Disable CGIM2, SWTC and other weather addons.

SoC and CS use AF3 weighted hourly cycles, alternating good/bad periods, transition weather, night brightness and all eight moon phases. The AtmosFear 3 tab in IX-Ray options exposes independent primary/secondary weather presets and period lengths for every outdoor location (12 in SoC, 11 in CS), plus shared night brightness and moon settings. IX-Ray saves the selected values and restores them at startup; `af3-common/configs/atmosfear_parameters.ltx` supplies defaults. Changes to the active weather profile apply on the next manager update; changes during a scripted surge are deferred until the scenario releases weather control. Zaton, Jupiter and Pripyat profiles supply initial defaults only; changes to a location do not alter settings for other locations. CoP retains its individual Zaton, Jupiter and Pripyat controls. Yantar (SoC/CS), Radar (SoC) and Pripyat (SoC/CoP) default to the foggy/rainy preset for both periods; saved menu choices take priority. State persists in actor pstor; underground weather and native packet layouts are preserved.

CS scripted surges keep control for the entire `xr_surge_hide` lifecycle, including gaps between WFX and phases 4/5 after `start_surge` is cleared. The ChNPP finale is also protected. A saved WFX is resumed once with its remaining time. The addon preserves native CS surge effect files, task logic and the introductory weather conditions. SoC scripted WFX, including the ChNPP-1 surge, have priority over dynamic weather.

Random AF3 blowouts, psi storms, fallout, their tasks and event options remain exclusive to CoP. Weather options are available on all three platforms. SoC and CS keep their native event systems and particle libraries; the CoP `particles.xr` is not mounted there.

## Verification

Run the portable manager regression suite from the repository root with a Lua 5.1/LuaJIT shared library:

```powershell
python tests/run_weather_tests.py --lua-library D:\ixray-team\soc\bin\lua51.dll
```

The suite covers weather periods, transitions, saving/loading, WFX restoration, CS surge phases, finale ownership and weather options on all three platforms with mocked game APIs. It does not replace an in-game check of the tutorial and finale.

## Contributors

### IX-Ray Team

- Drombeys

### AtmosFear Team

- CrommCruac

## License

Сontents of this repository are licensed under terms of the __CC BY-NC-SA 4.0__ license unless otherwise specified. See [this](./LICENSE.txt) file for details

## Обзор

Это официальная адаптация погодного мода Atmosfear 3 под платформу IX-Ray. Atmosfear - это набор модификаций для игры S.T.A.L.K.E.R. - Зов Припяти, который изменяет и расширяет стандартную систему погоды, делая ее более реалистичной и разнообразной. Дальность видимости на некоторых погодных циклах была значительно увеличена (и снижена на других), так что в ясный день можно разглядеть буквально всю локацию

## Примечаниe

> [!WARNING]
>
> Поддерживается только на __IX-Ray Platform__!
> Не совместимо с другими погодными модами!

## Рекомендации

Установка:

1. Установить «Тень Чернобыля», «Чистое небо» или «Зов Припяти»
2. Установить сборку IX-Ray для выбранной игры с поддержкой платформ в аддонах и погоды формата ЗП
3. Распаковать архив в папку с игрой, сохранив все четыре модуля в `ixr_addons/ixray-atmosfear-3/`
4. Запустите игру и играйте

Ссылка на текущую версию __IX-Ray__ [релиз](https://github.com/ixray-team/ixray-1.6-stcop/releases/latest)

## Модули и поведение платформ

| Папка | Платформа | Содержимое |
| --- | --- | --- |
| `af3-common` | `soc`, `cs`, `cop` | Единые циклы, текстуры, звуки, анимации, графы, параметры и настройки рендера; общая реализация погоды и меню погодных опций ТЧ/ЧН |
| `af3-soc` | `soc` | Привязки погоды, родные звуки и частицы, штатный формат сохранения, `USE_DIFFUSE_SKY_COLOR = 0` |
| `af3-cs` | `cs` | Привязки погоды, условия вступления, совместимость со скриптовыми выбросами и сохранениями ЧН |
| `af3-cop` | `cop` | Исходные менеджеры AF3, опции, задания, интерфейс, эмбиенты уровней и библиотека частиц |

Имя папки и поле `name` общего модуля совпадают: `af3-common`. Все платформенные модули используют `depends: af3-common`. Общий модуль обязателен. Перед запуском отключите CGIM2, SWTC и другие погодные аддоны.

В ТЧ и ЧН работают взвешенный выбор циклов каждый игровой час, чередование хороших/плохих периодов, переходная погода, яркость ночей и восемь фаз луны. Вкладка AtmosFear 3 в опциях IX-Ray позволяет независимо выбрать первичную/вторичную погоду и длительность периодов для каждой наземной локации (12 в ТЧ, 11 в ЧН). Яркость ночей и фазы луны задаются для всей игры. Система опций сохраняет значения и восстанавливает их при запуске; `af3-common/configs/atmosfear_parameters.ltx` задаёт значения по умолчанию. Изменения активного погодного профиля применяются при следующем обновлении менеджера; во время скриптового выброса применение откладывается до освобождения управления погодой. Профили Затона, Юпитера и Припяти служат только начальными значениями: настройки одной локации не меняют другие. В ЗП сохранены отдельные настройки Затона, Юпитера и Припяти. Янтарь (ТЧ/ЧН), Радар (ТЧ) и Припять (ТЧ/ЗП) по умолчанию используют туманно-дождливый пресет для обоих периодов; сохранённый выбор в меню имеет приоритет. Состояние сохраняется в штатном хранилище актёра; погода подземелий и форматы блоков сохранения остаются родными.

В ЧН погода уступает управление скриптовому выбросу на всём протяжении `xr_surge_hide`, включая промежутки между WFX и завершающие фазы 4/5, когда `start_surge` уже сброшен. Финал на ЧАЭС также защищён. Сохранённый WFX восстанавливается однократно с оставшимся временем. Штатные файлы эффектов выброса, задания и условия погоды вступления ЧН сохраняются. В ТЧ скриптовые WFX, включая выброс на ЧАЭС-1, имеют приоритет над динамической погодой.

Случайные выбросы AF3, пси-штормы, осадки, их задания и событийные опции остаются в модуле ЗП. Настройки погоды доступны для всей трилогии. ТЧ и ЧН используют родные событийные системы и библиотеки частиц: `particles.xr` из ЗП на них не подключается.

Регрессионные проверки запускаются через `tests/run_weather_tests.py` с параметром `--lua-library` (пример выше). Проверки используют подменённые API игры и не заменяют прохождение вступления и финала в самой игре.

## Контрибьюторы

### Команда IX-Ray

- Drombeys

### Команда AtmosFear

- CrommCruac

## Лицензия

Содержимое этого репозитория лицензировано в соответствии с условиями лицензии __CC BY-NC-SA 4.0__, если не указано иное. Подробности см. в файле [здесь](./LICENSE.txt)
