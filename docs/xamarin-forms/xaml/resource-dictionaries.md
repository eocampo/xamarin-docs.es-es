---
title: Diccionarios de recursos
description: Recursos XAML son objetos que se pueden compartir y volver a utilizar en una aplicación de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: c2e6a5624baba251061bcd324fcb849e3d95ebfd
ms.sourcegitcommit: c2d1249cb67b877ee0d9cb8d095ec66fd51d8c31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291040"
---
# <a name="resource-dictionaries"></a>Diccionarios de recursos

_Recursos XAML son definiciones de objetos que se pueden compartir y volver a utilizar en una aplicación de Xamarin.Forms._

Estos objetos de recursos se almacenan en un diccionario de recursos. Este artículo describe cómo crear y utilizar un diccionario de recursos y cómo se combinan los diccionarios de recursos.

## <a name="overview"></a>Información general

A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) es un repositorio para los recursos utilizados por una aplicación de Xamarin.Forms. Recursos típicos que se almacenan en un `ResourceDictionary` incluyen [estilos](~/xamarin-forms/user-interface/styles/index.md), [plantillas de control](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [plantillas de datos](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), colores y convertidores.

En XAML, los recursos que se almacenan en un `ResourceDictionary` , a continuación, se recuperan y aplican a los elementos mediante el uso de la `StaticResource` extensión de marcado. En C#, también se pueden definir los recursos en un `ResourceDictionary` y, a continuación, recuperar y aplicar a los elementos mediante el uso de un indizador basada en cadena. Sin embargo, hay muy pocas ventajas en usar un `ResourceDictionary` en C#, como objetos compartidos pueden simplemente se almacenan como campos o propiedades y acceso directamente sin tener que primero recuperarlos de un diccionario.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Crear y consumir un ResourceDictionary

Los recursos se definen en un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) es que después se establece en uno de los siguientes `Resources` propiedades:

- El [ `Resources` ](xref:Xamarin.Forms.Application.Resources) propiedad de cualquier clase que deriva de [`Application`](xref:Xamarin.Forms.Application)
- El [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) propiedad de cualquier clase que deriva de [`VisualElement`](xref:Xamarin.Forms.Application)

Un programa de Xamarin.Forms contiene solo una clase que deriva de `Application` , pero a menudo hace uso de muchas clases que derivan de `VisualElement`, incluidos los controles, diseños y páginas. Cualquiera de estos objetos pueden tener sus `Resources` propiedad establecida en un `ResourceDictionary`. Elegir dónde colocar un determinado `ResourceDictionary` impactos donde se pueden usar los recursos:

- Recursos en un `ResourceDictionary` que está conectada a una vista como `Button` o `Label` solo puede aplicarse a ese objeto concreto, por lo que no resulta muy útil.
- Recursos en un `ResourceDictionary` adjunta a un diseño como `StackLayout` o `Grid` pueden aplicarse para el diseño y todos los elementos secundarios de ese diseño.
- Recursos en un `ResourceDictionary` definido en la página de nivel puede aplicarse a la página y a todos sus elementos secundarios.
- Recursos en un `ResourceDictionary` definido en la aplicación de nivel se puede aplicar a lo largo de la aplicación.

El código XAML siguiente muestra los recursos definidos en un nivel de aplicación `ResourceDictionary` en el **App.xaml** archivo creado como parte del programa de Xamarin.Forms estándar:

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Esto `ResourceDictionary` define tres [ `Color` ](xref:Xamarin.Forms.Color) recursos y un [ `Style` ](xref:Xamarin.Forms.Style) recursos. Para obtener más información sobre la `App` de clases, consulte [clase App](~/xamarin-forms/app-fundamentals/application-class.md).

A partir de Xamarin.Forms 3.0, el explícita `ResourceDictionary` etiquetas no son necesarias. El `ResourceDictionary` objeto se crea automáticamente y se pueden insertar directamente entre los recursos del `Resources` etiquetas de elemento de propiedad:

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

Cada recurso tiene una clave que se especifica utilizando el `x:Key` atributo, que se convierte en la clave de diccionario en el `ResourceDictionary`. La clave se usa para recuperar un recurso desde el `ResourceDictionary` por el [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) extensión de marcado, como se muestra en el siguiente ejemplo de código XAML que se muestra recursos adicionales definidos dentro de la `StackLayout`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

La primera [ `Label` ](xref:Xamarin.Forms.Label) instancia recupera y consume el `LabelPageHeadingStyle` recursos definidos en el nivel de aplicación `ResourceDictionary`, con la segunda `Label` instancia recuperar y consumir la `LabelNormalStyle`recursos definidos en el nivel de control `ResourceDictionary`. De forma similar, el [ `Button` ](xref:Xamarin.Forms.Button) instancia recupera y consume el `NormalTextColor` recursos definidos en el nivel de aplicación `ResourceDictionary`y el `MediumBoldText` recursos definidos en el nivel de control `ResourceDictionary`. El resultado es el aspecto que se muestra en las capturas de pantalla siguiente:

[![](resource-dictionaries-images/screenshots-sml.png "Consumiendo recursos ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "consumiendo recursos ResourceDictionary")

> [!NOTE]
> Recursos que son específicos de una sola página no deben incluirse en un aplicación nivel diccionario de recursos, como por ejemplo, recursos, a continuación, se analizará al iniciar la aplicación en lugar de cuando se solicite una página. Para obtener más información, consulte [reducir el tamaño de diccionario de recursos de aplicación](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Reemplazar recursos

Cuando `ResourceDictionary` compartan recursos `x:Key` valores de atributo, definidos más abajo en la jerarquía de la vista de recursos tienen prioridad sobre los definidos mayor seguridad. Por ejemplo, si se establece la `PageBackgroundColor` recurso `Blue` en la aplicación de nivel serán reemplazado por un nivel de página `PageBackgroundColor` del conjunto de recursos `Yellow`. De forma similar, un nivel de página `PageBackgroundColor` recursos serán reemplazados por un nivel de control `PageBackgroundColor` recursos. Esta prioridad se muestra en el ejemplo de código XAML siguiente:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

La versión original `PageBackgroundColor` y `NormalTextColor` instancias, definidas en el nivel de aplicación, son reemplazadas por la `PageBackgroundColor` y `NormalTextColor` instancias definidas en el nivel de página. Por lo tanto, el color de fondo de página se convierte en azul, y el texto en la página, se convierte en amarillo, como se muestra en las capturas de pantalla siguiente:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Reemplazar recursos ResourceDictionary")](resource-dictionaries-images/overridding-screenshots.png#lightbox "reemplazar ResourceDictionary recursos")

Sin embargo, tenga en cuenta que la barra de fondo de la [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) está de color amarillo todavía, porque la [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) propiedad está establecida en el valor de la `PageBackgroundColor` recursos definidos en la aplicación nivel de `ResourceDictionary`.

Ésta es otra manera de pensar en `ResourceDictionary` prioridad: cuando XAML el analizador encuentra un `StaticResource`, busca una clave coincidente por seguridad recorre el árbol visual, con la primera coincidencia que encuentra. Si esta búsqueda finaliza en la página y la clave no se ha encontrado todavía, el analizador de XAML busca la `ResourceDictionary` conectado a la `App` objeto. Si todavía no se encuentra la clave, se produce una excepción.

## <a name="stand-alone-resource-dictionaries"></a>Diccionarios de recursos independiente

Una clase derivada de `ResourceDictionary` también pueden estar en un archivo independiente independiente. (Más concretamente, una clase derivada de `ResourceDictionary` requiere normalmente un _par_ de archivos porque los recursos se definen en un archivo XAML, pero un archivo de código subyacente con una `InitializeComponent` llamada también es necesaria.) El archivo resultante, a continuación, se puede compartir entre aplicaciones.

Para crear este archivo, agregue un nuevo **vista de contenido** o **página contenido** elemento al proyecto (pero no un **vista de contenido** o **página contenido** con solo un archivo C#). En el archivo XAML y el archivo de C#, cambie el nombre de la clase base desde `ContentView` o `ContentPage` a `ResourceDictionary`. En el archivo XAML, el nombre de la clase base es el elemento de nivel superior.

El ejemplo XAML siguiente se muestra un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) denominado `MyResourceDictionary`:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

Esto `ResourceDictionary` contiene un único recurso, que es un objeto de tipo `DataTemplate`.

Puede crear instancias de `MyResourceDictionary` colocándola entre un par de `Resources` elemento de la propiedad etiquetas, por ejemplo, en un `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Una instancia de `MyResourceDictionary` está establecido en el `Resources` propiedad de la `ContentPage` objeto.

Sin embargo, este enfoque tiene algunas limitaciones: la `Resources` propiedad de la `ContentPage` hace referencia a esta sola `ResourceDictionary`. En la mayoría de los casos, desea que la opción de incluir otros `ResourceDictionary` instancias y quizás también otros recursos.

Esta tarea requiere diccionarios de recursos combinados.

## <a name="merged-resource-dictionaries"></a>Diccionarios de recursos combinados

Diccionarios de recursos combinados combinan uno o varios `ResourceDictionary` instancias a otro `ResourceDictionary`. Puede hacerlo en un archivo XAML estableciendo el [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) propiedad a uno o varios diccionarios de recursos que se combinará en la aplicación, la página o el nivel de control `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` También define un [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) propiedad. No utilice esta propiedad; se ha quedado obsoleta en Xamarin.Forms 3.0.

Y la instancia de `MyResourceDictionary` se pueden combinar en cualquier aplicación, la página o el nivel de control `ResourceDictionary`. El ejemplo de código XAML siguiente muestra lo que se combinan en un nivel de página `ResourceDictionary` mediante el `MergedDictionaries` propiedad:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Ese marcado muestra solo una instancia de `MyResourceDictionary` que se va a agregar a la `ResourceDictionary` , pero también puede hacer referencia otros `ResourceDictionary` instancias dentro de la `MergedDictionary` etiquetas de elemento de propiedad y otros recursos fuera de esas etiquetas:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary.MergedDictionaries>

                <!-- Add more resource dictionaries here -->

                <local:MyResourceDictionary />

                <!-- Add more resource dictionaries here -->

            </ResourceDictionary.MergedDictionaries>

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Puede haber solo un `MergedDictionaries` sección un `ResourceDictionary`, pero puede incluir tantos `ResourceDictionary` instancias allí como desee.

Cuando se combinan [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) recursos compartan idénticos `x:Key` valores de atributo, Xamarin.Forms utiliza la prioridad de los recursos siguientes:

1. Los recursos locales para el diccionario de recursos.
1. Los recursos incluidos en el diccionario de recursos que se ha combinado a través de las regiones [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) propiedad.
1. Los recursos contenidos en los diccionarios de recursos que se combinaron a través de la `MergedDictionaries` colección, en el orden inverso aparecen en la `MergedDictionaries` propiedad.

> [!NOTE]
> Buscar en diccionarios de recursos puede ser una tarea de cálculo intensiva si una aplicación contiene varios diccionarios de recursos de gran tamaño. Por lo tanto, para evitar búsquedas innecesarias, debe asegurarse de que cada página de una aplicación solo usa los diccionarios de recursos que son adecuados para la página.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Combinación de diccionarios de Xamarin.Forms 3.0

A partir de Xamarin.Forms 3.0, el proceso de combinar [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) instancias se ha convertido en algo más fácil y más flexibles. El `MergedDictionaries` etiquetas de elemento de propiedad ya no son necesarias. En su lugar, se agrega al diccionario de recursos otro `ResourceDictionary` etiqueta con el nuevo [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) propiedad establecida en el nombre de archivo del archivo XAML con los recursos:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary Source="MyResourceDictionary.xaml" />

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Porque Xamarin.Forms 3.0 crea automáticamente una instancia el `ResourceDictionary`, esos dos externas `ResourceDictionary` etiquetas ya no son necesarias:

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Add more resources here -->

        <ResourceDictionary Source="MyResourceDictionary.xaml" />

        <!-- Add more resources here -->

    </ContentPage.Resources>
    ...
</ContentPage>
```

Esta nueva sintaxis _no_ crear una instancia de la `MyResourceDictionary` clase. En su lugar, hace referencia al archivo XAML. Por esa razón el archivo de código subyacente (**MyResourceDictionary.xaml.cs**) ya no es necesario. También puede quitar el `x:Class` atributo de la etiqueta raíz de la **MyResourceDictionary.xaml** archivo.

## <a name="summary"></a>Resumen

Este artículo explica cómo crear y consumir un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)y cómo se combinan los diccionarios de recursos. Un `ResourceDictionary` permite a los recursos definidos en una sola ubicación y volver a utilizar en una aplicación de Xamarin.Forms.

## <a name="related-links"></a>Vínculos relacionados

- [Diccionarios de recursos (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Estilos](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
