---
title: Где мой компьютер Mac?
description: Инструкции по настройке компьютера Mac для сборки проектов Xamarin.iOS с помощью Visual Studio
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f3e2988c9700549db24ad69277df9e412c99caed
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/21/2018
---
# <a name="wheres-my-mac"></a>Где мой компьютер Mac?

_Инструкции по настройке компьютера Mac для сборки проектов Xamarin.iOS с помощью Visual Studio_

Текущая **стабильная** версия Xamarin для Visual Studio взаимодействует с компьютером Mac не так, как предыдущие версии[^](#earlier-versions).

Для использования компьютера Mac в качестве агента Xamarin Mac Agent необходимо включить на нем **удаленный вход**.

1. Откройте программу *Spotlight* (`Cmd-Space`), выполните поиск по запросу **Удаленный вход** и выберите результат **Общий доступ**. Откроется панель **Общий доступ** в программе **Системные настройки**.

  ![](visual-studio-ssh-images/spotlight.png "Поиск удаленного входа в Spotlight")

2. В списке **Служба** в левой части окна установите флажок **Удаленный вход**, чтобы разрешить Xamarin для Visual Studio подключаться к компьютеру Mac.

  ![](visual-studio-ssh-images/sharing.png "Установка флажка "Удаленный вход" в списке "Служба"")

3. Для параметра **Удаленный вход** необходимо разрешить доступ **всем пользователям** либо включить свое имя пользователя Mac или группу в список разрешенных пользователей справа.

Компьютер Mac теперь должен обнаруживаться средой Visual Studio, если они находятся в одной сети.
Когда Visual Studio пытается подключиться к компьютеру Mac, вы получаете запрос на вход из Visual Studio с помощью ваших учетных данных пользователя Mac.

## <a name="where-can-i-find-more-information"></a>Где можно найти дополнительные сведения?

Существует ряд руководств, которые могут помочь настроить новый агент и понять принципы его работы. Они перечислены ниже:

- [Подключение к компьютеру Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Устранение неполадок](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University — Xamarin Mac Agent](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>Переход с предыдущих версий

Если вы работали с предыдущими версиями Xamarin для Visual Studio, то вам знакомо приложение **Xamarin.iOS Build Host**, которое требовалось запускать на компьютере Mac, а затем *связывать* с экземпляром Visual Studio с помощью ПИН-кода.

Это приложение больше не требуется в текущей версии Xamarin для Visual Studio.
