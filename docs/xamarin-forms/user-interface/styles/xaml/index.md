---
title: Aplicar estilos a las aplicaciones de Xamarin.Forms con estilos XAML
description: Esta guía explica cómo personalizar la apariencia de una aplicación de Xamarin.Forms con estilos XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 5145572b30302e58c36250fff40e8b637fcd221f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995080"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Aplicar estilos a las aplicaciones de Xamarin.Forms con estilos XAML

## <a name="introductionintroductionmd"></a>[Introducción](introduction.md)

A menudo, las aplicaciones de Xamarin.Forms contienen varios controles que tienen una apariencia idéntica. Establecer la apariencia de cada control individual puede ser repetitiva y propensas a errores. En su lugar, los estilos pueden crearse que personalizar la apariencia del control mediante la agrupación y propiedades de configuración disponibles en el tipo de control.

## <a name="explicit-stylesexplicitmd"></a>[Estilos explícitos](explicit.md)

Un *explícita* estilo es aquel que se aplica de manera selectiva a los controles estableciendo sus [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) propiedades.

## <a name="implicit-stylesimplicitmd"></a>[Estilos implícitos](implicit.md)

Un *implícita* estilo es aquella que se usa por todos los controles del mismo [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), sin necesidad de cada control para hacer referencia al estilo.

## <a name="global-stylesapplicationmd"></a>[Estilos globales](application.md)

Los estilos pueden estar disponibles globalmente agregándolos a la aplicación [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). Esto ayuda a evitar la duplicación de estilos a través de las páginas o controles.

## <a name="style-inheritanceinheritancemd"></a>[Herencia de estilo](inheritance.md)

Los estilos pueden heredar de otros estilos para reducir la duplicación y habilitar la reutilización.

## <a name="dynamic-stylesdynamicmd"></a>[Estilos dinámicos](dynamic.md)

Los estilos no responder a los cambios de propiedad y permanecen sin cambios para la duración de una aplicación. Sin embargo, las aplicaciones pueden responder a cambios de estilo dinámicamente en tiempo de ejecución mediante el uso de recursos dinámicos.

## <a name="device-stylesdevicemd"></a>[Estilos de dispositivo](device.md)

Xamarin.Forms incluye seis *dinámica* estilos, conocidos como *dispositivo* estilos, en el [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) clase. Todos los seis estilos pueden aplicarse a [ `Label` ](xref:Xamarin.Forms.Label) solo instancias.
