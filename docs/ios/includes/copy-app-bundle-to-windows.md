
При создании приложений iOS в Visual Studio и агенте сборки Mac пакет APP не копируется обратно на компьютер Windows. В инструменты Xamarin для Visual Studio 7.4 добавлено новое свойство `CopyAppBundle`, позволяющее сборкам CI копировать пакеты APP обратно в Windows.

Чтобы использовать эту функцию, добавьте свойство `CopyAppBundle` в CSPROJ-файл в группе свойств, к которой следует применить эту функцию. Например, в приведенном ниже примере показано копирование пакета APP обратно на компьютер Windows для сборки **Отладка**, предназначенной для **iPhoneSimulator**:

    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
        <CopyAppBundle>true</CopyAppBundle>
    </PropertyGroup>

