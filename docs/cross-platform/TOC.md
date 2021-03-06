# [Кроссплатформенные приложения](index.yml)
## [Начало работы](get-started/index.md)
### [Requirements](get-started/requirements.md)
### [Установка](get-started/installation/index.md)
#### [Установка Xamarin в Windows](get-started/installation/windows.md)
##### [Установка предварительных версий Xamarin в Windows](get-started/installation/windows-preview.md)
#### [Установка Visual Studio для Mac](/visualstudio/mac/installation/)
##### [Установка предварительных версий Xamarin в Mac](/visualstudio/mac/update/)
#### [Инструкции по настройке брандмауэра Xamarin](get-started/installation/firewall.md)
#### [Удаление Xamarin](get-started/installation/uninstalling-xamarin.md)
### [Введение в разработку мобильных приложений](get-started/introduction-to-mobile-development.md)
### [Введение в жизненный цикл программного обеспечения (SDLC) мобильных устройств](get-started/introduction-to-mobile-sdlc.md)

### [Создание кросс платформенных приложений](app-fundamentals/building-cross-platform-applications/index.md)
#### [Обзор набора средств Visual Studio для Unity](app-fundamentals/building-cross-platform-applications/overview.md)
#### [Часть 1. Основные сведения о мобильной платформе Xamarin](app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)
#### [Часть 2. Архитектура](app-fundamentals/building-cross-platform-applications/architecture.md)
#### [Часть 3. Настройка кроссплатформенного решения Xamarin](app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)
#### [Часть 4. Работа с несколькими платформами](app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)
#### [Часть 5. Практические стратегии совместного использования кода](app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)
#### [Часть 6. Тестирование и утверждение магазина приложений](app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)
#### [Практический пример: Tasky](app-fundamentals/building-cross-platform-applications/case-study-tasky.md)

## [Общий доступ к коду](app-fundamentals/index.md)
### [Обзор набора средств Visual Studio для Unity](app-fundamentals/code-sharing.md)
### [.NET Standard](app-fundamentals/net-standard.md)
### [Общие проекты](app-fundamentals/shared-projects.md)
### [Переносимые библиотеки классов](app-fundamentals/pcl.md)
### [Пакеты NuGet (автоматизированные)](app-fundamentals/nuget-multiplatform-libraries/index.md)
#### [Имеющиеся проекты библиотеки](app-fundamentals/nuget-multiplatform-libraries/existing-library.md)
#### [Новая многоплатформенная библиотека](app-fundamentals/nuget-multiplatform-libraries/single-codebase.md)
#### [Новые библиотеки для конкретных платформ](app-fundamentals/nuget-multiplatform-libraries/platform-specific.md)
#### [Руководство по метаданным](app-fundamentals/nuget-multiplatform-libraries/metadata.md)
### [Пакеты NuGet (руководство)](app-fundamentals/nuget-manual.md)

## [Поддержка языков](platform/index.md)

### C#
#### [Общие сведения о поддержке асинхронного выполнения](platform/async.md)
#### [6 возможностей языка C#](platform/csharp-six.md)
### [F#](platform/fsharp/index.md)
#### [Начало работы](platform/fsharp/overview.md)
#### [Примеры F#](platform/fsharp/samples.md)
### [Visual Basic.NET](platform/visual-basic/index.md)
#### [Xamarin iOS и Android](platform/visual-basic/native-apps.md)
#### [Xamarin.Forms](platform/visual-basic/xamarin-forms.md)
### [Шаблоны HTML Razor](platform/razor-html-templates/index.md)

## [Производительность и безопасность](deploy-test/performance.md)
### [Производительность](deploy-test/memory-perf-best-practices.md)
#### [Производительность для Android](~/android/deploy-test/performance.md?context=xamarin/cross-platform)
#### [Производительность для iOS](~/ios/deploy-test/performance.md?context=xamarin/cross-platform)
#### [Производительность для Mac](~/mac/deploy-test/performance.md?context=xamarin/cross-platform)
### [Протокол TLS](app-fundamentals/transport-layer-security.md)
#### [Android](~/android/app-fundamentals/http-stack.md?context=xamarin/cross-platform)
#### [iOS и Mac](~/cross-platform/macios/http-stack.md?context=xamarin/cross-platform)
## [Развертывание и отладка](deploy-test/index.md)
### [Пользовательская конфигурация компоновщика](deploy-test/linker.md)
### [Установка NUnit 2.6.4 с помощью NuGet](deploy-test/installing-nunit-using-nuget.md)
### [Отладка нескольких процессов](deploy-test/multi-process-debugging.md)

## [Разработчики классических приложений](desktop/index.md)
### [Сравнение жизненного цикла приложений](desktop/lifecycle.md)
### [Сравнение элементов управления пользовательского интерфейса](desktop/controls/index.md)
#### [Сравнение WPF и Xamarin.Forms](desktop/controls/wpf.md)
### [Руководство по переносу](desktop/porting.md)
### [Примеры](desktop/samples.md)

## [Устранение неполадок](troubleshooting/index.md)
### [Часто задаваемые вопросы](troubleshooting/questions/index.md)
#### [Как можно просмотреть, какие библиотеки поддерживаются в PCL](troubleshooting/questions/pcl-support-libraries.md)
#### [API отражения PCL](troubleshooting/questions/pcl-reflection.md)
#### [Практический пример PCL. Как устранять проблемы, связанные с System.Diagnostics.Tracing для пакета NuGet потока данных библиотеки параллельных задач Майкрософт](troubleshooting/questions/pcl-case-study.md)
#### [Как обновить NuGet](troubleshooting/questions/nuget-update.md)
#### [Как перейти на использование более ранней версии пакета NuGet](troubleshooting/questions/nuget-package-downgrade.md)
#### [Ошибка отсутствующих пакетов после обновления пакетов NuGet](troubleshooting/questions/nuget-packages-missing.md)
#### [Объединение компонентов Сервисов Google Play и NuGet](troubleshooting/questions/gps-components-nuget.md)
#### [Где на компьютере хранятся компоненты](troubleshooting/questions/component-storage.md)
#### [Где я могу найти информацию о версии и журналы](troubleshooting/questions/version-logs.md)
#### [Когда и как следует указывать отчет об ошибке](troubleshooting/questions/howto-file-bug.md)
#### [Почему Xamarin не поддерживает Jenkins](troubleshooting/questions/xamarin-jenkins.md)
#### [Какие параметры проекта нужны отладчику](troubleshooting/questions/debugger-settings.md)

### Вопросы по Visual Studio
#### [Отключены флажки развертывания в Configuration Manager](troubleshooting/questions/deploy-checkboxes.md)
#### [Отсутствуют расширения Visual Studio после установки](troubleshooting/questions/missing-vs-extensions.md)
#### [Как полностью удалить Xamarin для Visual Studio?](troubleshooting/questions/uninstall-xamarin-vs.md)
#### [Как собрать текущие стеки вызовов процесса Visual Studio?](troubleshooting/questions/vs-callstack.md)
#### [Почему Visual Studio не включает проект библиотеки в сборку, которая на него ссылается?](troubleshooting/questions/vs-config-manager.md)

### [Обновление ссылок на компоненты в NuGet](troubleshooting/component-nuget.md)
### [Варианты поддержки](troubleshooting/support-options.md)
## [Примеры](samples/index.yml)
