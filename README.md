# idear

A general purpose [voice user interface](https://en.wikipedia.org/wiki/Voice_user_interface) for the IntelliJ Platform, inspired by [Tavis Rudd](https://www.youtube.com/watch?v=8SkdfdXWYaI). Possible use cases: visually impaired and [RSI](https://en.wikipedia.org/wiki/Repetitive_strain_injury) users. For background information, check out [this presentation](https://speakerdeck.com/breandan/programming-java-by-voice).

# Roadmap

Idear is currently a work in progress. These are some of the features we are currently working on:

## Speech Recognition

ASR is supported by [Amazon Lex](https://aws.amazon.com/lex/) and [CMU Sphinx](https://github.com/cmusphinx/sphinx4/).

### Speech-to-Text

Whether Lex manages to resolve and fulfill an intent or not, it will still return the recognised utterance in text ([unless it did not hear anything at all](https://github.com/OpenASR/idear/issues/40)).
[`LexASR`](https://github.com/OpenASR/idear/blob/master/src/main/java/org/openasr/idear/asr/awslex/LexASR.kt) and
[`CMUSphinxASR`](https://github.com/OpenASR/idear/blob/master/src/main/java/org/openasr/idear/asr/cmusphinx/CMUSphinxASR.kt)
provide a method `waitForUtterance()` which blocks until the speech to text service returns a string. 

### NLP - Text to Action

If Lex _does_ manage to resolve and fulfill (to the point where it delegates to client-side fulfillment) an intent by 
invoking a Lamba function then [`LexRecognizer`](https://github.com/OpenASR/idear/blob/master/src/main/java/org/openasr/idear/asr/awslex/LexRecogniser.kt)
notifies a `NlpResultListener` that the the request has been fulfilled or failed etc.

[`NlpProvider`](https://github.com/OpenASR/idear/blob/master/src/main/java/org/openasr/idear/nlp/NlpProvider.kt)
defines a method `processUtterance()` which takes a string utterance and context. 
[`LexNlp`](https://github.com/OpenASR/idear/blob/master/src/main/java/org/openasr/idear/nlp/lex/LexNlp.kt)
implements `NlpProvider` and notifies the `NlpResultListener`. 

## Text-to-Speech

TTS is supported by [Amazon Polly](https://aws.amazon.com/polly/) and [MaryTTS](https://github.com/marytts/marytts). 

### Activation

- [ ] User presses button or activates voice control by saying something, “Okay __, help me.”
- [ ] “Hello <system user>, welcome to the handsfree audio development interface for IntelliJ IDEA.”
- [ ] “There are a number of commands you can use, for example ‘Open settings’, ‘Find action’, ‘Open file’...”

### Visually Impaired Mode

- [ ] Action reader. When user enables a flag, any selecting menu options or actions read back to user. 
- [ ] Status updates. User says, “Run application”. Plugin responds, “building project”, “compiling application”, “running project”.
- [ ] Text selection. Plugin reads back selected region (rapidly).
- [X] User says, "Where am I?". Plugin responds, "You are inside method X, on line Y".

### Interactive Features

- [ ] User says, “open Analyze”. Plugin responds, “Would you like to ‘Inspect Code’, ‘Code Cleanup’...”
- [ ] User says, “open tip of the day”. Plugin responds, “Did you know that... <tip of the day contents>”
- [ ] User says, “activate intentions”. Plugin responds, “Would you like to ‘Invert if condition’, ‘Remove braces’,...”

### IDE Features

- [X] Understand numbers (one, two , three, four, five, six…)
  - [X] Jump to text inside the editor window
  - [X] Goto line numbers
- [ ] Understand free form language
  - [ ] Finding text in the editor
  - [ ] Performing arbitrary actions
- [ ] Menus (open + file, edit, view, navigate, code, analyze, refactor, build, run, tools, version control)
- [X] Navigation keys (“Page Up”, “Page down”, “line up”, “line down”, “go left”, “go right”)
- [X] Fixed actions (“extract method”, “expand selection”, “shrink selection”, “focus project”)
		
### Code Features

- [ ] Code generation (generate for-loop, getter, setter…)
- [ ] Refactorings
  - [ ] Extract method
  - [ ] Extract parameter
- [ ] Show intention actions
- [ ] Auto-completion
- [ ] Speech typing

# Building

Run `git clone https://github.com/OpenASR/idear && cd idear && ./gradlew runIde`.

Recognition works with most popular microphones (preferably 16kHz, 16-bit). For best results, minimize background noise.
