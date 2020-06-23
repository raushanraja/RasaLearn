# RasaLearn

### Level of Assistance
- Notification Assistance
- FAQ Assistance
- Contextual AI Assistance
- Personalized Assistance
- Autonomous Assistance

### RASA ML Approach , Component
#### NLU (Natural Language Understanding)
+ Ear of the assistnce
+ Extacts strcutured data into `intents` & `entity`
+ Intent
    + Intents are labels attatched to user input
    + Eg. Hello has a intent/label of greet
+ Entity
    + Information from user which assiatance may need in future
    + Eg. Name of the person


#### Core
+ Brain of the assistance
+ Manages dialogue betweeb assistance and user


### Conversation Design



### Commands
+ rasa init
+ rasa train nlu : Train on pipeline
+ rasa shell nlu : Load most recently trained model
+ rasa run actions : Run custom action server
+ rasa run actions & rasa shell : Runs both actions server and cli chatbot

### Directory Structure
+ `data` : 
    + Contains NLU training files `nlu.md`
        + NLU training training data
        + nlu.md consist of `intent label` and texts falling under that label
        + example: 
        ```md
            ## intent:greet
            - hey
            - hello
            - hi
            -good morning
            - hey there

            ## intent:goodbye
            - bye
            - goodbye
        ```
        + They also contains `entity`
        + Entities are extracted from the user input
        + Do that, we have to label the entities
        + It i s dony by adding square bracket around the word (which should be extracted as an entity)<br/> and defining the label in paranthesis next to entity.
        + example:
        ```md
            ## intent:serach_atms
            - Locate an atm in [dehradun] (location)
            - fine me an atm in [dehradun] (location)
        ```
        + Entity in above example is dehradun and location is type of entity

    + Dialogue management model `stories.md`
        + It has more than one stories defined, to train the model
        + End of stories are marked with a new line
        + Each stories ar defined in follwing sequence
        ```md
            ## story name           -> Name for story, great for debug
            * intent                -> Intent name of which the user message belongs
                - action_name       -> response of the assistant is expresses as acion name, actually defined in domain.yml
        ```
        + Example:
        ```md
            ## happy path
            * greet
              - utter_greet
            * mood_great
              - utter_happy
        ```
        + Actions Type:
            - `utter_action_name`: Hard coded actions
            - `action_custom`: Involves custom code that can be executed

+ `models`:
    + This directory contains the trained models
+ `config.yml`
    + Contains 
        + Pipeline specifiction
        + Training policies


+ `domain.yml`:
    + Components:
        + `intent` : List of intents
        + `entities` : List of entities_type 
        + `actions` : Actions
        + `templates`
            + Defines response of assistant based on intent prediction
        + `slots`
            + Assistants memory
            + Store informtion extracted as entity
            + components:
                + `slot_name` : name of extracted entity type
                + `slot_type` : type of information to store such as `text`
            + can be used in utterance (using `{slot_name}`) or custom action (using `tracker.get_slot("slot_name")` )
            + Use `SlotSet("slot_name","slot_value") in custom axtion to set some value
+ `actions.py`
    + Consists of custom action containing python code for assitant reply 
    + Custom actions runs on different server
    + Default implementation is given in endpoints.yml file
    + Command to run: `rasa run actions`



### Pipeline
+ Sequence of processing steps defined in `config.yml`
+ It extracts some feature & train components
+ Command to train on defined pipeline: `rasa train nlu`
+ Processing pipelines are used to correctly train the model 
+ And interpret user input and assistant output
+ Types:
    + Pre-configured pipelines
        + pretrained_embeddings_spacy
        + supervised_embeddedings

### Training Policies
+ Policies are responsible for deciding how to respond next
+ Defined in `config.yml`
+ Hyper-Parameters:
    + max_history
    + data augmentation
+ Types of policies
    + MemorizationPolicy : Memorize previous conversation
    + MappingPolicy : Maps intent to an action 
    + KerasPolicy : Uses keras to predict next action
    + EmbeddingPolicy
    + FormPlolicy
    + FallbackPolicy
        + nlu_threshold, core_threshold : Minimum confidence needed to trigger
        + ambiguity_threshold : Minimum difference between first and second prediction confidence
    + TwoStageFallbackPolicy
