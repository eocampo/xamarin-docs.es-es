---
title: Conceptos avanzados y funcionamiento interno
description: Esta guía presentan conceptos avanzados y funcionamiento interno de Xamarin.Forms. Actualmente incluye artículos sobre los representadores rápidos y .NET Standard.
ms.prod: xamarin
ms.assetid: 2273a31c-4022-42ba-befe-0d23ce2ff3b5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2017
ms.openlocfilehash: 292b0814cba446c97042ba1fe52ad9414ba74760
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203064"
---
# <a name="advanced-concepts--internals"></a>Conceptos avanzados y funcionamiento interno

## <a name="fast-renderersfast-renderersmd"></a>[Representadores rápidos](fast-renderers.md)

Este artículo presentan a los representadores rápidos, que reducen la inflación y los costos de representación de un control de Xamarin.Forms en Android mediante la reducción de la jerarquía de control nativo resultante.

## <a name="net-standardnet-standardmd"></a>[.NET Standard](net-standard.md)

En este artículo se explica cómo convertir una aplicación de Xamarin.Forms para usar .NET Standard 2.0.

## <a name="dependency-resolutiondependency-resolutionmd"></a>[Resolución de dependencias](dependency-resolution.md)

En este artículo se explica cómo insertar un método de resolución de dependencia en Xamarin.Forms, para que el contenedor de inserción de dependencia de la aplicación tiene control sobre la construcción y la duración de los representadores personalizados, los efectos, y `DependencyService` implementaciones.
