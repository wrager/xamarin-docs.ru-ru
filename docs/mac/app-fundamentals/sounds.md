---
title: "Воспроизведение звука с AVAudioPlayer"
description: "В этой статье показано, как использовать вспомогательный класс для управления воспроизведение звука с помощью AVAudioPlayer."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: b3eb3f16f358becb22029cee295ef6064caad85a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Воспроизведение звука с AVAudioPlayer

_В этой статье показано, как использовать вспомогательный класс для управления воспроизведение звука с помощью AVAudioPlayer._

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
