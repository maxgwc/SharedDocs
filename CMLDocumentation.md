
# Chatbot Markup Language
XML-based language for orchestrating chatbots for use with GreenWatt VoiceBot.

## Initialization
- \<chatbot initialText="" voicemail="" availability="" appt=""> <\chatbot>
	 - The root for conversation flows originated by an outbound call
 	 - initialText: Defines the initial text to be said when the contact picks up. 
	 - voicemail: Defines the voicemail to be sent if the contact does not pick up
 	 - availability: Integer number of days to cache availability for.
   	 - appt: true if this call is related to an appointment, with appt_date and appt_time sent through GHL webhook. false otherwise.  
---
- \<chatbot_inbound>
    - The root for conversation flows originated by an inbound call
    - Will retain data from any previous outbound conversation coming from the same bot.
 ---
- \<chatbot_data>
	<\chatbot_data>
	- Defines data and performs operations before the chatbot begins.

## Control Flow
- \<if condition=""> <\if>
	- Branch based on result of evaluating the given boolean condition. Can use boolean operators ! (NOT), && (AND), || (OR)
	- Be very careful with this. Non-conditional prompts will fail.

## Conversation Activities
- \<static_message text="">
	- Sends a string message to the contact.
---
- \<ai_message prompt="" >
	- Sends a message generated by the AI engine using the given prompt.
---
- \<extract prompt="" variable=''> </extract>
	- Extracts data from the context of the conversation using the given data prompt (plaintext description of the data to get).
	- If the extraction was successful, operations within the body are executed and can access the data through the named variable.
	- Provide as much guidance within the data prompt as possible.
---
- \<infer conditional="" variable=''>
	- Evaluate the given plaintext conditional prompt.
	- Place the boolean result in the named variable.

## GHL
- \<appt_booking booked=''>
	- Books the contact's desired appointment, if possible
	- Variable declared in "booked" is a boolean which notifies if the appointment has been booked
---
- \<add_tags tags="tag1;tag2;...">
	- Adds the required tags to contact's GHL record
---
- \<remove_tags tags="tag1;tag2;...">
	- Removes the required tags from contact's GHL record
---
- \<set_field field="" variable=''>
	- Sets GHL custom field to variable value
---
- \<appt_cancel cancelled=''>
	- Cancels the most recently made appointment. If appt is enabled in chatbot declaration, the appointment the call was made for is cancelled, unless another appointment was made during the course of the call  

## Variables
- \<str_var name='' value="">
	- Declares or updates a named variable.
	- Will not evaluate expressions.
- \<bool_var name='' value="">
	- Declares or updates a boolean variable.
	- Will evaluate boolean expressions.
- \<int_var name='' value="">
	- Declares or updates a integer variable.
	- Will evaluate integer expressions.
- All values can be referenced in non-identifier strings like so: "~{name}"
	- Identifier strings are shown in this document by single quotes: ''
	- All other strings will use double quotes: ""


## Default Values
- contact_fname
- contact_lname
- contact_address
- appt_date (defined if appt is enabled)
- appt_time (defined if appt is enabled)
