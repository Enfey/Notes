# Conversational Design
## Command and Control vs Conversational 
Most smartspeakers operate with 'one-shot interactions' rather than true multi-turn conversations. The wake word e.g., Alexa is further needed to explicitly invoke the interaction, and devices then listen for predetermined periods e.g., until 2s after it hears the last input. 

So what does conversational mean? 
	For CUIs, being conversational involves thoughtful **prompt design.** By prompt design we mean the developer's design of the system response e.g., a 'prompt' for the user
		Acknowledging 'thank you' with 'you're welcome'
	This involves using **conversational markers** to make interactions feel more natural and human-like. 
### Conversational Markers
These are linguistic elements that make interactions feel more natural. 

**Timelines**: "First", "Halfway there", "Finally"
**Acknowledgments**: "Thanks", "Got it", "Alright", "Sorry about that"
**Positive feedback**: "Good job", "Nice to hear that"
**Greetings**: "Hi", "Hello", "Bye"
![[Pasted image 20251111193230.png]]



## Conversational Design Examples
![[Pasted image 20251111193324.png]]

## Context Tracking in CUIs
Maintaining conversational context across multiple turns is very important. Context refers to the background information, previous interactions, and situational awareness permitting conversational systems to respond appropriately to user inputs across multiple turns. There is no concrete definition, as context can include many different elements. 

![[Pasted image 20251111193955.png]]

### Types of Context
#### Conversational Context
- What was said earlier
- The topic being discussed
- The flow of the dialogue

#### Entity Context
- Who or what is being discussed
- Attributes of the entities mentioned
- Relationships between entities
#### Task Context
- What the user is trying to accomplish
- Where they are in a multi-step process
- Completed vs pending actions

#### User context
- User preferences and history
- User location and time zone
- User personal info
#### Environmental Context
- Time of day
- Current location
- Device being used

### Coreference resolution
The task of identifying when expressions in a text or conversation refer to the same entity. Specific aspect of context tracking focused on linking references together.

## Conversational design recommendations
- Keep track of context
- Let the user decide:
	- How long the conversation should be
	- And when to initiate the conversation
- Set user expectations
	- Be transparent about capabilities.
- Personalise responses
- Help the user understand what they can do
	- *Discoverability* is an important principle, this will be returned to in future. 
- Be concise and provide clear, actionable prompts.
- Provide error recovery
	- Status updates etc. 



# CUI Design Process and Tools

## Design Process
How does one ensure effective CUI design? 
CUIs, like other UIs, should follow a UX design process. 
	**User-Centered(UCD)** or **Human-Centered Design(HCD)**
	HDC focuses on users, their needs, and requirements, by applying human factors/ergonomics and usability knowledge and techniques to enhance the effectiveness and efficiency of a system, whilst improving human well-being, user satisfaction, and accessibility.
How do we do this for CUIs?
### Design Tools
Design tools support the taking early concepts to fully working prototypes. These include:
- Sample dialogues
- Visual mockups
- Flow
- Prototyping tools
#### Sample Dialogue
Writing sample conversations between the CUI and its users
Prevents writing prompts in isolation, which can lead to stilted experiences for users. Focus on the 5 most common use cases for the CUI. Start with the best-path scenarios, but also write for error scenarios, focusing on recovery. 
#### Mockups
Similar to wireframes and mockups. Especially useful for hybrid CUIs with graphical components. Can be combined with sample dialogue to create visual storyboards. Excellent for presenting and refining early concepts. 
#### Flow Diagrams
Decision tree style diagrams showing possible paths through CUI. Important limitation is that language interaction isn't truly hierarchical, so designers must be careful with nesting levels in this upfront design. 


# Confirmations
Crucial in CUI design for verifying user actions before execution. Over confirmation can be a problem and can frustrate users. 

## Considerations for choosing a confirmation strategy
1. **Consequences of errors**: What happens if the system gets it wrong? (Wrong flight booked vs wrong song played)
2. What **modalities** will the system have to provide feedback: audio-only, text-only, visual feedback via GUI.
3. Screen size(if applicable)
4. What type of confirmation is appropriate, explicit, implicit, a hybrid?
## Types of Confirmations
### Explicit Confirmation
Requires user to take another conversational turn to confirm.
	"I think you want to set a reminder to 'buy insurance before going skydiving next week.' Is that right?"
	User must respond to proceed
### Implicit Confirmation
Does not require additional user input to confirm the action to be taken.

## Confirmation methods
1. Three-Tiered Confidence Strategy
	Adjusts confirmation type based on system confidence level.
	Greater than 80% confidence: Implicit confirmation
	45-79% confirmation: Use explicit confirmation
	Less than 45%: Request clarification
2. Implicit confirmation
	Build part of the request into the response such that the user can be confident in the answer.
	E.g., "the weather tomorrow is" vs "the weather tomorrow in Manchester is"
3. Generic Confirmation
	Universal responses that work in many contexts
	- "I'm sorry to hear that"
	- "Tell me more"
	- "That's interesting"
	- "Thank you for sharing that with me"
4. Visual Confirmation
	Good for hybrid experiences e.g., display calendar entry on screen
5. Nonspeech Confirmation
	Physical action serves as confirmation e.g., for a request that is "turn on the lights" the response could simply be to turn them on
# Error Handling
**"When you talk to a human being, there is never an unrecoverable error state."** – Abi Jones

This principle should guide CUI error handling design.

## Traditional error handling
- **Reprompt**
	Traditional response returned if the user was not heard or understood. Not always necessary.
- **Silence**
	- Can work in much the same way as a reprompt
	- Silence in human converstion signal trouble
	- People respond to silent VUIs as indicating problems
	- But silence does not explain to the user what went wrong.
## Types of VUI errors
VUIs face more potential errors than text-only CUIs
- No speech detected
	- Pre-ASR error
	- No response returned as a result
	- Provide visual indication that system is listening, absence of indicator shows system isn't listening therefore unable to detect speech
- Speech detected, nothing recognised
	- Transcription failure
	- Didn't understand response
	- Handling strategies
		- Do nothing
			- Poor strategy, no way to recover as user
		- Indicate that system has processed the query, but isn't returning a response
		- Call it out explicitly e.g., "what was the name of the restauraunt again?"
- Something recognised incorrectly
	- Transcription error
	- Unexpected/unintended response
	- May be avoidable through confirmation
	- Partially incorrect transcriptions are very frequent
		- Present partial response for confirmation to allow user to correct when VUI less certain
		- Build error into response so user can correct and progress
- Something was recognised correctly, but the system does the wrong thing with it. 
	- Semantic error/no matching intent
	- Unexpected/unintended response
	- Common text-based CUIs too, not just voice
	- May be avoidable via disambiguation
		- Use confirmation to achieve.
## Reprompt strategies for error handling
Instead of generic error messages, progressively provide more specific guidance e.g., 
- First attempt: User says "Uhh...576782"
- System: "I'm sorry, I don't recognize that. Your flight number is three digits long and follows the letters UA."
- User (corrected): "Oh, that! It's 375."
- System: "Thank you. Getting your reservation..."
General principles include not blaming the user, and design for different user types. 

## Universal commands and help and extras
Universals: ‘global’ commands the user can always say E.g., help, stop. 
Help has specific significance in VUIs, due to invisibility of the affordances of the app



