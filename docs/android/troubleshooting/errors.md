---
title: "Матрица Xamarin.Android ошибок"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c3aab44fcb0522b762aaa101de0aeba08016b641
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-errors-matrix"></a>Матрица Xamarin.Android ошибок

## <a name="errors-reference"></a>Справочник ошибок

Этот документ содержит некоторые сведения о различных кодах ошибок из Xamarin.

<table>
    <thead>
        <tr>
            <td>Категория</td>
            <td>Описание:</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> Ошибки</td></tr>
        <tr><td>XA1xxx</td><td>Копирование файла или символических ссылок (проект связанные) ошибок</td></tr>
        <tr><td>XA2xxx</td><td>Ошибки компоновщика</td></tr>
        <tr><td>XA3xxx</td><td>Ошибки AOT</td></tr>
        <tr><td>XA4xxx</td><td>Ошибки создания кода</td></tr>
        <tr><td>XA5xxx</td><td>GCC и инструментов ошибки</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> Внутренние ошибки средства</td></tr>
        <tr><td>XA7xxx</td><td>Зарезервированное</td></tr>
        <tr><td>XA8xxx</td><td>Зарезервированное</td></tr>
        <tr><td>XA9xxx</td><td>Ошибок лицензирования</td></tr>
    </tbody>
</table>

## <a name="error-codes"></a>Коды ошибок

### <a name="xa0xxx-errors"></a>Ошибки XA0xxx

<table>
    <thead>
        <tr><td>Код ошибки</td><td>Описание:</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>Непредвиденная ошибка - заполните отчет об ошибках в <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>"-devname предоставлен без вмешательства конкретных устройств</td></tr>
        <tr><td>XA0002</td><td>Не удалось выполнить синтаксический анализ переменной среды «{0}».</td></tr>
        <tr><td>XA0003</td><td>Имя приложения «{0} .exe» конфликтует с именем сборки (DLL) пакета SDK или продукта.</td></tr>
        <tr><td>XA0004</td><td>Новая логика refcounting требует sgen слишком включения.</td></tr>
        <tr><td>XA0005</td><td>Выходной каталог «{0}» не существует.</td></tr>
        <tr><td>XA0006</td><td>Ни одна платформа devel на «{0}», использовать--платформы = PLAT, чтобы указать пакет SDK</td></tr>
        <tr><td>XA0007</td><td>Корневой сборки «{0}» не существует.</td></tr>
        <tr><td>XA0008</td><td>Можно указать только один корневой сборки</td></tr>
        <tr><td>XA0009</td><td>Произошла ошибка при загрузке сборки: {0}</td></tr>
        <tr><td>XA0010</td><td>Не удалось выполнить синтаксический анализ аргументов командной строки: {0}</td></tr>
        <tr><td>XA0011</td><td>{0} был создан в более поздней выполнения ({1}) не поддерживает MonoTouch</td></tr>
        <tr><td>XA0012</td><td>Неполные данные, введенные на завершения `{0}`.</td></tr>
        <tr><td>XA0013</td><td>Поддержка профилей требует sgen слишком включения</td></tr>
        <tr><td>XA0014</td><td>операций ввода-вывода {0} не поддерживает создание приложений, предназначенных для ARMv6</td></tr>
        <tr><td>XA0020</td><td>Не удалось определить путь mandroid.</td></tr>
        <tr><td>XA0100</td><td>EmbeddedNativeLibrary «{0}» является недопустимым в проект приложения Android. Вместо этого используйте AndroidNativeLibrary.</td></tr>
    </tbody>
</table>

### <a name="xa1xxx-errors"></a>XA1xxx Errors

<table>
    <thead>
        <tr><td>Код ошибки</td><td>Описание:</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>Не удалось найти приложение в указанном каталоге</td></tr>
        <tr><td>XA1002</td><td>Не удалось создать символьных ссылок, были скопированы файлы</td></tr>
        <tr><td>XA1003</td><td>Не удалось завершить приложения «{0}». Может потребоваться вручную завершить приложение.</td></tr>
        <tr><td>XA1004</td><td>Не удалось получить список установленных приложений.</td></tr>
        <tr><td>XA1005</td><td>Не удалось завершить приложение «{0}» на устройстве «{1}»: {2}. Может потребоваться вручную завершить приложение.</td></tr>
        <tr><td>XA1006</td><td>Не удалось установить приложение «{0}» на устройстве «{1}»: {2}.</td></tr>
        <tr><td>XA1007</td><td>Не удается запустить приложение «{0}» на устройстве «{1}»: {2}. Приложение по-прежнему можно запустить вручную, нажав на нем.</td></tr>
        <tr><td>XA1008</td><td>Не удается запустить имитатор: {0}</td></tr>
        <tr><td>XA1009</td><td>Не удалось скопировать сборку «{0}» к «{1}»: {2}</td></tr>
        <tr><td>XA1010</td><td>Не удалось загрузить сборку «{0}»: {1}</td></tr>
        <tr><td>XA1011</td><td>Не удалось добавить отсутствующий файл ресурсов: «{0}»</td></tr>
        <tr><td>XA1101</td><td>Не удалось запустить приложение</td></tr>
        <tr><td>XA1102</td><td>Не удалось присоединить к приложению (kill его): {0}</td></tr>
        <tr><td>XA1103</td><td>Не удалось отсоединить</td></tr>
        <tr><td>XA1104</td><td>Сбой при отправке пакета: {0}</td></tr>
        <tr><td>XA1105</td><td>Неожиданный отклик типа</td></tr>
        <tr><td>XA1106</td><td>Не удалось получить список приложений на устройстве: Истекло время ожидания запроса.</td></tr>
        <tr><td>XA1107</td><td>Не удается запустить приложение</td></tr>
        <tr><td>XA1201</td><td>Не удалось загрузить имитатор: {0}</td></tr>
        <tr><td>XA1301</td><td>Собственная библиотека `{0}` ({1}) было пропущено, так как он не соответствует текущей сборки architecture(s) ({2})</td></tr>
    </tbody>
</table>

### <a name="xa2xxx-errors"></a>XA2xxx Errors

<table>
    <thead>
        <tr><td>Код ошибки</td><td>Описание:</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>Не удалось выполнить привязку сборок</td></tr>
        <tr><td>XA2002</td><td>Не удается разрешить ссылку: {0}</td></tr>
        <tr><td>XA2003</td><td>Параметр «{0}» будет игнорироваться, так как компоновка отключена</td></tr>
        <tr><td>XA2004</td><td>Не удалось найти файл определения дополнительных компоновщика «{0}».</td></tr>
        <tr><td>XA2005</td><td>Не удалось выполнить синтаксический анализ определения из «{0}».</td></tr>
        <tr><td>XA2006</td><td>Не удалось разрешить ссылку на элемент метаданных (определенное в «{1}») «{0}» из «{2}».</td></tr>
    </tbody>
</table>

### <a name="xa3xxx-errors"></a>XA3xxx Errors

Это AOT ошибок.

<table>
    <thead>
        <tr><td>Код ошибки</td><td>Описание:</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>Удалось AOT сборки «{0}»</td></tr>
        <tr><td>XA3002</td><td>Ограничение AOT: метод «{0}» должен быть статическим, так как он объявлен с [MonoPInvokeCallback].</td></tr>
        <tr><td>XA3003</td><td>Конфликтующие--параметры отладки и--llvm. Отладка программной отключена.</td></tr>
    </tbody>
</table>

### <a name="xa4xxx-errors"></a>XA4xxx Errors

Это ошибки создания кода.

<table>
    <thead>
        <tr><td>Код ошибки</td><td>Описание:</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>Не удалось expansed в главном шаблоне `{0}`.</td></tr>
        <tr><td>XA4101</td><td>Регистратор не удалось создать подпись для типа `{0}`.</td></tr>
        <tr><td>XA4102</td><td>Регистратор найден недопустимый тип `{0}` в сигнатуре метода `{2}`. Взамен рекомендуется использовать `{1}`.</td></tr>
        <tr><td>XA4103</td><td>Регистратор найден недопустимый тип `{0}` в сигнатуре метода `{2}`: тип реализует INativeObject, но не имеет конструктора, который принимает два (IntPtr bool) аргументов</td></tr>
        <tr><td>XA4104</td><td>Регистратор не может маршалировать возвращаемое значение для типа `{0}` в сигнатуре метода `{1}`.</td></tr>
        <tr><td>XA4105</td><td>Регистратор не может маршалировать параметр типа `{0}` в сигнатуре метода `{1}`.</td></tr>
        <tr><td>XA4106</td><td>Регистратор не может маршалировать возвращаемое значение для структуры `{0}` в сигнатуре метода `{1}`.</td></tr>
        <tr><td>XA4107</td><td>Регистратор не может маршалировать параметр типа `{0}` в сигнатуре метода `{1}`.</td></tr>
        <tr><td>XA4108</td><td>Регистратор не удается получить тип ObjectiveC для управляемого типа `{0}`.</td></tr>
        <tr><td>XA4109</td><td>Не удалось скомпилировать код созданный регистратора. Отправьте отчет об ошибках в <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>Регистратор не может маршалировать выходной параметр типа `{0}` в сигнатуре метода `{1}`.</td></tr>
        <tr><td>XA4111</td><td>Регистратор не удалось создать подпись для типа `{0}' in method `{1} ".</td></tr>
        <tr><td>XA4200</td><td>ACW можно создавать только для типов «claas».</td></tr>
        <tr><td>XA4201</td><td>Не удалось определить имя JNI для типа {0}.</td></tr>
        <tr><td>XA4203</td><td>Указанное имя типа должно быть полным.</td></tr>
        <tr><td>XA4204</td><td>Не удалось разрешить тип интерфейса «{0}». Отсутствует ссылка на сборку?</td></tr>
        <tr><td>XA4205</td><td>[ExportField] можно использовать только в методах с 0 параметров.</td></tr>
        <tr><td>XA4206</td><td>[Экспорта] не может использоваться для универсального типа.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] не может использоваться для универсального типа.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] не может использоваться на метод, возвращающий значение void.</td></tr>
        <tr><td>XA4209</td><td>Не удалось создать JavaTypeInfo для класса: {0} из-за {1}</td></tr>
        <tr><td>XA4210</td><td>Необходимо добавить ссылку на Mono.Android.Export.dll при использовании ExportAttribute или ExportFieldAttribute.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion «{0}» меньше, чем $(TargetFrameworkVersion) «{1}». С помощью API-\ {1\\} для ACW компиляции.</td></tr>
    </tbody>
</table>

### <a name="xa5xxx-errors"></a>Ошибки XA5xxx

<table>
    <thead>
        <tr><td>Код ошибки</td><td>Описание:</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>Отсутствует «{0}» компилятора. Выполните установку Android NDK.</td></tr>
        <tr><td>XA5102</td><td>Не удалось выполнить преобразование из сборки в машинный код. Отправьте отчет об ошибках в <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>Не удалось скомпилировать файл «{0}». Отправьте отчет об ошибках в <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>Не удалось выполнить связывание собственный. Проверьте предоставленный gcc флаги пользователя: {0}</td></tr>
        <tr><td>XA5202</td><td>Не удалось выполнить связывание собственный. Просмотрите журнал сборки.</td></tr>
        <tr><td>XA5303</td><td>Машинный код связывания предупреждение: {0}</td></tr>
        <tr><td>XA5300</td><td>Пакет SDK для Android не найдена, или установлена не полностью.</td></tr>
        <tr><td>XA5301</td><td>Отсутствует средство «удалить». Установите компонент «Средства командной строки» Xcode</td></tr>
        <tr><td>XA5302</td><td>Отсутствует средство «dsymutil». Установите компонент «Средства командной строки» Xcode</td></tr>
        <tr><td>XA5203</td><td>Не удалось создать символы отладки (dSYM directory). Просмотрите журнал сборки.</td></tr>
        <tr><td>XA5204</td><td>Не удалось удалить конечный двоичный файл. Просмотрите журнал сборки.</td></tr>
        <tr><td>XA5205</td><td>Отсутствует средство «aapt». Установите пакет средств построения пакета SDK для Android.</td></tr>
        <tr><td>XA5206</td><td>{0}. Android ресурсов {1} каталог не существует.</td></tr>
        <tr><td>XA5207</td><td>{0}. {1} файл библиотеки Java не существует.</td></tr>
        <tr><td>XA5208</td><td>Не удалось загрузить. Загрузить {0} и поместить его в каталог {1}.</td></tr>
        <tr><td>XA5209</td><td>Сбой распаковки. {0} Загрузите и извлеките файлы в каталоге {1}.</td></tr>
        <tr><td>XA5210</td><td>{0}. Собственная библиотека {1} файл не существует.</td></tr>
    </tbody>
</table>

### <a name="xa6xxx-errors"></a>Ошибки XA6xxx

<table>
    <thead>
        <tr><td>Код ошибки</td><td>Описание:</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>Запущенный Cecil не поддерживает удаление сборки</td></tr>
        <tr><td>XA6002</td><td>Не удалось удалить сборку `{0}`.</td></tr>
        <tr><td>XA6003</td><td>Сообщение UnauthorizedAccessException]</td></tr>
    </tbody>
</table>

### <a name="xa9xxx-errors"></a>Ошибки XA9xxx

Это коды ошибок, ошибки лицензирования и активации.

 <a name="xa9000" />


#### <a name="xa9000"></a>XA9000

 **Причина:** истек срок действия лицензии

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
        </tr>
    </tbody>
</table>

 <a name="XA9001" />


#### <a name="xa9001"></a>XA9001

 **Причина:** истек срок действия пробной версии

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
            <td>ПРЕДУПРЕЖДЕНИЕ</td>
        </tr>
    </tbody>
</table>

 <a name="XA9002" />


#### <a name="xa9002"></a>XA9002

 **Cause:** AndroidJavaSource

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 **Причина:** AndroidJavaLibrary

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 **Если сборка привязки .jar внедренных, это перехватывается во время упаковки не время построения.**

 **Причина:** AndroidNativeLibrary

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 <a name="XA9003" />


#### <a name="xa9003"></a>XA9003

 **Причина:** System.Runtime.Serialization

 **При выполнении проверка:** пакета

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 **Причина:** System.ServiceModel.Web

 **При выполнении проверка:** пакета

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 **Причина:** Mono.Data.Tds

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 **Это упоминается в System.Data.dll, что разрешено**

 **Причина:** Mono.Android.Export

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 <a name="XA9004" />


#### <a name="xa9004"></a>XA9004

 **Причина:** --профилирования

 **При выполнении проверка:** пакета

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 <a name="XA9005" />


#### <a name="xa9005"></a>XA9005

 **Причина:** ограничение размера (32 КБ)

 **При выполнении проверка:** пакета

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>

 <a name="XA9006" />


#### <a name="xa9006"></a>XA9006

 **Причина:** имен System.Data.SqlClient

 **При выполнении проверка:** пакета

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 <a name="XA9008" />


#### <a name="xa9008"></a>XA9008

 **Причина:** построение из командной строки

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОК</td>
            <td>ОК</td>
            <td>ОК</td>
        </tr>
    </tbody>
</table>

 <a name="XA9009" />


#### <a name="xa9009"></a>XA9009

 **Причина:** отсутствует серийный номер

 **При выполнении проверка:** не поддающегося

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
        </tr>
    </tbody>
</table>

 <a name="XA9010" />


#### <a name="xa9010"></a>XA9010

 **Причина:** недопустимый ProductId

 **При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
        </tr>
    </tbody>
</table>

Эквивалентно XA9018

 <a name="XA9011" />


#### <a name="xa9011"></a>XA9011

 **Причина:** не удалось обновить файл лицензии (в новый формат файлов)

 **При выполнении проверка:** активации

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
        </tr>
    </tbody>
</table>

 <a name="XA9012" />


#### <a name="xa9012"></a>XA9012

 **Причина:** Интернет отсутствует

 **При выполнении проверка:** активации

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
        </tr>
    </tbody>
</table>

 <a name="XA9013" />


#### <a name="xa9013"></a>XA9013

 **Причина:** Неизвестная ошибка

 **При выполнении проверка:** активации

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
        </tr>
    </tbody>
</table>

 <a name="XA9014" />


#### <a name="xa9014"></a>XA9014

 **Причина:** недопустимый Activation кода

 **При выполнении проверка:** активации

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
        </tr>
    </tbody>
</table>

 <a name="XA9017" />


#### <a name="xa9017"></a>XA9017

 **Причина:** сервер активации не возвращает действительной лицензии

 **При выполнении проверка:** активации

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
            <td>ОШИБКА</td>
        </tr>
    </tbody>
</table>

<a name="XA9018" />

#### <a name="xa9018"></a>XA9018

**Причина:** недействительную лицензию

**При выполнении проверка:** построения

<table>
    <thead>
        <tr>
            <th>Начального уровня</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Предприятие</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>ОШИБКА</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
