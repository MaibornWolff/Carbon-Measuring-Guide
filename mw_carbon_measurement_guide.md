# MaibornWolff Green Software Carbon Measurement Recommendations Guide

## Table of Contents

- [Glossary](#Glossary)
- [What to measure](#what-to-measure)
- [MW SCI Definition](#mw-sci-definition)
- [Why we mostly use SCi, and when to use GHG](#why-we-mostly-use-sci-and-when-to-use-ghg)
- [Our Functional Units](#our-functional-units)
    - [How to choose your functional unit](#how-to-choose-your-functional-unit)
- [What we include, exclude and why](#what-we-include-exclude-and-why)
- [Our Measurement and Calculation Approach](#our-measurement-and-calculation-approach)
    - [Cluster carbon measurement/estimation](#cluster-carbon-measurementestimation)
    - [Website efficiency](#website-efficiency)

### Glossary

| Term                  | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CO2eq / CO2-eq / CO2e | Carbon dioxide equivalent. This unit of measurement indicates the potential impact of CO2 and non-CO2 gases on global warming in carbon terms.                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| gCO2eq/kWh            | Grams of carbon per kilowatt hour. The standard unit of carbon intensity is gCO2eq/kWh, or grams of carbon per kilowatt hour.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| SCI                   | Software Carbon Intensity. A standard which gives an actionable approach to software designers, developers and operations to measure the carbon impacts of their systems.                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Carbon intensity      | Carbon intensity measures how much carbon (CO2e) is emitted per kilowatt-hour (KWh) of electricity consumed. The standard unit of carbon intensity is gCO2eq/kWh, or grams of carbon per kilowatt hour.                                                                                                                                                                                                                                                                                                                                                                                                            |
| Carbon awareness      | Term to describe information about the differing levels of carbon intensity at differing places and times                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Net zero              | Net zero means reducing emissions according to the latest climate science and balancing remaining residual emissions through carbon removals (neutralizations). Net zero, by definition, requires emissions reductions in line with a 1.5°C pathway. All businesses must do this to achieve net-zero global emissions by 2050. <br />The critical differentiator between net zero and carbon neutral is net zero's focus on abatement rather than neutralizations and compensations. A net-zero target aims to eliminate emissions and only to use offsetting for the residual emissions that you cannot eliminate |
| GSF                   | [Green Software Foundation](https://greensoftware.foundation/manifesto)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| GHG protocol          | [Greenhouse Gas protocol](https://ghgprotocol.org/) - the most commonly-used method for organizations to measure their total carbon emissions                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RAPL                  | Running Average Power Limit is a power estimation feature in modern x86 CPUs from Intel and AMD. n the green software community it is extensively used in order to get accurate energy measurements for the CPU and DRAM components.                                                                                                                                                                                                                                                                                                                                                                               |

### What to measure

As discussed in the "[what can be measured and what should we measure](../analysis/overview_guide_measurement_practices.md#what-can-be-measured-and-what-should-we-measure)" section
of our [overview guide for measurement practices](../analysis/overview_guide_measurement_practices.md) our first priority should be to measure electricity usage.
It alone does not enable us to calculate all the carbon we create, but it is the most important building block in doing so.

Hardware usage is the second very important thing we can measure with relative ease. Even if we do not know the exact amount of embodied carbon in each
piece of equipment or its expected lifespan, maximising usage and minimising idle time caused by us is always the correct direction to take,
even if in some cases we might not be able to calculate how much carbon we save.

In cases where we relay solely on the SCI, we will just measure the electricity usage and hardware utilisation of our pods running in our k8s cluster.
In cases where we also need our total GHG score - [see here for when that is](#why-we-mostly-use-sci-and-when-to-use-ghg) - we will need to
also measure several other components, including during our development process.

### MW SCI Definition

The general definition of SCI was already discussed [here](../analysis/overview_guide_measurement_practices.md#software-carbon-intensity), so we won't repeat it again.
Instead, we will discuss why we use it over GHG most of the time and what to do with the individual factors in its formula:

```
SCI = ((E * I) + M) per R
```
For this we will have a look how to choose the functional unit (R), how to measure the energy consumed by a software system (E) and what should be in- and excluded for this,
what tools to use to convert the measured energy into gCO2eq based on the location-based marginal carbon emissions (I),
and lastly how to measure the embodied emissions of a software system (M) based on what must be included and what can be excluded for this.

#### Why we mostly use SCI, and when to use GHG

As already partially discussed [here](../analysis/overview_guide_measurement_practices.md#quantify) we generally prefer SCI over GHG because it sets the right incentives
for improving efficiency over time and does not penalise up-front investments into carbon cost saving measures. We can also observe changes in our efficiency
more easily when we use SCI, both positive and negative. It also will not penalise growing the user base if the functional unit is chosen correctly.

#### Our Functional Units

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
- TODO

##### How to choose your functional unit

TODO TRi cut this down and integrate picture

The first step is, of course, to eliminate those units that don't make sense for the system. E.g. if you have a system that receives requests from users that it answers with simple results consisting of only KBites worth of text.
Streamed data volume would make no sense here. It could be that your application performs quite demanding calculations for some answers, and not for others. That might make API calls also a bad R. Why?

Because the questions we must ask ourselves first when choosing a functional unit is: **Is each instance of this R at least somewhat similar to its other instances? If not, is their difference in occurrence at least somewhat stable?**

What do we mean by this? Well, if our API offers a number of calculations that all differ wildly in how much computing power their answer requires,
and they are not called with the same level of volume, our results might not be meaningful.
```
Let's say we have a weather app. Calls about the current weather in a place require almost no effort from our system and predictions for specific places require a lot. The ratio of calls between the two varies from 80/20 to 20/80. 
Our SCI will experience significant fluctuations in response to the ratio of predictions calls to current weather calls, without any action on our side. 
If the ratio follows predictable patterns, it can be statistically normalised, but if not, it makes the SCI inadequate at best and misleading at worst.
```

Second we must ask ourselves: **Is this the unit that scales with the use of my application? Would growth of the business be impeded/punished by this unit?**

Our R should reflect what the driver of our emissions is. If all other things stay the same, an increase or decrease in Rs should not affect our SCI.
E.g. if we choose registered users as our R, a growing user base should not affect our SCI, even though it might affect our total emissions increase. Let's also look at an example where total registered users would be a bad R because of this.
```
Let's again assume we are a weather forecasting service. But in this example, we make one large costly prediction calculation every day and cache it. Every time a user calls our API, the easy and cheap process of simply sending them that cached result is executed. 
Only a negligible fraction of our total carbon costs is produced by handling the API calls. In this case using a functional unit like API calls or registered/daily users will lead to the following, undesirable situation: 
As more users join, the daily carbon cost gets divided by a growing number of users/calls, **leading to our SCi falling without our software getting more efficient**. In this case, e.g. per day of application runtime would be a better R. 
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
   This means that if the number of overall API calls to the system increases, the SCi gets better as the contribution of these services is devided by a larger number of calls
2. It creates no incentive to reduce unnecessary call/topic reads. In 2023 a large abount of RAM and CPU usage was saved by the voice-samrthome service by simply no longer processing 
   state change information that had no relevance to the service. This lowered its carbon output, but with API calls as an R, it would have not lowered its SCI score.
   
A good solution here might be to have an SCI with a fittingly chosen R for each group of applications made by each each development team. So fo the 3 voice teams the R might be processed requests, 
but with each service seen seperatly, for analytics connected devices and for the mcs-service API calls. 
If the project menagement insists upon one R for the whole system landsacape we should probably choose a definition of API calls that includes all reads/writes from/to Kafka topics as the least bad option.
```



#### What we include, exclude and why

What we include in both our measurements and embodied carbon measurements is largely based on what can be measured with what level of effort
and how important it is to set the right incentives to improve the carbon efficiency of the software.

First lets get the easy decisions out of the way:

- We definitely need to measure all electricity used by all systems we have control over and convert that into gCO2eq.
- We definitely need to [in some way](TODO add section about how to ahndle this and link it here) include electricity used by systems we make calls to, that are not under our control, as otherwise we would create an incentive to simply outsource carbon intensive calculations
- We definitely should not include carbon costs that were incurred when things like the fundamental infrastructure the internet runs on was created, the laptops we develop on were built, and other things that are very difficult to factor in

Now with the easy decisions out of the way, we turn to the system boundaries and embodied emissions that are harder to define. TODO TRi expand here

### Our Measurement and Calculation Approach

#### Cluster carbon measurement/estimation

The carbon footprint of your cluster will be decisive in many projects.

Our first standard recommendation is the [Cloud-Carbon-Footprint-Tool](../analysis/overview_guide_existing_measurement_tools.md#cloud-carbon-footprint-tool-by-thoughtworks).

Currently, we only have a recommendation and [step-by-step](TODO link to set up doc) guide for Azure hosted projects,
but the recommended tool also works for AWS and Google cloud, you just have to put a bit more work into setting it up.

Note some important things:
- Like all available tools, the CCF does only provide an estimation
- It's accuracy will increase if you give it access to an [electricity maps](../analysis/overview_guide_existing_measurement_tools.md#electricity-maps) account. While the CCF is free and open source, electricity maps is a paid service
- You need to give it at least "Reader" level access to you Azure subscription.
- It has some limitations to its granularity. It will measure the energy usage of you whole k8s cluster, not pod by pod TODO check if there really isn't a way to do this

So why the CCF? In short, it is easy to set up, gives you not only a good CO2eq estimation,
but also your electricity usage and estimated cost, and works with all 3 big cloud providers.
Its [methodology for CO2eq estimation](https://www.cloudcarbonfootprint.org/docs/methodology) is sound and finds approval in the Green IT community.
It is also a free open source project, as long as you are fine with the lower accuracy it has without the electricity maps account.
This makes it easier to pitch to potential customers, and once they are happy with the results, you can always upgrade and
buy an electricity maps account. TODO talk about potential MW EM account with group and then potentially edit here

We recommend that you talk to your PO or potential customer and show them what the CCF can provide, by showing them the
[dashboard of the Green-in-IT](http://mwgiitccf.westeurope.cloudapp.azure.com) project. If more convincing is needed,
follow our [step-by-step](TODO link to set up doc) guide to quickly set up an instance in a virtual machine.
If they are sold, [integrate it in your existing DevOps environment](TODO link to second part of set up doc).

TODO TRi finish


#### Website efficiency
