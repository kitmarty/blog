---
layout: post
title:  "ARIS Database Performance Test: How To Work With Huge Number of Objects"
date:   2019-07-06 12:32:48 +0300
categories: ARIS Reports
---
So, it's my first post here and I'd like to introduce myself. My name is Nikita Martyanov and I've been working as an ARIS Developer / ARIS Products Specialist for more than 8 years. [Here][here] you'll find my contacts, resumes and other information.

Today I want to tell you about different modes of ARIS database working. It's a very common task to upload to ARIS a lot of new data / renew data on schedule and so on. Sometimes we need to create tons of occurrences, definitions, models, groups. And if we work in standard mode we'll start to experience performance troubles. But ARIS has a built-in solution.

First of all, ARIS has three database modes (object `Constants`):
{% highlight Java %}
Constants.SAVE_AUTO //default mode
Constants.SAVE_ON_DEMAND
Constants.SAVE_IMMEDIATELY
{% endhighlight %}

To change the mode we can use `ArisData` object:
```java
ArisData.Save(Constants.SAVE_ON_DEMAND)
```

Some words about ARIS Database modes. The default mode is `SAVE_AUTO` as it's said in ARIS Script Help. `SAVE_ON_DEMAND` mode let us to accumulate changes somewhere in the memory and don't apply them to a database until `SAVE_NOW` is called. And `SAVE_IMMEDIATELY` mode (the slowest) applies changes at once.

To prove there's a big difference between those modes I created the test ARIS report and performed an experiment.
I think that an absolute result may vary, it depends on the computer performance, type of DBMS and other factors. Nevertheless we can get relative results on the same machine.

This test contains five tasks:
- create 1000 groups inside the root group of a database;
- create 1000 models inside the root group of a database;
- create 1000 object definitions inside the root group of a database;
- create 1000 object occurrences on a random model;
- clear a database (delete all created items);

It's not a good idea to post here full code, so you'll find it in the [Github Repository][test_code].

I ran each test three times and below in the table the average execution time is presented:

| DB Mode | 1000 groups | 1000 models | 1000 definitions | 1000 occurrences | Clear a database | Total time | Total time change |
|---|---|---|---|---|---|---|---|
|<span style="font-weight:bold">SAVE_AUTO</span>|00:00:42,848|00:00:00,733|00:00:00,322|00:01:35,659|00:01:42,450|00:04:02,013| <span style="color:blue">100%</span> |
|<span style="font-weight:bold">SAVE_ON_DEMAND</span>|00:00:00,536|00:00:01,394|00:00:00,119|00:00:38,568|00:00:39,868|00:01:25,031| <span style="color:green">~-65%</span> |
|<span style="font-weight:bold">SAVE_IMMEDIATELY</span>|00:00:30,684|00:00:37,403|00:00:40,661|00:01:32,435|00:03:57,794|00:07:18,978| <span style="color:red">~+81%</span> |

*there can be some inaccuracies in the time data because of the mode difference and their approaches of working with DB. Nevertheless total time is accurate.*

As you can see execution time decreases by <span style="color:green; font-weight:bold">65%</span> percent when we use `SAVE_ON_DEMAND` mode. And this result we got on an empty database. I suppose it will be much more effective, when a database has a lot of objects inside.

And time extremely increases by <span style="color:red; font-weight:bold">81%</span> when we use `SAVE_IMMEDIATELY` mode. This mode is effective only if we want to write changes in a database right after the execution of a particular command in the script.

Hope that the information above will be useful for you.

[here]: https://kitmarty.github.io/
[test_code]: https://github.com/kitmarty/ARIS-Database-Performance-Test/blob/master/ARIS%20Database%20Performance%20Test.js
