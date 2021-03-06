---
title: Modificadores de campo XAML de Xamarin.Forms
description: 'El atributo de espacio de nombres de x: FieldModifier especifica el nivel de acceso para los campos generados para los elementos XAML con nombre.'
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209514"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Modificadores de campo XAML de Xamarin.Forms

_El `x:FieldModifier` atributo de espacio de nombres especifica el nivel de acceso para los campos generados para los elementos XAML con nombre._

## <a name="overview"></a>Información general

Los valores válidos del atributo son:

- `Public` : Especifica que el campo generado para el elemento XAML es `public`.
- `NotPublic` : Especifica que el campo generado para el elemento XAML está `internal` al ensamblado.

Si el valor del atributo no está configurado, el campo generado para el elemento estará `private`.

Las siguientes condiciones deben cumplirse para un `x:FieldModifier` atributo para ser procesados:

- El elemento de nivel superior XAML debe ser válido `x:Class`.
- El elemento XAML actual tiene un `x:Name` especificado.

El código XAML siguiente muestra ejemplos de establecer el atributo:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> El `x:FieldModifier` atributo no puede utilizarse para especificar el nivel de acceso de una clase XAML.
