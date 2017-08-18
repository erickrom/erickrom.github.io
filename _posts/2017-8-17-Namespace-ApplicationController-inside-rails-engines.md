---
layout: post
title: Namespace ApplicationController inside Rails Engines
---

Recently I ran into an issue with one of our Rails Engines at work.
This engine, which we'll call *History* was mounted on our main app and
all of the sudden trying to access the engine's routes would 
return a strange 'NoMethodError' when visiting any of the
controllers' routes, with a strange stacktrace that pointed to a line
in the **ApplicationController**.

To our surprise, the engine's repo had not been modified recently,
so it was weird that all of the sudden it stopped failing.

I decided to look into the 'History' repo and inspect the **ApplicationController**
inside the engine.  I found out that the *HistoryController* had the following line:

```class HistoryController < ApplicationController```

I realized that the error was happening because the *HistoryController* was
inheriting from *ApplicationController*, but it was being overwritten by the
host application's *ApplicationController*, hence the **NoMethodError**

The fix was to Namespace ApplicationController inside the Engine:

```class HistoryController < History::ApplicationController```
 
I also moved the application_controller.rb from the main app/controllers
directory to the corresponding path, at app/controllers/history.

