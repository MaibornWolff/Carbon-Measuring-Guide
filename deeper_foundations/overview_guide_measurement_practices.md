# Green in IT Comparative Overview of Carbon Footprint Measurement Practices in the IT Landscape

This guide will give a comparative overview of the currently, commonly used measurement practices for the carbon footprint in the IT landscape. 
Based on this overview and those that can be found in the overview guide for existing tools and the overview guide for scientific results, 
the MaibornWolff project recommendation guide will then recommend an approach to use in IT projects that is 
both thorough and simple enough to be meaningful in its result and practical in its application. 

## Table of Contents

- [Glossary](#Glossary)
- [What can be measured and what should we measure](#what-can-be-measured-and-what-should-we-measure)
- [Carbon awareness](#Carbon-awareness)
- [Hardware efficiency](#hardware-efficiency)
- [Measurement](#Measurement)
  - [The GHG protocol](#The-ghg-protocol)
    - [GHG: Pro and Contra](#ghg-pro-and-contra)
  - [Software Carbon Intensity](#Software-Carbon-Intensity)
    - [How to calculate your SCI score](#how-to-calculate-your-SCI-score)
        - [Decide what to include](#decide-what-to-include)
          - [Choose your functional unit](#choose-your-functional-unit)
          - [Decide how to measure or calculate your emissions](#decide-how-to-measure-or-calculate-your-emissions)
          - [Quantify](#quantify)
    - [SCI: Pro and Contra](#SCI-pro-and-contra)
  - [Measurement comparison: Prioritising SCI over GHG](#measurement-comparison-prioritising-SCI-over-ghg)

### Glossary

| Term                  | Meaning                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CO2eq / CO2-eq / CO2e | Carbon dioxide equivalent. This unit of measurement indicates the potential impact of CO2 and non-CO2 gases on global warming in carbon terms.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| gCO2eq/kWh            | Grams of carbon per kilowatt hour. The standard unit of carbon intensity is gCO2eq/kWh, or grams of carbon per kilowatt hour.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| SCI                   | Software Carbon Intensity. A standard which gives an actionable approach to software designers, developers and operations to measure the carbon impacts of their systems.                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Carbon intensity      | Carbon intensity measures how much carbon (CO2e) is emitted per kilowatt-hour (KWh) of electricity consumed. The standard unit of carbon intensity is gCO2eq/kWh, or grams of carbon per kilowatt hour.                                                                                                                                                                                                                                                                                                                                                                                                          |
| Carbon awareness      | Term to describe information about the differing levels of carbon intensity at differing places and times                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Net zero              | Net zero means reducing emissions according to the latest climate science and balancing remaining residual emissions through carbon removals (neutralization). Net zero, by definition, requires emissions reductions in line with a 1.5°C pathway. All businesses must do this to achieve net-zero global emissions by 2050. <br />The critical differentiator between net zero and carbon neutral is net zero's focus on abatement rather than neutralization and compensations. A net-zero target aims to eliminate emissions and only to use offsetting for the residual emissions that you cannot eliminate |
| GSF                   | [Green Software Foundation](https://greensoftware.foundation/manifesto)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| GHG protocol          | [Greenhouse Gas protocol](https://ghgprotocol.org/) - the most commonly-used method for organizations to measure their total carbon emissions                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| RAPL                  | Running Average Power Limit is a power estimation feature in modern x86 CPUs from Intel and AMD. n the green software community it is extensively used in order to get accurate energy measurements for the CPU and DRAM components.                                                                                                                                                                                                                                                                                                                                                                             |



### What can be measured and what should we measure

#### Singular impacts

| Property                   | Most common unit(s)                                          |
| -------------------------- | ------------------------------------------------------------ |
| Electricity usage          | kW/h                                                         |
| Carbon footprint           | gCO2eq/kWh                                                   |
| Water usage                | Liter or m^3                                                 |
| Land usage                 | m^2 or hectare                                               |
| Air Quality                | Combination of different airborne compounds and gasses usually measured in parts per million. <br />Common compounds measured include, but are not limited to Ozone, Nitrogen Oxides, Sulfur Dioxide, Carbon Monoxide, Volatile Organics (includes many different compounds) |
| Solid waste products       | Combination of different solid compounds usually measured in kg or tonnes, or a mass per volume unit such as mg/m^3. <br />Common categories measured include, but are not limited to plastics (includes many different compounds), Controlled solid waste (includes many different compounds) or just overall solid waste, such as common household waste. |
| Environmental heavy metals | Environmental heavy metals such as chromium, arsenic, cadmium, mercury, and lead are usually tracked seperatly from other solid waste products. The unit is also either kg or tonnes, or a mass per volume unit such as mg/m^3<br />Note that there are also nutritionally essential heavy metals heavy metals such as iron, which can still be damaging when ingested in to high quantities. However, the five mentioned above are usually tracked the most |
| Polluted/unsafe Water      | Note that this is different from just water usage. Water used in e.g. agriculture gets used up, but at some point again becomes part of the water cycle. Polluted or unsafe water has been made unsafe for consumption or usage by being mixed with various substances that make it unsafe. Usually measured either in liter of m^3 |

#### Combined impact scores

| Score                                                                                                          | What it measures                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | By whom                                                                                                                                                                                                                              | Notes                                                                                                                                                                                                                                                                                                                                                           |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Environmental Performance Index (EPI)](https://epi.yale.edu/)                                                 | "The 2022 Environmental Performance Index (EPI) provides a data-driven summary of the state of sustainability around the world. Using **40 performance indicators across 11 issue categories**, the EPI ranks 180 countries on climate change performance, environmental health, and ecosystem vitality.". For an exact break down of those, [check page 17 of their report](https://epi.yale.edu/downloads/epi2022report06062022.pdf). <br />Uses a scale of 0-100 based on various indicators.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | [Yale Center for Environmental Law & Policy](https://envirocenter.yale.edu/) and Columbia University in collaboration with the World Economic Forum. TODO links                                                                      | [Can not be used for commercial purposes](https://epi.yale.edu/faq/epi-faq#)                                                                                                                                                                                                                                                                                    |
| [Ecological Footprint](https://www.footprintnetwork.org/our-work/ecological-footprint/)                        | "Ecological Footprint accounting measures the *demand* on and *supply* of nature. <br />On the demand side, the **Ecological Footprint©** adds up all the biologically productive areas for which a population, a person or a product competes. It measures the ecological assets that a given population or product requires to produce the natural resources it consumes (including plant-based food and fiber products, livestock and fish products, timber and other forest products, space for urban infrastructure) and to absorb its waste, especially carbon emissions. <br />On the supply side, a city, state or nation’s **biocapacity** represents the productivity of its ecological assets (including cropland, grazing land, forest land, fishing grounds, and built-up land). These areas, especially if left unharvested, can also serve to absorb the waste we generate, especially our carbon emissions from burning fossil fuel.<br />Both the Ecological Footprint and biocapacity are expressed in **global hectares**—globally comparable hectares with world average productivity." | [Global Footprint Network](https://www.footprintnetwork.org/)                                                                                                                                                                        | Calculated by Country, but its [API](https://data.footprintnetwork.org/?_ga=2.209205970.1350518277.1694610175-1301410648.1694610175#/api) could be used to evaluate impact more broadly                                                                                                                                                                         |
| Triple Bottom Line (TBL)                                                                                       | [According to Harvard business school](https://online.hbs.edu/blog/post/what-is-the-triple-bottom-line): "The triple bottom line can be broken down into “three P's”: profit, people, and the planet. Firms can use these categories to conceptualize their environmental responsibility and determine any negative social impacts to which they might be contributing."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | The term “triple bottom line”  was first coined in 1994 by John Elkington, business writer and founder of the management consultancy SustainAbility, but there is no one organization that maintains a standardized framework for it | More suited as a tool that takes the results of other scores into account to help the business make decisions                                                                                                                                                                                                                                                   |
| Environmental, Social, and Governance (ESG) Scores by Sustainability Accounting Standards Board (SASB) Metrics | Different standards per industry, determined by the [Sustainable Industry Classification System®](https://sasb.org/find-your-industry/), in our case most likely TC-SI (Software & IT Services)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | [International Sustainability Standards Board](https://sasb.org/about/)                                                                                                                                                              | "Commercial use of the Sustainable Industry Classification System® (SICS®) is restricted to those parties that have entered into commercial terms and use agreements. Interested parties who would like to use SICS® for commercial purposes may contact us at [sustainability_licensing@ifrs.org](mailto:sustainability_licensing@ifrs.org) for terms of use." |

#### Useful metrics for IT projects

Not all the above-mentioned metrics lend themselves well to IT projects. A metric should be evaluated under the following considerations

- It should not create a misaligned incentive + create an incentive to improve the product as intended
- It should not be easily "fooled"/avoided at the expense of other, not tracked metrics
- It should be (at least somewhat) easy to measure/lend itself to high data quality
- Cover all areas where an incentive for improvement is desired
- Be (somewhat) future-proof and expanded upon/integrated into an even better metric
- Have a universally agreed upon definition and clear meaning to all who read it

Electricity usage is the base metric for IT projects. The Carbon footprint can (mostly, see chapter on [hardware efficiency](#hardware-efficiency)) be calculated based on it if [carbon awareness](#carbon-awareness) is considered. All other metrics and impact scores can either be considered secondary in priority to electricity usage, in part derived from it, or electricity usage/a derived value of it will be part of a larger score. See the [project recommendation guide](../recommendations/mw_project_recommendation_guide.md#what-to-measure) for more details on what our conclusions are for when to use what. 

### Carbon awareness

Since not all electricity is generated in the same way, we need to factor into our calculations how much CO2eq was used to create each KW/h we measure/calculate. The standard unit of carbon intensity is gCO2eq/kWh. Carbon intensity varies by location and time. 

Location matters because on every grid is fed by the same mix of sources, some being much more carbon intensive than others. But even within the same location, what the individual users energy mix consist of, and hence their carbon intensity, can be changed by changing the sources they buy from within that grids energy market. We will not go in depth regarding the purchasing agreements of electricity from a specific mix of sources as it is of low relevance for our project, but for more on the topic see [here](https://learn.greensoftware.foundation/climate-commitments).

Both time in terms of the date of the year, and the time of day play a role. E.g. within a grid that is partially fed by solar panels, the carbon intensity might be lower in summer during the day, than during the night or in winter, since solar panels are a low carbon energy source. But what the available energy mix is at a given time, is not static. For this we need to take a look at [dispatchability and curtailment. A more in depth explanation can be found in the linked section](https://learn.greensoftware.foundation/carbon-awareness#dispatchability--curtailment), but in summery, dispatchability is how fast an energy source can come online and curtailment means throwing away energy when production outweighs demand. Not causing sudden and large spikes or drops in demand, helps avoid both. 

These concepts open up requirements for how we calculate our emissions and possibilities to save CO2eq without further lowering our energy usage. It is vital for getting us to net-zero, as we will always need to use some amount of energy, and currently most grids by themselves are not net zero. For a more in depth look on how to use the concept of carbon awareness, check out [this guide by the Green Software Foundation](https://learn.greensoftware.foundation/carbon-awareness#how-to-be-more-carbon-aware). 

In short, we can reduce carbon significantly - [45% to 99% carbon reductions depending on the number of renewables powering the grid](https://ieeexplore.ieee.org/document/6128960)- by what is called demand shifting. Demand shifting basically just means doing as much work when - temporal shifting - and where - spatial shifting - the produced energy is as green as possible, while trying to absorb as much energy that would otherwise be curtailed, and without causing spikes in demand that would require quickly dispatchable, dirty energy sources to come online. 

As per the Green Software Foundation, "some of the biggest technology companies have recognized the importance of carbon awareness and are using advanced modeling techniques to implement demand shifting."

- **Google Carbon Aware Data Centers** - Google launched a project to [make some of the cloud workloads carbon aware](https://blog.google/outreach-initiatives/sustainability/carbon-aware-computing-location/). They created models to predict tomorrow's carbon intensity and workload. They then shaped large-scale workloads so more would happen when and where the carbon intensity is lowest, but in such a way that they could still handle the expected load.
- **Microsoft Carbon Aware Windows** - [Microsoft announced a project to make Windows 11 more sustainable](https://www.techradar.com/news/windows-11-is-getting-an-eco-friendly-update-but-could-microsoft-do-more). Initially, this means running Windows updates when the carbon intensity is lower.

Additionally, there is the concept of [demand shaping](https://learn.greensoftware.foundation/carbon-awareness#demand-shaping), a sort of sister concept to demand shifting. As per the GSF, "demand shifting is the strategy of moving computation to regions or times when the carbon intensity is lowest. Demand shaping is a similar strategy. However, instead of moving demand to a different region or time, we shape our computation to match the existing supply." Meaning, if carbon intensity is low, increase the demand; do more in your applications and vise versa. This can happen automatically, or the user can make a choice. 

The limitation with this is of course, that applications are often limited in what they can do in this regard, especially when it comes to automatic implementations, as companies often want to avoid impacting the user experience without consulting the user first. However, there are cases that show that this is still a very valuable concept. Examples given by the GSF include

- Video conferencing software that adjusts streaming quality automatically. Rather than streaming at the highest quality possible at all times, it reduces the video quality to prioritize audio when the bandwidth is low.
- TCP/IP. The transfer speed increases in response to how much data is broadcast over the wire.
- Progressive enhancement with the web. The web experience improves depending on the resources and bandwidth available on the end user’s device.

We can also ask the user to consent to an eco-mode in our application, which will reduce performance, but save carbon and money. 

### Hardware efficiency

While the hardware used in cloud computing is not under the customers control, there are still ways to calculate its effect into our emissions metrics, and measures we can take to lower that impact. But how does hardware have an impact on our emissions besides the energy it uses to run? The answer is [embodied carbon](https://learn.greensoftware.foundation/hardware-efficiency#embodied-carbon). "Embodied carbon (also referred to as "embedded carbon") is the amount of carbon pollution emitted during the creation and disposal of a device. When calculating the total carbon pollution for computers running software, both the carbon pollution associated with running the computer as well as the embodied carbon of the computer must be accounted for." 

Of course, we need to [amortize](https://learn.greensoftware.foundation/hardware-efficiency#amortization) this amount over its average lifespan and only add the amount for our usage time to our score. The time we use it is of course known to us, but the initial CO2eq cost of its construction is a bit harder to find. There will be more in the [tools guide](#TODO an link to relevant tools section later) on this, but in short, for some cloud providers, values can be obtained. 

So how can we have an impact? More details can be found [here](https://learn.greensoftware.foundation/hardware-efficiency#how-to-improve-hardware-efficiency), but in short, increasing the utilization of each device, so e.g. using 100% of one device instead of 25% of 4 is much better, as the lifespan of a device will be roughly the same either way, but this way only demand for one device to be manufactured is created. Public clouds are much better at this than private clouds, by default, but our setting can have an impact here as well. 

### Measurement

#### The GHG protocol

[The GHG protocol](https://learn.greensoftware.foundation/measurement#the-ghg-protocol) is the most widely used and internationally recognized greenhouse gas accounting standard. [92%](https://ghgprotocol.org/about-us) of Fortune 500 companies use the GHG protocol when calculating and disclosing their carbon emissions.

The GHG protocol divides emissions into three scopes:

- **Scope 1**: Direct emissions from **operations** owned or controlled by the reporting organization, such as on-site fuel combustion or fleet vehicles.
- **Scope 2**: Indirect emissions related to **emission generation of purchased energy**, such as heat and electricity.
- **Scope 3**: Other indirect emissions from all the other activities you are engaged in. Including all **emissions from an organization's supply chain;** business travel for employees, and the electricity customers may consume when using your product.

Scope 1 is irrelevant for our purposes. Scope 2 is only relevant if the project uses a private of Hybrid Cloud. Otherwise our emissions fall into scope 3. 

For both a public cloud, and a front end, our CO2eq will come from both embodied emissions and the energy consumed when running it, wit both falling under scope 3. In case of a private cloud, its consumed energy falls under scope 2 and its embodied energy under scope 3.

For software, regardless of whether it's run on infrastructure we own, rent, or consumers own, there are three parameters to consider for bucketing emissions:

- How much energy it consumes
- How clean or dirty that electricity is
- How much hardware it needs to function

To calculate a total for software carbon emissions, you need access to detailed data regarding the energy consumption, carbon intensity, and hardware that your software is running on. This is challenging data to gather, even in the case of an organization's own closed-source software products where they can track its usage with telemetry or logs.

Open-source software maintainers don't have the same visibility into how and where their software is used, how much energy is consumed, and on what hardware.

Open-source projects typically have multiple contributors from multiple organizations. As a result, it’s unclear who should be responsible for calculating the emissions as well as who is accountable for eliminating them. When you also consider that open-source software makes up 90% of a typical enterprise stack, it is clear that there is going to be a large amount of carbon emissions that are not accounted for.

##### GHG: Pro and Contra

But is the total even what we need? Is it the best metric by which we should judge our efforts to reduce our footprint? 
Take [this](https://learn.greensoftware.foundation/measurement#do-totals-tell-the-whole-story) scenario from the GSF:

"A total is only one metric that describes the state of something. To make the right decisions, you need to look at many different metrics.

Imagine a scenario where you are the leader of an organization and charged with reducing the emissions of your software. 
You measure the emissions in Q1 and come out with a total of 34 tonnes. After making some investments into projects that eliminate emissions, 
you find that by Q2 the emissions have increased to 45 tonnes. Does this mean your efforts failed?

Not necessarily. We know that a total by itself doesn't tell the whole story and must look at other metrics to find out 
if an emissions-reduction project has been successful. For example, if you measured the carbon intensity as well as the carbon total, 
you might come out with a different perspective. In the same project, if the carbon intensity was 3.3g CO2eq/user in Q1, 
and 2.9g CO2eq/user in Q2, you might consider the project a success and continue to invest further.

While the total informed you that your organization's carbon emissions had increased overall, 
the intensity gave a more complete perspective that would help you to make a more informed decision on how to proceed."

To summarise: 
Pros:

- Nothing is missed, all sources though out the whole lifecycle of the product are taken into account
- Harder to manipulate by hiding carbon sources
- The entire system can always be viewed as a hole, no need to split anything into smaller analytical components

Cons:

- Can create perverse incentives that dissuade from making investments into efficiency that would pay off long term
- Can clash with economic incentives such as growing the user base, leading decision makers to disregard greenness as a non-functional requirement entirely

#### Software Carbon Intensity

The [Software Carbon Intensity (SCI) specification](https://grnsft.org/SCI) is a methodology developed by the Standards Working Group of the Green Software Foundation, designed to score a software application along a dimension of sustainability and to encourage action towards eliminating emissions. 
It will form the basis for assessing the effectiveness of our decisions and actions. Why will be shown once we understand how to calculate it, as well as some important concepts regarding carbon efficiency.

The SCI is calculated as follows:

```
SCI = ((E * I) + M) per R
```

In this formula

- E = Energy consumed by a software system
- I = Location-based marginal carbon emissions
- M = Embodied emissions of a software system
- R = Functional unit (e.g. carbon per additional user, API-call, ML job, etc)

So SCI = C per R (Carbon per R). C is the total amount of carbon released by operating the software. R is the core characteristic of the SCI and turns it into an intensity rather than a total. This is what is called a *functional unit*.

##### How to calculate your SCI score

Follow these four steps to calculate your SCI score:

- [Decide what to include](#Decide-what-to-include)
- [Choose your functional unit](#choose-your-functional-unit)
- [Decide how to measure your emissions](#decide-how-to-measure-or-calculate-your-emissions)
- [Quantify](#Quantify)

###### Decide what to include

What software components to include or exclude in the SCI score means defining the boundaries of your software; where it starts and where it ends. For every software component you include, you will need to measure its impact. For every major component you exclude, you need to explain why.

The SCI specification doesn't currently make any demands regarding what to include and what not to include. However, you must include all supporting infrastructure and systems that significantly contribute to the software operation.

###### Choose your functional unit

The SCI is a rate rather than a total. It measures the intensity of emissions according to the chosen functional unit. The specification currently doesn't prescribe, which functional unit shall be used. Based on a customers application we might choose between different ones that make sense in that specific case. Usually, a good functional unit is the scaling unit of the application/software. Examples for functional units are:

- Number of users (Could be daily active users, registered users, etc.)
- Time spent by customers in the application (Could be minutes of video content streamed, seconds on/in the website/app, etc.)
- Connected devices (Could be connected IOT devices, user devices, etc.)
- Number of API Calls (Every API offered could be its own functional unit if they differ strongly, or one can just treat any equally)
- Data Volume transferred
- The time the servers are running without consideration of what specifically they are doing

###### Decide how to measure or calculate your emissions

Once you have a list of the software components you want to measure and the functional unit you will use to measure them, the next step is to decide how you will quantify the emissions of each software component.

There are two methods of quantification; measurement and calculation.

- **Measurement** is using meters or gauges. For example, measuring the energy consumption of your software component by using an external wattmeter between the wall socket and the device, or using a tool build into the hardware that measures energy consumption, e.g. [RAPL](https://www.intel.com/content/www/us/en/developer/articles/technical/software-security-guidance/advisory-guidance/running-average-power-limit-energy-reporting.html). One should use measurements when ever possible.
- **Estimation** typically involves proxy metrics and models to calculate an approximation of the energy consumed or the green house gases emitted. For instance, if you cannot directly measure your application's energy consumption but instead have a model that estimates the energy consumption based on the CPU utilization, this is considered an estimation rather than a measurement.

In the case of applications running in the cloud, you will be faced with one of these two scenarios:

1. The cloud provider or a third party has measurement tools available. You will find a list of tools [here](./overview_guide_existing_measurement_tools.md).
2. No such tooling is available, then you will either have to estimate consumption. You can do this either based on
    1. proxy metrics, which the cloud provider reports such as: details from your bill, the amount of storage or compute used, the amount of data transferred, the number of requests/invokes, or
    2. based on tests and measurements you run on devices, which you have control over.

In case of desktop or other locally run application, as well as the local/end-user component of an application, measurements are much more realistic. Tools like [Scaphandre](./overview_guide_existing_measurement_tools.md#scaphandre) can help with this, reducing the initial effort needed to set up the measuring infrastructure.

###### Quantify

Once all software components are defined and an SCI score for each can be calculated. A total SCI of your application - the sum of all component SCIs - is only possible though,
if you chose the same functional unit for each of them, hence it is advisable to do that, unless there is a very good reason not to.
In such a case components with the same functional units would be grouped and seen as a separate application with their own SCI score.
E.g. might be seeing the desktop application of a product as one application with its own functional unit and SCI score,
and the cloud hosted backend of the same product as another application with its own functional unit and SCI score.

##### SCI: Pro and Contra

Pro:

- The SCI is a great KPI, because it can sum up the impact of all of your components and express the overall impact in one number.
- It rewards improving any part of your software that is included in its calculation scope.
- It helps you track your emissions over time and encourages you to improve.
- No risk of misaligned incentives, does not clash with basic economic incentives such as growing the user base.

Contra:

- It does not include emissions and impacts during the development or end of live phases of your software, which is especially problematic for software which uses deep learning or similar approaches, since they are often most energy and carbon intense during their training/development.
- Only optimising your SCI score won't help you reach goals like those from the Paris Climate Accord. Because you can reduce your SCI, while increasing your overall total carbon emissions, even if your SCI encompasses every aspect of your software. Here is an example to demonstrate:
  - Scenario #1:
    - Given:
      - R = 1 User
      - M = 10 kg
      - I = 1 kgCO2eq/kWh
      - E = 1 kWh
    - Result:
      - Total Carbon released: 11 kg.
      - SCI = 11 kgCO2eq/User
  - Scenario #2:
    - Given:
      - R = 100 User
      - M = 100 kg
      - I = 1 kgCO2eq/kWh
      - E = 10 kWh
    - Result:
      - Total Carbon released: 110 kg.
      - SCI = 1.1 kgCO2eq/User
- The scope of what to include is not specified (see [Decide what to include](#decide-what-to-include)). This can lead to efforts to manipulate it by strategically excluding components
- It becomes hard to manage if you cannot apply the same R for all parts of your software. This can lead to situations where you will have to group components of you product into different groups for SCI assessment.

#### Measurement comparison: Prioritising SCI over GHG

SCI is not a replacement for the GHG protocol, but an additional metric that helps software teams understand how their software behaves in terms of carbon emissions, so they can make more informed decisions.
While the GHG protocol calculates the **total emissions**, the SCI is about calculating the **rate of emissions**.

This is usually more useful for software projects, and creates better incentives in the long run to reduce carbon output, so we recommend to prioritizing SCI over GHG.
However, as mentioned in the contra section of the [SCI: Pro and Contra](#SCI-pro-and-contra), projects with large upfront energy investments, such as training some kind of AI model (see e.g. training an Amazon Alexa Custom Skill), should give at least equal attention to GHG.

