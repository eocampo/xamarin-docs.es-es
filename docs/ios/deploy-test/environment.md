---
title: Entorno de ejecución para aplicaciones Xamarin.iOS
description: En este documento se describe cómo configurar variables de entorno temporales y permanentes para una aplicación Xamarin.iOS. Las variables se pueden especificar en las propiedades de un proyecto o como argumentos adicionales para la herramienta de empaquetado mtouch.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 246c65729f9327dd1ccf549603b4c2b1feb023e8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784971"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Entorno de ejecución para aplicaciones Xamarin.iOS

El *entorno de ejecución* es el conjunto de variables de entorno que influyen en la ejecución del programa. Las variables de entorno se pueden establecer temporalmente en las propiedades del proyecto o permanentemente mediante la definición de argumentos adicionales en la herramienta de empaquetado mtouch.

## <a name="temporary-environment-variables"></a>Variables de entorno temporales

Las variables de entorno temporales se definen en la ventana **Propiedades**/**Opciones** del proyecto en la sección **Ejecutar > General**. Estas variables de entorno solo tienen efecto si la aplicación se inicia con Visual Studio para Mac; si la aplicación se inicia manualmente al puntear en ella, estas variables de entorno no se definen.

## <a name="permanent-environment-variables"></a>Variables de entorno permanentes

Las variables de entorno permanentes se establecen mediante la definición de argumentos adicionales en la herramienta de empaquetado mtouch. Estas variables de entorno se compilan en el archivo ejecutable y se definirán incluso si la aplicación no se inicia desde Visual Studio para Mac.

## <a name="example"></a>Ejemplo

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

