---
title: PhotoKit
description: Комплект средств для фотографий позволяет приложениям запрашивать библиотека изображений системы и создать пользовательский Интерфейс для просмотра и изменения его содержимого.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: c721064f62f8e2255de2b4ea2d0438e3ed630d39
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="photokit"></a>PhotoKit

_Комплект средств для фотографий позволяет приложениям запрашивать библиотека изображений системы и создать пользовательский Интерфейс для просмотра и изменения его содержимого._

Набор фото — новую платформу, которая позволяет приложениям запрашивать библиотека изображений системы и создавать настраиваемые пользовательские интерфейсы для просмотра и изменения его содержимого. Он включает несколько классов, представляющих изображения и видео активы, а также коллекции средств, таких как диски и папки.

## <a name="model-objects"></a>Объекты модели
Комплект средств для фотографий представляет этих средств, в то, что он вызывает объекты модели. Модель объектов, представляющих фотографии и видео, сами имеют тип `PHAsset`. Объект `PHAsset` содержит метаданные, такие как тип актива мультимедиа и дата его создания.
Аналогичным образом `PHAssetCollection` и `PHCollectionList` классы содержат метаданные о коллекциях активов и списки коллекции соответственно. Коллекции активов — группы ресурсов, таких как фотографии и видео для заданного года. Аналогичным образом списки коллекции представляют собой группы активов коллекций, такие как фотографии и видео, сгруппированных по годам.

## <a name="querying-model-data"></a>Запрос данных модели
Комплект средств для фотографий упрощает процесс для запроса модели данных через различные методы выборки. Например, чтобы получить все образы, следует вызвать `PFAsset.Fetch`, передав `PHAssetMediaType.Image` тип носителя.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

`PHFetchResult` Экземпляр будет содержать все `PFAsset` экземпляров, представляющих изображения. Для получения изображений, сами используйте `PHImageManager` (или версия кэширования, `PHCachingImageManager`) для создания запроса для изображения, вызвав `RequestImageForAsset`. Например, следующий код извлекает изображение для каждого средства в `PHFetchResult` для отображения в ячейке представления коллекции:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

Это приводит к сетке изображений, как показано ниже:

![](photokit-images/image4.png "Отображение сетки образов выполняющегося приложения")
 
## <a name="saving-changes-to-the-photo-library"></a>Сохранение изменений в библиотеке фото

Это способ обработки запросов и чтение данных. Можно записывать эти изменения обратно в библиотеку. Так как несколько заинтересованных приложения, могут взаимодействовать с библиотекой фото системы, можно зарегистрировать наблюдателя для получения уведомлений об изменениях, с помощью PhotoLibraryObserver. Затем когда поступают изменения приложения можно обновить соответствующим образом. Например Вот простой реализации для перезагрузки выше представления коллекции.

    class PhotoLibraryObserver : PHPhotoLibraryChangeObserver
    {
        readonly PhotosViewController controller;
        public PhotoLibraryObserver (PhotosViewController controller)
        
        {
            this.controller = controller;
        }
    
        public override void PhotoLibraryDidChange (PHChange changeInstance)
        {
            DispatchQueue.MainQueue.DispatchAsync (() => {
            var changes = changeInstance.GetFetchResultChangeDetails (controller.fetchResults);
            controller.fetchResults = changes.FetchResultAfterChanges;
            controller.CollectionView.ReloadData ();
            });
        }
    }
    
Чтобы фактически записать изменения из приложения, создайте запрос на изменение. Каждый из классов модели содержит класс связанные с ним изменения запроса. Например чтобы изменить PHAsset, необходимо создать PHAssetChangeRequest. Для выполнения изменений, которые записываются обратно в библиотеку фото и отправляемые наблюдателям, аналогичный приведенному выше, необходимо:

-   Выполните операцию редактирования.
-   Сохраните данные фильтрованного изображения экземпляра PHContentEditingOutput.
-   Сделать запрос на изменение для публикации формы изменения редактирования выходные данные.

Ниже приведен пример, который записывает изменение изображения, которое применяет фильтр noir образ Core:

    void ApplyNoirFilter (object sender, EventArgs e)
    {
            
            Asset.RequestContentEditingInput (new PHContentEditingInputRequestOptions (), (input, options) => {
            
        // perform the editing operation, which applies a noir filter in this case
            var image = CIImage.FromUrl (input.FullSizeImageUrl);
            image = image.CreateWithOrientation((CIImageOrientation)input.FullSizeImageOrientation);
            var noir = new CIPhotoEffectNoir {
                Image = image
            };
            var ciContext = CIContext.FromOptions (null);
            var output = noir.OutputImage;
            var uiImage = UIImage.FromImage (ciContext.CreateCGImage (output, output.Extent));
            imageView.Image = uiImage;
        //
        // save the filtered image data to a PHContentEditingOutput instance
            var editingOutput = new PHContentEditingOutput(input);
            var adjustmentData = new PHAdjustmentData();
            var data = uiImage.AsJPEG();
            NSError error;
            data.Save(editingOutput.RenderedContentUrl, false, out error);
            editingOutput.AdjustmentData = adjustmentData;
        //
        // make a change request to publish the changes form the editing output
            PHPhotoLibrary.GetSharedPhotoLibrary.PerformChanges (() => {
                PHAssetChangeRequest request = PHAssetChangeRequest.ChangeRequest(Asset);
                request.ContentEditingOutput = editingOutput;
          },
          (ok, err) => Console.WriteLine ("photo updated successfully: {0}", ok));
      });
    }
    
Когда пользователь выбирает кнопку, применения фильтра:

![](photokit-images/image5.png "Пример фильтров")
 
И благодаря PHPhotoLibraryChangeObserver, это изменение отражается в представлении коллекции, когда пользователь переходит обратно:

![](photokit-images/image6.png "Это изменение отражается в представлении коллекции, когда пользователь переходит назад")
