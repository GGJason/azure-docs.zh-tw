---
title: 如何指定語音轉換文字的來來源語言
titleSuffix: Azure Cognitive Services
description: 語音 SDK 可讓您在將語音轉換成文字時，指定來源語言。 本文說明如何使用 FromConfig 和 SourceLanguageConfig 方法，讓語音服務知道來源語言，並提供自訂模型目標。
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 32c08af129172fb1dbebf1679ea01694e8bd3d1a
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653291"
---
# <a name="specify-source-language-for-speech-to-text"></a>指定語音轉換文字的來來源語言

在本文中，您將瞭解如何針對傳遞至語音 SDK 以進行語音辨識的音訊輸入，指定來源語言。 此外，也會提供範例程式碼來指定自訂語音模型，以改善辨識。

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>如何在 C 中指定來源語言#

在此範例中，會使用結構明確地提供來來源語言做為參數 `SpeechRecognizer` 。

```csharp
var recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

在此範例中，會使用來提供來來源語言 `SourceLanguageConfig` 。 然後， `sourceLanguageConfig` 會傳遞做為參數以進行 `SpeechRecognizer` 結構。

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

在此範例中，會使用來提供來來源語言和自訂端點 `SourceLanguageConfig` 。 然後， `sourceLanguageConfig` 會傳遞做為參數以進行 `SpeechRecognizer` 結構。

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE", "The Endpoint ID for your custom model.");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SpeechRecognitionLanguage`和 `EndpointId` set 方法已從 `SpeechConfig` c # 中的類別取代。 不建議使用這些方法，而不應在建立時使用 `SpeechRecognizer` 。

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>如何在 c + + 中指定來源語言

在此範例中，會使用方法，將原始語言明確地提供給參數 `FromConfig` 。

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

在此範例中，會使用來提供來來源語言 `SourceLanguageConfig` 。 然後，在 `sourceLanguageConfig` 建立時，會將做為參數傳遞至 `FromConfig` `recognizer` 。

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

在此範例中，會使用來提供來來源語言和自訂端點 `SourceLanguageConfig` 。 在 `sourceLanguageConfig` 建立時，會將當做參數傳遞至 `FromConfig` `recognizer` 。

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SetSpeechRecognitionLanguage`和 `SetEndpointId` 已被取代為 `SpeechConfig` c + + 和 JAVA 中類別的方法。 不建議使用這些方法，而不應在建立時使用 `SpeechRecognizer` 。

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>如何在 JAVA 中指定來源語言

在此範例中，建立新的時，會明確地提供來來源語言 `SpeechRecognizer` 。

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

在此範例中，會使用來提供來來源語言 `SourceLanguageConfig` 。 然後，在 `sourceLanguageConfig` 建立新的時，會傳遞做為參數 `SpeechRecognizer` 。

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

在此範例中，會使用來提供來來源語言和自訂端點 `SourceLanguageConfig` 。 然後，在 `sourceLanguageConfig` 建立新的時，會傳遞做為參數 `SpeechRecognizer` 。

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `setSpeechRecognitionLanguage`和 `setEndpointId` 已被取代為 `SpeechConfig` c + + 和 JAVA 中類別的方法。 不建議使用這些方法，而不應在建立時使用 `SpeechRecognizer` 。

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>如何在 Python 中指定來源語言

在此範例中，會使用結構明確地提供來來源語言做為參數 `SpeechRecognizer` 。

```Python
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, language="de-DE", audio_config=audio_config)
```

在此範例中，會使用來提供來來源語言 `SourceLanguageConfig` 。 然後， `SourceLanguageConfig` 會傳遞做為參數以進行 `SpeechRecognizer` 結構。

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

在此範例中，會使用來提供來來源語言和自訂端點 `SourceLanguageConfig` 。 然後， `SourceLanguageConfig` 會傳遞做為參數以進行 `SpeechRecognizer` 結構。

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE", "The Endpoint ID for your custom model.")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

>[!Note]
> `speech_recognition_language`和 `endpoint_id` 屬性已從 `SpeechConfig` Python 中的類別取代。 不建議使用這些屬性，而且不應在建立時使用 `SpeechRecognizer` 。

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>如何在 JAVAscript 中指定來源語言

第一個步驟是建立 `SpeechConfig` ：

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

接下來，使用下列方式指定音訊的來來源語言 `speechRecognitionLanguage` ：

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

如果您使用自訂模型進行辨識，則可以使用來指定端點 `endpointId` ：

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>如何指定目標中的來源語言-C

在此範例中，會使用結構明確地提供來來源語言做為參數 `SPXSpeechRecognizer` 。

```Objective-C
SPXSpeechRecognizer* speechRecognizer = \
    [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig language:@"de-DE" audioConfiguration:audioConfig];
```

在此範例中，會使用來提供來來源語言 `SPXSourceLanguageConfiguration` 。 然後， `SPXSourceLanguageConfiguration` 會傳遞做為參數以進行 `SPXSpeechRecognizer` 結構。

```Objective-C
SPXSourceLanguageConfiguration* sourceLanguageConfig = [[SPXSourceLanguageConfiguration alloc]init:@"de-DE"];
SPXSpeechRecognizer* speechRecognizer = [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                                                     sourceLanguageConfiguration:sourceLanguageConfig
                                                                              audioConfiguration:audioConfig];
```

在此範例中，會使用來提供來來源語言和自訂端點 `SPXSourceLanguageConfiguration` 。 然後， `SPXSourceLanguageConfiguration` 會傳遞做為參數以進行 `SPXSpeechRecognizer` 結構。

```Objective-C
SPXSourceLanguageConfiguration* sourceLanguageConfig = \
        [[SPXSourceLanguageConfiguration alloc]initWithLanguage:@"de-DE"
                                                     endpointId:@"The Endpoint ID for your custom model."];
SPXSpeechRecognizer* speechRecognizer = [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                                                     sourceLanguageConfiguration:sourceLanguageConfig
                                                                              audioConfiguration:audioConfig];
```

>[!Note]
> `speechRecognitionLanguage`和 `endpointId` 屬性已從 `SPXSpeechConfiguration` 目標-C 中的類別取代。 不建議使用這些屬性，而且不應在建立時使用 `SPXSpeechRecognizer` 。

::: zone-end

## <a name="see-also"></a>另請參閱

* 如需支援的語言和語音轉換文字的地區設定清單，請參閱[語言支援](language-support.md)。

## <a name="next-steps"></a>後續步驟

* [語音 SDK 參考檔](speech-sdk.md)