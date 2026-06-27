[[skyrimnet]]'s [[skyrimnet/actions]]

### What do you want?
Suggestions for YAML Actions/Tags 


### Single Source of Truth 
* We should support a shared source of truth for a category's child actions.
    * This makes it easier to update and results in fewer errors.
* To this end, we are adding an `actions` field to the category.
    * Each child inherits its parent's values but can overwrite them.
    * The parent acts as the single source of truth.

### Parameter Arrays Build Up Over Time in Order 
* Each child action inherits all the parent's parameters, plus any it defines that are not in the parent's set. 
* So, if the category defines 2 parameters, all child actions will have 2 or more parameters.
* The order of the parameters is based on the order they are defined in the parent category and then the child action.

### Stopping Function (Optional)
* Called if the action should be stopped; allows the action to be killed cleanly. 
* The stopping function is always passed the speaker parameter.

### Categories and Actions Should Have Uniform Presentation to the LLM 
* The LLM is more likely to trigger if it doesn't need to choose between different types of values.
    * **Current category:** `- ACTION: Furniture — Interact with furniture — sit, sleep, or stand up.`
    * **Unified category:** `- ACTION: Interact_Furniture PARAMS: {"type":"sit on|sleep on|stand up from"} — [type] furniture`

### Tags Should Act as Locks 
* Tags act as actor locks.
    * An action is ineligible if the actor is locked.
    * Plugins are required to correctly lock and unlock in their code.
    * `bool function actor_lock(Actor akActor, String tag, int priority)`
        * Will attempt to lock the actor.
        * A higher-priority action will change the actor's lock to itself and then call the previous script's stopping function. 
    * `bool function actor_unlock(Actor akActor, String tag)`
        * Will attempt to unlock the actor; this only works if the script owns the lock. 
* Actions/Categories have an implied set of tags: `quest.category` and `quest.action`. 
    * This allows new mods to handle the edge case where an existing mod/action didn't check any tags, but still needs to be locked out.
* tags names should reflect what the action needs to do: 
  * not_busy: general tag to lock out full body actions 
  * can_use_hands: will need to use hands for this action 
  * can_use_magic: needs to be able to use magic 
  * can_use_legs: needs for moving or kicking 

### ParameterMapping's Descriptions are Parsed by Inja at Runtime 
* This moves the dynamic parameters list out of the action's description back into the parameter list where they belong.

### Category/Action Namespaces 
* Provides a namespace for categories/actions so the variables can be called and set once.
* variable added to eligiblityRules, 

### Example 

```yaml
- type: category
  category: cuddling
  questEditorId: Cuddle_Main
  scriptName: Cuddle_Actions
  executionFunctionName: Start_Cuddle
  namespace:  {{render_template("helpers/cuddle_variables")}}
  stoppingFunction: 
    # parameters (speaker) 
    questEditorId: Cuddle_Main
    scriptName: Cuddle_Actions
    executionFunctionName: Stop_Cuddle
  tags: 
    - not_busy
    - can_use_hands
  description: start cuddling with [target]
  parameterMapping:
    - type: speaker
      name: akActor
      description: Speaker Actor parameter (triggering entity)
    - type: dynamic
      name: target
      description: partner
  eligibilityRules:
    - conditions:
      - variable: "cuddle.is_ready"
        comparisonOperator: ==
        expectedValue: true
  
type: action_set 
category: cuddling
actions:
  # parameters (speaker, target, type)
  - name: Cuddle_Standing
    description: "Standing, facing each other, and hugging."
    parameterMapping:
      - type: static
        name: type
        value: standing
  # parameters (speaker, target, type, position)
  - name: Cuddle_laying
    description: "laying on the ground, [position]."
    parameterMapping:
      - type: static
        name: type
        value: laying
      - type: dynamic
        name: position 
        value: "{{cuddle.laying.options}}"
  .....
```
```