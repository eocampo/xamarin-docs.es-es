---
title: 'Tutorial: usar la herramienta Instruments de Apple'
description: En este artículo se describe cómo usar la herramienta Instruments de Apple para diagnosticar problemas de memoria en una aplicación iOS compilada con Xamarin. Muestra cómo iniciar Instruments, tomar instantáneas del montón y analizar el aumento de la memoria, entre otras cosas.
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4241fe9fed260091de98ba47d68b0ad5d97ed626
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785780"
---
# <a name="walkthrough---using-apples-instruments-tool"></a>Tutorial: usar la herramienta Instruments de Apple

_Este artículo es una guía de uso de la herramienta Instruments de Apple para diagnosticar problemas de memoria en una aplicación iOS compilada con Xamarin. Muestra cómo iniciar Instruments, tomar instantáneas del montón y analizar el aumento de la memoria. También muestra cómo usar Instruments para mostrar e identificar las líneas de código exactas que provocan el problema de memoria._

En esta página se muestra cómo usar la **herramienta Instruments de Xcode** para diagnosticar un problema de memoria en una aplicación de iOS.
En primer lugar, descargue el [ejemplo MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) y abra la solución **anterior** en Visual Studio para Mac.

## <a name="diagnosing-the-memory-issues"></a>Diagnóstico de problemas de memoria

1.  En Visual Studio para Mac, inicie **Instruments** desde el elemento de menú **Herramientas > Iniciar Instruments**.
2.  Cargue la aplicación en el dispositivo eligiendo el elemento de menú **Ejecutar > Upload to Device (Cargar en el dispositivo)**.
3.  Elija la plantilla **Asignaciones** (icono naranja con cuadro blanco).

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Seleccionar la plantilla Asignaciones")

4.  Seleccione la aplicación **Memory Demo** en la lista **Choose a profiling template for: (Elegir una plantilla de generación de perfiles para:)** en la parte superior de la ventana. Primero haga clic en el dispositivo iOS para expandir el menú que muestra las aplicaciones instaladas.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Seleccionar la aplicación MemoryDemo")

5.  Pulse el botón **Elegir** (extremo inferior derecho de la ventana) para iniciar **Instruments**. Esta plantilla mostrará dos elementos en el panel superior: Asignaciones y VM Tracker (Seguimiento de VM).

6.  Pulse el botón **Grabar** (círculo rojo del extremo superior izquierdo) de Instruments, lo que iniciará la aplicación.

7.  Seleccione la fila **VM Tracker (Seguimiento de VM)** del panel superior (ahora que la aplicación se está ejecutando, contiene dos secciones: Dirty y Resident Size). En el panel **Inspector**, elija la opción **Show Display Settings (Mostrar configuración de pantalla)** (el icono de engranaje) y luego active la casilla **Automatic Snapshotting (Creación automática de instantáneas)** del extremo inferior derecho de esta captura de pantalla:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Elegir la opción para mostrar la configuración de pantalla en el icono de engranaje y luego marque la casilla de creación automática de instantáneas")

8.  Seleccione la fila **Asignaciones** del panel superior (ahora que la aplicación se está ejecutando indicará *All Heap and Anonymous VM*)
9.  En el panel **Inspector**, elija la opción **Show Display Settings (Mostrar configuración de pantalla)** (el icono de engranaje) y luego haga clic en el botón **Mark Generation (Generación de marca)** para establecer una línea de base. Aparecerá un pequeño indicador rojo en la escala de tiempo de la parte superior de la ventana
10.  Desplácese por la aplicación y vuelva a seleccionar **Mark Generation (Generación de marca)** (repita varias veces)
11.  Haga clic en el botón **Detener**.
12.  Expanda el nodo **Generación** con el mayor **Crecimiento** y ordene por **Crecimiento** (descendente).
13.  Vaya del panel **Inspector** a **Show Extended Detail (Mostrar más detalles)** (la "E"), que muestra el **Seguimiento de la pila**.

14.  Observe que el nodo **<no objeto>** muestra un aumento excesivo de la memoria. Haga clic en la flecha situada junto a este nodo para ver más detalles. Haga clic con el botón derecho en el seguimiento de la pila para agregar **Ubicación de origen** al panel:

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "Agregar ubicación de origen al panel")

15.  Ordene por **Tamaño** y muestre la vista **Extended Detail** (Más detalles):

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Ordenar por tamaño y mostrar la lista con más detalles")

16.  Haga clic en la entrada deseada en la pila de llamadas para ver el código relacionado:

    ![](walkthrough-apples-instrument-images/05-related-code.png "Ver el código relacionado")

En este caso, se crea una nueva imagen y se almacena en una colección para cada celda; no se reutilizan las celdas de la vista de colección existentes.

## <a name="resolving-the-memory-issues"></a>Solución de problemas de memoria

Es posible solucionar estos problemas y volver a ejecutar la aplicación mediante Instruments.

Al declarar una sola instancia en el nivel de clase, se puede volver a usar la imagen y el objeto de celda de un grupo existente en lugar de crearlos cada vez, como se muestra a continuación:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Dequeue a cell from the reuse pool
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);

    // Reuse the image declared at the class level
    imageCell.ImageView.Image = image;

    return imageCell;
}
```

Ahora, cuando se ejecuta la aplicación, se reduce enormemente el uso de memoria: el **Crecimiento** entre generaciones ahora se mide en k (kilobytes) en lugar de MB (megabytes), como se hacía antes de corregir el código:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Mostrar el uso de la memoria de la aplicación")

El código mejorado está disponible en el [ejemplo MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) de la solución **posterior** en Visual Studio para Mac.

Este blog de la comunidad sobre [Recolección de elementos no utilizados de Xamarin.iOS](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/) es una referencia útil para solucionar problemas de memoria con Xamarin.iOS.


## <a name="summary"></a>Resumen

En este artículo se muestra cómo usar Instruments para diagnosticar problemas de memoria.
En él se ha explicado cómo iniciar Instruments desde Visual Studio para Mac, cargar la plantilla de asignación de memoria y usar instantáneas para identificar problemas de memoria.
Por último, se ha vuelto a examinar la aplicación para comprobar que se ha corregido el problema.


## <a name="related-links"></a>Vínculos relacionados

- [Ejemplo MemoryDemo](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Recolección de elementos no utilizados de Xamarin.iOS](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
