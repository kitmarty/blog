---
layout: post
title:  "ARIS Database Modes For Model Changes"
date:   2019-11-09 15:32:48 +0300
categories: ARIS Reports
---
To continue the [previos post][prev_post] about ARIS Database modes I'd like to show another way of ARIS Database modes usage.

Sometimes when we use ARIS we need to display (print as report result) part of the model, for example, without particular symbol types or connections. The common way is to create new model (using ARIS Report), create copies of the occurrences from the source model, delete symbols we don't need and print the model. After these actions we usually delete this temporary model.

However there's another way. We can switch to `SAVE_ONDEMAND` mode, make necessary changes, print model as output and don't save anything to database. The fact is the changes we make when we use `SAVE_ONDEMAND` are available in the current session only. And if we don't call `SAVE_NOW`, all the changes we made before will be canceled. But the changes we make are available in the current session and if change something in this mode we can read it and get changed value. You can check it for example by changing attribute and reading it in the debug mode.

I created demo database that contains one model only. And wrote simple ARIS Report to demonstrate the result. Here [you can get it][get_it].

Hope you'll find this material useful.

---
Best Regards,
Nikita Martyanov

[prev_post]: https://kitmarty.github.io/blog/aris/reports/2019/07/06/aris-database-performance-test.html
[get_it]: https://github.com/kitmarty/ARIS-Model-In-Memory-Changes
