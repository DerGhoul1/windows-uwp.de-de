---
Description: Erfahren Sie, wie Sie Probleme mit der Genauigkeit der Spracherkennung behandeln, die auf die Qualität der Audioeingabe zurückzuführen sind.
title: Verwalten von Problemen bei der Audioeingabe
ms.assetid: 3E36C683-C96A-4FEE-AD52-FDB87E0CC299
label: Manage audio input issues
template: detail.hbs
keywords: Sprache, Stimme, Spracherkennung, natürliche Sprache, diktieren, Eingabe, Benutzerinteraktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6281165d64b8e6e3f77807dbafd6bfff1dd0704f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258330"
---
# <a name="manage-issues-with-audio-input"></a>Verwalten von Problemen bei der Audioeingabe


Erfahren Sie, wie Sie Probleme mit der Genauigkeit der Spracherkennung behandeln, die auf die Qualität der Audioeingabe zurückzuführen sind.

> **Wichtige APIs**: [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), [**RecognitionQualityDegrading**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading), [**SpeechRecognitionAudioProblem**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem)


## <a name="assess-audio-input-quality"></a>Bewerten der Qualität der Audioeingabe


Wenn die Spracherkennung aktiviert ist, verwenden Sie das [**RecognitionQualityDegrading**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading)-Ereignis Ihrer Spracherkennung, um festzustellen, ob Audioprobleme die Spracheingabe stören. Das Ereignisargument ([**SpeechRecognitionQualityDegradingEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs)) enthält die [**Problem**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionqualitydegradingeventargs.problem)-Eigenschaft, die die Probleme mit der Audioeingabe aufzeigt.

Die Erkennung kann durch zu viele Hintergrundgeräusche, eine Stummschaltung des Mikrofons und die Lautstärke oder Geschwindigkeit des Lautsprechers beeinflusst werden.

Hier konfigurieren wir eine Spracherkennung und beginnen, auf das [**RecognitionQualityDegrading**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading)-Ereignis zu lauschen.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = "Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="manage-the-speech-recognition-experience"></a>Verwalten der Spracherkennungsfunktion


Mit der bereitgestellten Beschreibung der [**Problem**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionqualitydegradingeventargs.problem)-Eigenschaft können die Benutzer die Bedingungen für die Spracherkennung verbessern.

Hier erstellen wir einen Handler für das [**RecognitionQualityDegrading**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognitionqualitydegrading)-Ereignis, der überprüft, ob die Lautstärke niedrig ist. Anschließend verwenden wir ein [**SpeechSynthesizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer)-Objekt, um den Benutzer aufzufordern, lauter zu sprechen.

```CSharp
private async void speechRecognizer_RecognitionQualityDegrading(
    Windows.Media.SpeechRecognition.SpeechRecognizer sender,
    Windows.Media.SpeechRecognition.SpeechRecognitionQualityDegradingEventArgs args)
{
    // Create an instance of a speech synthesis engine (voice).
    var speechSynthesizer =
        new Windows.Media.SpeechSynthesis.SpeechSynthesizer();

    // If input speech is too quiet, prompt the user to speak louder.
    if (args.Problem == Windows.Media.SpeechRecognition.SpeechRecognitionAudioProblem.TooQuiet)
    {
        // Generate the audio stream from plain text.
        Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream;
        try
        {
            stream = await speechSynthesizer.SynthesizeTextToStreamAsync("Try speaking louder");
            stream.Seek(0);
        }
        catch (Exception)
        {
            stream = null;
        }

        // Send the stream to the MediaElement declared in XAML.
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.High, () =>
        {
            this.media.SetSource(stream, stream.ContentType);
        });
    }
}
```

## <a name="related-articles"></a>Verwandte Artikel


* [Sprachinteraktionen](speech-interactions.md)

**Beispiele**
* [Beispiel für Spracherkennung und Sprachsynthese](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




