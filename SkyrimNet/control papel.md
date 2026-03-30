GIven you have a clean powerful UI, I would prefer to have the skyrimNet mods register their control panels with SkyrimNet. Register a inja file as a mod's control panel. Register call back functions to handle the panel's interactions. Then use decorators to pull information into the control panel page. SkyrimNet UI would have a Mod ControlPanels page and a single page, for now, for each registered mod.
```
SkyrimNetAPI.RegisterControlPanel(String inja_filename)
```