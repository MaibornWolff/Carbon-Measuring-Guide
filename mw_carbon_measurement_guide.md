# MaibornWolff Green Software Carbon Measurement Recommendations Guide

## Table of Contents

- [Introduction](#Introduction)
- [Glossary](#Glossary)
- [Software Carbon Intensity](#software-carbon-intensity)
  - [Why we mostly use SCI, and when to use GHG](#why-we-mostly-use-sci-and-when-to-use-ghg)
  - [MW SCI Usage](#mw-sci-usage)
    - [Our Functional Units](#our-functional-units)
        - [How to choose your functional unit](#how-to-choose-your-functional-unit)
- [What to measure](#what-to-measure)
  - [What we include, exclude and why](#what-we-include-exclude-and-why)
- [Our Practical Measurement and Calculation Approach](#our-practical-measurement-and-calculation-approach)
    - [Cluster carbon measurement/estimation](#cluster-carbon-measurementestimation)
    - [Website emissions and efficiency](#website-emissions-and-efficiency)
    - [Emissions produced by external systems](#emissions-produced-by-external-systems)
    - [The actual MW SCI calculation](#the-actual-mw-sci-calculation)
- [The authors](#the-authors)

### Introduction

Welcome to the MaibornWolff green software carbon measurement recommendations guide. This guide will give you a standard template
for how to set up an effective monitoring structure for the carbon emissions the software you are creating is producing. 

It is tailored towards projects we commonly develop, that are hosted in the cloud and use technologies common in our tech-stack.
Things like desktop applications and unusual edge cases are not discussed for now, but might be added in the future if there is demand for it.

It will also provide you with some understanding of the subject of green software in general. It is not a guide focused on carbon optimisation,
for that take a look [here](TODO TRi add link to other guide). However, there is some overlap by default, which will at times be discussed here as well. 

There is deeper knowledge on the topic available as well, which can be found in the documents we link to when discussing the relevant topics here.
This is for the interested and not strictly required in order to practically apply our green software development approach in your projects. 
For any questions and feedback, contact the members of the [MaibornWolff Green in IT project](#the-authors).

### Glossary

| Term                  | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CO2eq / CO2-eq / CO2e | Carbon dioxide equivalent. This unit of measurement indicates the potential impact of CO2 and non-CO2 gases on global warming in carbon terms.                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| gCO2eq/kWh            | Grams of carbon equivalent per kilowatt hour. The standard unit of carbon intensity is gCO2eq/kWh, or grams of carbon per kilowatt hour.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| SCI                   | Software Carbon Intensity. A standard which gives an actionable approach to software designers, developers and operations to measure the carbon impacts of their systems.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Carbon intensity      | Carbon intensity measures how much carbon (CO2e) is emitted per kilowatt-hour (KWh) of electricity consumed. The standard unit of carbon intensity is gCO2eq/kWh, or grams of carbon per kilowatt hour.                                                                                                                                                                                                                                                                                                                                                                                                            |
| Carbon awareness      | Term to describe information about the differing levels of carbon intensity at differing places and times                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Net zero              | Net zero means reducing emissions according to the latest climate science and balancing remaining residual emissions through carbon removals (neutralizations). Net zero, by definition, requires emissions reductions in line with a 1.5°C pathway. All businesses must do this to achieve net-zero global emissions by 2050. <br />The critical differentiator between net zero and carbon neutral is net zero's focus on abatement rather than neutralizations and compensations. A net-zero target aims to eliminate emissions and only to use offsetting for the residual emissions that you cannot eliminate |
| GSF                   | [Green Software Foundation](https://greensoftware.foundation/manifesto)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| GHG protocol          | [Greenhouse Gas protocol](https://ghgprotocol.org/) - the most commonly-used method for organizations to measure their total carbon emissions                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RAPL                  | Running Average Power Limit is a power estimation feature in modern x86 CPUs from Intel and AMD. n the green software community it is extensively used in order to get accurate energy measurements for the CPU and DRAM components.                                                                                                                                                                                                                                                                                                                                                                               |

### Software Carbon Intensity

Software Carbon Intensity (SCI) is a methodology developed to score a software application along a dimension of sustainability and to encourage action towards eliminating emissions. 
If you are interested in more than simply what you need to know for its application in a project, you can take a look either at our more detailed exploration [here](/deeper_foundations/overview_guide_measurement_practices.md#software-carbon-intensity)
or take a look at the [definition](https://grnsft.org/SCI) provided by its creators, the Standards Working Group of the Green Software Foundation.
Instead, we will discuss why we use it over a score of total carbon emissions (GHG) most of the time and how we use it in our projects.

#### Why we mostly use SCI, and when to use GHG

If you are interested in a deeper look into the pros and contras of SCI and GHG look [here](/deeper_foundations/overview_guide_measurement_practices.md#quantify).
In short, we generally prefer SCI to GHG because it sets the right incentives for improving carbon efficiency over time 
and does not penalise up-front investments into carbon cost saving measures. We can also observe changes in our efficiency
more easily when we use SCI, both positive and negative. It also will not penalise growing the user base if the functional unit is chosen correctly.

If you are working on a project that requires a very large upfront carbon investment, such as training a machine learning model, SCI might not suffice.
In that case, please contact the [MaibornWolff Green in IT project](#the-authors) for advice.

#### MW SCI Usage

Let's first take a look at the SCI formula, and then explain what each component normally entails in a software project
```
SCI = ((E * I) + M) per R
```

In this formula

- E = Energy consumed by a software system
- I = Location-based marginal carbon emissions
- M = Embodied emissions of a software system
- R = Functional unit (e.g. carbon per additional user, API-call, ML job, etc)

We will first have a look at what are [possible functional units (R)](#our-functional-units) and [how to choose them](#how-to-choose-your-functional-unit).

Then we discuss what tools we like to use to [measure](#our-practical-measurement-and-calculation-approach) the energy consumed by a software system (E) 
and [what should be in- and excluded for this](#what-to-measure),
what [tools to use to convert the measured energy into gCO2eq](#electricity-maps-paragraph) based on the location-based marginal carbon emissions (I),
and lastly how to [measure the embodied emissions of a software system (M)](#embodied-emissions-paragraph) based on what must be included and what can be excluded for this.

##### Our Functional Units

Deciding which functional unit (R) is right for the project will always be a case by case decision. But we can provide a step-by-step guide that will make the decision much easier.

What are our recommended functional units and when should you choose one or the other:

- API calls
- ((daily/weekly/monthly) active) users
- Registered users/devices
- Visits by users
- Seconds/minutes spent in/on the application/website
- Per second/minute/.../day of application runtime
- Data volume streamed to the user
- Data volume/calculations processed by the application
- Number of models/locations/etc. for which different calculations are required
- Any combination/fraction of the above-mentioned

###### How to choose your functional unit

For the choosing of the functional unit, we offer a quick and easy way [here](#the-heuristic-way) that will work 
in most cases and does not require an in depth understanding of the subject material. This will usually be all that is required 
and if this is the case in your project, you can scip the section titled ["A more in depth look"](#a-more-in-depth-look).
But if you want to gain a better understanding or are faced with a more complex situation, give it a look.

###### The heuristic way

For ease of use with customers, we created a handy flow chart. It can be used together with a customer to have 
a structure that you can lead them through, or you can guide them through these questions another way.

<img alt="Our easy find your R flow chart" height="1000" src="/diagrams/simple_functional_unit_mermaid-diagram.svg" width="2600"/>

In addition to this, you should go through the following checks afterward and from time to time during the software lifecycle, to make sure the chosen R does make sense:

- If I apply this R, can it happen that by being more successful(more users sign up/devises register, more sales happen, etc.) I get a worse SCI? The answer should be NO!
- If more users/devices/etc. become inactive but stay registered, can this decrease my SCI without me getting more efficient? The answer should be NO!
- Does my SCI fluctuate without my software changing in relevant ways? The answer should be NO! Check this from time to time once the software is live and reassess if necessary
- Do we have good metrics for this functional unit? Can we e.g. count how many API calls we had and when? The answer MUST be YES!

###### A more in depth look

This section is not necessary if you could find what you needed through the one above. It provides a deeper look with examples 
that you can read if you are having trouble with the one above or want a deeper understanding of the subject matter.

The first step is, of course, to eliminate those units that don't make sense for the system. 
E.g. if you have a system that receives requests from users that it answers with simple results consisting of only KBites worth of text.
Streamed data volume would make no sense here. It could be that your application performs quite demanding calculations for some answers, 
and not for others. That might make API calls also a bad R. Why?

Because the questions we must ask ourselves first when choosing a functional unit is: 
**Is each instance of this R at least somewhat similar to its other instances? 
If not, is their difference in occurrence at least somewhat stable?**

What do we mean by this? Well, if our API offers a number of calculations that all differ wildly in how much 
computing power their answer requires, and they are not called with the same level of volume, 
even statistically normalised oer time, our results might not be meaningful.
```
Let's say we have a weather app. Calls about the current weather in a place require almost no effort from our system and predictions for specific places require a lot. 
The ratio of calls between the two varies from 80/20 to 20/80. 
Our SCI will experience significant fluctuations in response to the ratio of predictions calls to current weather calls, without any action on our side. 
If the ratio follows predictable patterns, it can be statistically normalised, but if not, it makes the SCI inadequate at best and misleading at worst.
```

Second we must ask ourselves: **Is this the unit that scales with the use of my application? Would growth of the business be impeded/punished by this unit?**

Our R should reflect what the driver of our emissions is. If all other things stay the same, an increase or decrease in Rs should not affect our SCI.
E.g. if we choose registered users as our R, a growing user base should not affect our SCI, even though it might affect our total emissions increase. Let's also look at an example where total registered users would be a bad R because of this.
```
Let's again assume we are a weather forecasting service. But in this example, we make one large costly prediction calculation every day and cache it. Every time a user calls our API, the easy and cheap process of simply sending them that cached result is executed. 
Only a negligible fraction of our total carbon costs is produced by handling the API calls. In this case using a functional unit like API calls or registered/daily users will lead to the following, undesirable situation: 
As more users join, the daily carbon cost gets divided by a growing number of users/calls, **leading to our SCI falling without our software getting more efficient**. In this case, e.g. per day of application runtime would be a better R. 
On the other hand, per day of application runtime would be a poor R for any application that scales with an increase in users/calls. In such cases, an increase in users or their activity would increase the SCI without our application becoming less efficient. So, either the number of API calls or registered/active users would be a better R.
```

If we can't find any R that meets our criteria so far, we might have to ask ourselves a third question, namely: **Is one R is even enough for the project or are we dealing with a system so complex, we have to split it into different analytical units, each with their own R?**

You should never split one service that runs within one kubernetes pod into 2 or more analytical units, as this would make it technically too difficult to measure. So the minimum size of one analytical unit is one service. But there is no maximum size.
The project might be building a mirco service architecture with 17 different services, and it might still be the right decision to view it as one unit. It in fact is expected that cases when we have to split will be much rarer than those where we won't.
So when should you split? See how many of the following points apply:

1. The system performs services that are mostly unrelated to each other and of vastly differing usage levels. The reason why they are within the same architecture might be things like needing to access the same database or having other technical dependencies, but they perform very different functions and are used/called by different entities.
2. Due to the differing nature of the services, the well fitting functional units for one or more services just don't fit with one or more of the other services.
3. The usage levels of the services are so far apart that one would always overshadow the other, obscuring any improvements made in the efficiency of the latter, creating no incentive to improve it

```
We will now, for an example, take a look at the Miele cloud project, and decide what we should do based on our recommendations. It has 
- 4 different Voice services, targeted at 3 different voice smart-home assistants from different companies (Amazon: voice-smarthome & mcs-alexa, Google: mcs-google-assistant and AliGini: mcs-aligini-assistant), 
- one service that helps with reporting the states of all connected devices to the various end user services, registering new devices, and communicating with the devices themselves (mcs-service), 
- two apps developed by Miele itself but using the services developed by MW (Miele Smarthome & Miele RX-Scout),
- one basically independent analytics service that is not embedded, but separate from the other services (mcs-analytics)
- as well as various services that help with such things as authentication or directing requests to the correct geographical region and development stage namespace

These services are also partly developed by different teams. They have massively different usage rates and perform different tasks. 
Some of the services constantly read from a Kafka topic that updates the states of connected devices, others don't. 
Only a fraction of the users use the voice services, and the different voice services have strongly differing usage levels. 

If we choose registered devices as our R for the whole system, we would not make a terrible decision, but would have to contend with the following problems: 
1. The voice services that do not read from the Kafka topic for state updates, no longer play a significant role in our SCI, meaning their development teams have no incentive to make them more efficient.
2. Clean-up of the database is now disincentivised, as entries of inactive devices will bring down the SCI, as they no longer create change events.
3. Adding device types that have fewer state changes causes changes in our SCI that do not reflect any improvements in the software.

If we choose API calls as our R - including writing to/reading from a Kafka topic as such - we would also not make a terrible decision, but would have to contend with the following problems: 
1. Some services, like analytics services make very few API calls, but require a lot of computing power anyway, and their consumption is mostly not based on how many calls they get. 
   This means that if the number of overall API calls to the system increases, the SCI gets better as the contribution of these services is devided by a larger number of calls
2. It creates no incentive to reduce unnecessary call/topic reads. In 2023 a large abount of RAM and CPU usage was saved by the voice-samrthome service by simply no longer processing 
   state change information that had no relevance to the service. This lowered its carbon output, but with API calls as an R, it would have not lowered its SCI score.
   
A good solution here might be to have an SCI with a fittingly chosen R for each group of applications made by each each development team. So fo the 3 voice teams the R might be processed requests, 
but with each service seen seperatly, for analytics connected devices and for the mcs-service API calls. 
If the project menagement insists upon one R for the whole system landsacape we should probably choose a definition of API calls that includes all reads/writes from/to Kafka topics as the least bad option.
```


### What to measure

For a deeper discussion of the following topic, take a look at the "[what can be measured and what should we measure](/deeper_foundations/overview_guide_measurement_practices.md#what-can-be-measured-and-what-should-we-measure)" section
of our [overview guide for measurement practices](/deeper_foundations/overview_guide_measurement_practices.md). Here are the important takeaways:

Our first priority should be to measure electricity usage.
It alone does not enable us to calculate all the carbon we create, but it is the most important building block in doing so.

Hardware usage is the second very important thing we can measure with relative ease. Even if we do not know the exact amount of embodied carbon in each
piece of equipment or its expected lifespan, maximising usage and minimising idle time caused by us is always the correct direction to take,
even if in some cases we might not be able to calculate how much carbon we save.

In cases where we relay solely on the SCI, we will just measure the electricity usage and hardware utilisation of our pods running in our k8s cluster.
In cases where we also need our total GHG score - [see here for when that is](#why-we-mostly-use-sci-and-when-to-use-ghg) - we will need to
also measure several other components, including during our development process.

We also of course need certain basic analytics about our application, specifically those that comprise our functional unit. 
If e.g. our functional unit is active users, we need to be able to determine how many users we have and how many are active. 

#### What we include, exclude and why

What we include in both our measurements and embodied carbon measurements is largely based on what can be measured with what level of effort
and how important it is to set the right incentives to improve the carbon efficiency of the software.

First lets get the easy decisions out of the way:

- We definitely NEED to measure all electricity used by all systems we have control over and convert that into gCO2eq.
- We definitely SHOULD [in some way](#emissions-produced-by-external-systems) include electricity used by systems we make calls to, that are not under our control, as otherwise we would create an incentive to simply outsource carbon intensive calculations
- We definitely SHOULD NOT include carbon costs that were incurred when things like the fundamental infrastructure the internet runs on was created, the laptops we develop on were built, and other things that are very difficult to factor in

Now with the easy decisions out of the way, we turn to the system boundaries and embodied emissions that are harder to define. 
Generally for a common project, hosted on a cluster and with some sort of front end, 
our system boundary will be the cluster and the browser/user interface. 
In the following sections we explore how we actually measure the emissions produced by each 
and also what to do about emissions produced by systems we call.

### Our Practical Measurement and Calculation Approach

#### Cluster carbon measurement/estimation

The carbon footprint of your cluster will be decisive in many projects.

Our first standard recommendation is the [Cloud-Carbon-Footprint-Tool](https://www.cloudcarbonfootprint.org/docs/methodology) by Thoughtworks.

Currently, we only have a recommendation and [step-by-step](/setup_guides/cloud-carbon-footprint.md) guide for Azure hosted projects,
but the recommended tool also works for AWS and Google cloud, it even has [dedicated ways to deploy](https://www.cloudcarbonfootprint.org/docs/deploying) on those services, unlike for Azure.

Note some important things:
- Like all available tools, the CCF does only provide an estimation
- It's accuracy will increase if you give it access to an [electricity maps](https://www.electricitymaps.com) account. While the CCF is free and open source, electricity maps is a paid service
- You need to give it at least "Reader" level access to you Azure subscription.
- It has some limitations to its granularity. It will measure the energy usage of you whole k8s cluster, not pod by pod TODO check if there really isn't a way to do this

So why the CCF? In short, it is easy to set up, gives you not only a good CO2eq estimation,
but also your electricity usage and estimated cost, and works with all 3 big cloud providers.
Its [methodology for CO2eq estimation](https://www.cloudcarbonfootprint.org/docs/methodology) is sound and finds approval in the Green IT community.
It is also a free open source project, as long as you are fine with the lower accuracy it has without the electricity maps account.
This makes it easier to pitch to potential customers, and once they are happy with the results, you can always upgrade and
buy an electricity maps account.

<a name="electricity-maps-paragraph"></a>
TODO TRi talk about potential MW EM account with group and then potentially edit here

<a name="embodied-emissions-paragraph"></a>
TODO TRi talk about embodied emissions here

We recommend that you talk to your PO or potential customer and show them what the CCF can provide, by showing them the
[dashboard of the Green-in-IT](http://mwgiitccf.westeurope.cloudapp.azure.com) project. If more convincing is needed,
follow our [step-by-step](/setup_guides/cloud-carbon-footprint.md#setup-and-steps-used-for-testingdeploying-in-a-virtual-machine) guide to quickly set up an instance in a virtual machine.
If they are sold, [integrate it in your existing DevOps environment](/setup_guides/cloud-carbon-footprint.md#setup-and-deployment-to-your-kubernetes-cluster).

#### Website emissions and efficiency

Website emissions analysis will by its nature also venture into efficiency optimization. As such, this section goes beyond just measuring.
We primarily use three tools, [Ecograder](https://ecograder.com/), [Beacon](https://digitalbeacon.co/) and [Google Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) 
that each have their place at different times in the development lifecycle. We recommend you proceed something like the following:

##### During development

Use Lighthouse constantly during development. Beacon and Ecograder need publicly available websites, 
so you can't use them when you test locally/on your DEV-environment. Lighthouse will not give you a CO2eq calculation, 
but you can point you towards design inefficiencies that you can avoid, thereby becoming more CO2eq-efficient as well.

##### After every PROD-deployment

Use both Beacon and Ecograder on your website after each PROD-deployment. 

Beacon because it gives you a CO2eq score not only for your first visit, but also for subsequent visits. 
Subsequent visits are usually more efficient because of Cookies, so you need a score for both 
to be able to create representative calculations. Note each score like you might 
the results of a linting tool after a sprint change. Assuming you have analytics about website visits, 
you can now calculate an approximate value for the CO2eq usage of this part of the system. 

Use Ecograder because it not only gives you a score, but more so because it points you to the largest avoidable sources 
of your CO2eq output. Ideally there would be a story after every PROD Deployment dedicated to reducing these based on the results.

#### Emissions produced by external systems

In this case external systems mean systems that are mont only not on our cluster, but fully outside our control. 
Otherwise, we could measure their carbon output like described above. Luckily, we do not need an exact score in order to 
preserve an incentive structure that pushes us to be more efficient over time. We can instead set a few rules and heuristics 
to help us with that. 

1. If you can find the CO2eq or KW/h cost of the external process use that. Take as an example a ChatGPT query, which according to [this analysis](https://piktochart.com/blog/carbon-footprint-of-chatgpt/), produces approximately 4.32 grams of CO2. Sadly you often won't be able to such a value.
2. If you extract something from your system and replace it with a call to an external system, always assume that it is at least as costly as when your system was doing it, unless you have proof to the contrary. Take the reduction of KW/h needed by your system following the extraction of the process, divided by its frequency as your assumed cost of the outsourced process.
3. If the external process was external from the beginning, and you could not find out its CO2eq or KW/h cost, try to find one for a similar process and use it as a proxy. If you can find more than one similar process and its costs, use the worst one, unless there is reason to believe it is an outlier.
4. If you can't find a value for it or something similar, and it wasn't extracted from your system, take an average for your other external call of which you do have a value.
5. If heuristics 1-4 can't be applied, we muss accept it and exclude it from our system boundary and explain why we did so in when presenting our SCI.

#### The actual MW SCI calculation

Now that you have chosen a functional unit (R), are measuring the CO2eq output of your cluster, have a sample for your website, values for calls to external 
systems and analytics for your application that tell you quantity of your functional unit (over a period of time) you can calculate 
your SCI. You can automate this, but doing it once every sprint change or PROD-deployment is enough. While choosing your functional 
unit you will have also chosen a timeframe over which you want to average your energy consumption. Even if that timeframe is much 
larger than the frequency of you SCI calculation, you can still do it just the same. 

We will now go step by step along the manual path to calculating the SCI. Currently, we do not offer a template to automate 
this, but might do so in the future. If you have done this in your project, please contact us. 

Download the CSV from your Cloud Carbon Footprint Tool deployment. Use it as your basis and add the samples from your Website, 
one for first time visits, one for repeat visits in another column and do the same for calls to external systems. 
Average them for the timeframe you have chosen to make the results meaningful. As long as your analytics data can provide 
you with this data on a daily/hourly/etc. basis, this does not have to be the same timeframe over which you analyse the SCI. 
You could e.g. average them daily and still analyse how your SCI changes over a month. Add your Rs for the analysed timeframe 
from your analytics (e.g. API calls). Device the CO2eq average over your meaningful timeframe by Rs over that timeframe, and 
you have your SCI over time. Let's look at an example:

```
You have data from you cluster over a month and have produced 0,2 metric tons of CO2eq. 
Your website creates crates 4g of CO2eq per first time page load and 0,5g per load for repete visitors. 
You had 1 000 000 visits this month. 200 000 first, 800 000 repet.
You at times call to a an OpenAI based chatbot if customers on the website need support. It produces 4g of CO2eq per call. Over this month they did this 5000 times.
You avearge data over a day to make reduce the noise and make them meaningful. 
Your R is visits.
You deployed an update on the 15th which you hoped will improve your performance. After it, your page load metrics change to 3,9g and 0,48g respectively.

On the first of the month you had created 8,5kg of CO2eq from your cluster, you had 40 000 page visits, 8 000 firsts, 32 000 repete and you 200 calls to the chatbot. 
It follows, your SCI on the first of this month was 1,4325g CO2eq per visit. 

On the 16th of the month you had created 3,5kg of CO2eq from your cluster, you had 20 000 page visits, 4 000 firsts, 16 000 repete and you 100 calls to the chatbot.
It follows, your SCI on the first of this month was 1,359g CO2eq per visit. 

You would of course do this in Excel for every day of the timespan and create a graph based on it, that shows your SCI day by day. 
However, based on these two, simplified data points your update has indeed made your application more effcient. 
```

As you can see in the example, a lower or higher number of visits does not affect our SCI, but changes in efficiency do. 
A change in user behavior, such as users starting to use the chatbot more, would though. This is why we have to average results 
over a meaningful timeframe and also be ready to reassess our chosen R if we observe large, unforeseen changes. 

```
Now lets assume that we over time observe that users tend to first visit at the beginning of a calendar month, and use the chatbot a lot at that time as well.
This would mean that if we continue to average our usage over a day, we would always seem to get better over the course of a month and then get worse again at the beginning of the next one.
In such a case we should start averaging not over a day, but a month to see more meaningful results over time. 
```

As long as this section might have been, by now you hopefully see that calculating SCI scores is not complicated if you 
have the necessary metrics at hand. We hope to see you again in our [carbon optimisation guide](TODO TRi link to guide)

### The authors

This guide is a result of the MaibornWolff Green in IT project and was authored by in alphabetical order:

Antonio Adrian antonio.adrian@maibornwolff.de

Jochen Joswig jochen.joswig@maibornwolff.de

Tobias Rimmele tobias.rimmele@maibornwolff.de
