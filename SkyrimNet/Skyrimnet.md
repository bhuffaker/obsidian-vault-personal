#skyrim #minai 
I have an idea for a backchannel that would allow the LLM to express to SkyrimNet information it has decided are true. **Triggers** take a description of the trigger and  callback function. The trigger will be unregistered after it has fired number_times. If ongoing is true, the trigger never auto unregisters. All true triggers are fired. There would be a matching unregisterTrigger(id) to remove a trigger. 
```
skynetAPI.registerTrigger(id="Dom_Slave_Refused_Sex", 
       description="When a requester asks or orders a DOM_Slave to preform any kind of sexual act and the DOM_Slave refuses.",
       parameters='{"requester":"Actor", "dom_slave":"Actor"}'
       ongoing=true,
       number_times=0,
       executionScriptName="DOM_SkyrimNet_Library",
       executionFunctionName="RefusedSex")
       
skynetAPI.registerTrigger(id="NinaDebtProblem_PlayerKnows", 
       description="When {{ player.name }} has learned both that Nina has a debt problem and the amount of that debt.",
       parameters='{"amount":"Int"}',
       ongoing=false,
       number_times=1,
       executionScriptName="NinaDebtProblem_Library",
       executionFunctionName="PlayerKnowsProblem")
```


