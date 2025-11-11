# CUI
> Refers to conversational user interface

Can refer to text-based chatbots, or alternatively voice based VUIs or hybrid versions of GUI and voice such as Siri. There are many 'faces' of conversational interfaces. 

The term user experience is a broad one; encompassing the general user felt experience of the interaction with the application, not just the design of the interfaces and information.

Widespread adoption of CUIs varies depending on the nature of the CUI, and how that reflects the values and needs of a target audience.
In late 2021, 39% of adults in England owned and used at home, a Voice-activated personal assistant or smart speaker device, according to a survey commissioned by the UK government.

## VUI interaction
It should be noted that there are a host of problems in developing a usable VUI. Voice is a profoundly personal trait, and reflects where you're from, sex/gender, sexual orientation, your socio-economic background and more. There are also issues regarding privacy and surveillance, pertinent given the often intimate/home environment in which VUIs are placed. 
### User Input
User: Alexa, remind me to fix my clock.
Alexa: I put fix my clock on your todo list. 

What happens between the request and response? 

### ASR
Alexa converts spoken words into text. It has several components.

a) **Wakeword detection**
	Alexa is always listening for the wake word, when detected, it initialises and starts processing the rest of speech. 
b) **Acoustic Model-Based Transcription**
	Converts the audio into text
	The acoustic model maps sound waves to phonemes
	These phonemes are combined into words using a language model
c) **Confidence Level**
	ASR assigns a confidence score to each recognised word or prhase
	If the confidence is low, Alexa might ask for clarification

### NLU/NLP
Once speech has been converted into text, Alexa must gauge intent.

a) **Keyword Recognition**#
	Identifying important words like 'remind', 'fix', 'clock'
b) **Intent Matching**
	Determines the intent behind the request
	Here the intent is add reminder
c) **Variable/Slot Filling**
	Extract details from the sentence, acting as structured data the system can act upon
	E.g., extract **remind** as the action, and the task as 'fix my clock'

### Dialogue Management + TTS(Text-to-Speech)
After understanding the request, must decide how to respond. Must decide if reminder was successfully added, or more information needed e.g., time for the reminder. TTS converts Alexa's response text into speech that we hear. 

Following this is response generation, "I put 'fix my clock' on your todo list" confirming to the user that the task has been added. 

## Socio-economic challenges
Over 7000 actively spoken languages. 91 of them have at least 10 million speakers. As of late 2022, Alexa supports 8 languages, and dialects in only 3 of those. VUIs struggle more with certain dialects, and the accuracy is lower for higher pitched voices, resulting in a gender and ageist bias. 

Furthermore, such systems are often prone to racial bias, ASR systems at IBM, Google, Amazon, Apple, Microsoft analysed. Tested on transcription of structured interviews conducted with 42 white speakers and 73 black speakers. Found that all five ASR systems exhibited substantial racial disparaties. An average word error rate of 0.35 for black speakers compared with 0.19 for white speakers. peakers compared with 0.19 for white speakers. § WER standard measure of discrepancy between machine and human transcription, based on substitutions, insertions, deletions. § Authors trace these disparities to the underlying acoustic models used by the ASR systems.

Sociophonetics a field of linguistics that combines phonetics and sociolinguistics to study how social factors, like identity, class, and background, influence the sounds of a language and how these sounds are learned. Phonetics are the study of the production and perception of spoken language. VUIs synthesised speech generally represents a homogenous, mainstream accent – lack of diversity in what this voice represents. One can ask, why are voice assistants generally giiven a female voice, user trials, people preferred female voices. One must ask whether these trials can  sufficiently represent the entire potential user base. Regardless, gendering is embedding gender stereotypes. 


