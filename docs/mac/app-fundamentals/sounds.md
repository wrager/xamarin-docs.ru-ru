---
title: Воспроизведение звука с AVAudioPlayer в Xamarin.Mac
description: В этом документе описывается воспроизведение звука с AVAudioPlayer в приложении Xamarin.Mac. В нем описывается AVAudioPlayer на общем уровне ссылки на другие документацию, более подробно рассматривается его более подробно.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 9e5b9ec43189999f8a0aee29eb50221b494e2133
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791859"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Воспроизведение звука с AVAudioPlayer в Xamarin.Mac

## <a name="about-the-avaudioplayer"></a>О AVAudioPlayer

`AVAudioPlayer` Класс используется для воспроизведения звуковых данных из памяти или файла. Apple рекомендует использовать этот класс для воспроизведения звука в приложении, если Вы поступаете сети потоковой передачи или требуют аудио небольшой задержки ввода-вывода.

Можно использовать `AVAudioPlayer` класс делать следующее:

- Воспроизведение звуков любой продолжительности с необязательным циклов.
- Воспроизведение звуков несколько одновременно с необязательным синхронизации.
- Управлять тома, скорость воспроизведения и стерео позиционирование для каждого звуки воспроизведения.
- Поддерживает функции, такие как Перемотка вперед или перемотка назад.
- Получите уровень воспроизведения программных продуктов.

`AVAudioPlayer` поддерживает звуков в любой формат аудио, iOS, tvOS и macOS например .aif, WAV или MP3.

## <a name="playing-sounds-in-macos"></a>Воспроизведение звуков в macOS

Поскольку macOS поддерживает те же классы звуковых элементов, поскольку iOS, см. в разделе iOS [игральной звука с AVAudioPlayer](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) документации полные сведения о воспроизведение аудио в приложении Xamarin.Mac.

## <a name="related-links"></a>Связанные ссылки

- [Справочник по AVAudioPlayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
