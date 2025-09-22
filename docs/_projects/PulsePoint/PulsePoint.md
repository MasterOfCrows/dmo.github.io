---
layout: post
title: "[WIP] PulsePoint: The Cardiac Monitoring Device of Tomorrow"
date: 2024-12-10 01:00 +0700
modified: 2025-9-21 16:49:47 +07:00
tags: [biomed, engineering, health]
description: From Medtronic to NIH, my team's magnum opus upon graduating provided insights impossible to gain any other way.
image: 
---

The concept of the device started simple enough, but evolved into a strange technical problem. Most devices of this nature focused into one camp and did not traverse. Photoplethysmography (PPG) and Imaging Photoplethsymography (iPPG) were clearly different things, right?
Well, they are in fairly distinct ways in not input, but outcome and important metrics:

## Generic PPG is precise.
Generic PPG utilizes one or two photodiodes1 and various LEDs at specific orientations, sensitivity levels and wavelengths to translate through and capture differentials in tissue diffused blood. Generally, wavelengths between 540-940nm are emitted from the LEDs, depending on desired penetration distance. Historically, green LEDs at around 530 wavelength have proven the most successful, as such are most commonplace in consumer-grade devices such as the Apple Watch and Fitbit, although this is likely not a hard rule. Additionally generic PPGs tend to be extremely close to the skin. PPGs viewing finger oxygenation levels are in enclosed and dark environments by design with direct skin contact. Both of these presumed specifications lead to a specific result:

	[WIP]

Figure 1 one provides the environment a generic PPG will collect during use and figure 2 showcases its collected voltage differentials over time. By design, Generic PPGs are highly sensitive, but not particularly specific. Ultimately, the photodiode picks up light signals without compromise from whatever direction they come from. This feature is regulated by external aspects, but even in isolation, there comes issues related to crosstalk, saturation and lots of noise. Despite these, it is the most common use since these can indeed be externally regulated, but it comes with a particular downside: The acquired signal has limited informative content. If the goal is identifying physical ailments, irregular signals may not be enough, even under more consistent circumstances.

## iPPG is informative
iPPG, taken at varied distances, utilizes CMOS cameras and high powered LEDS to collect particular wavelengths of light upon specified areas of the body. Generally, wavelengths between 520 nm – 600 nm are used, chosen for their balance between penetration depth and visible contrast in captured images. Generally covering a much larger portion (like the face), subtle differentials are detected between pixels in the recording to calculate tissue blood flow. Much like Generic PPG, it depends on periodic light absorption changes in hemoglobin, but rather than treating the signal as a single stream of voltage variation, it distributes that signal spatially. Every pixel becomes a micro-detector.

By its nature, it collects more information theoretically. With a high enough frame rate, blood transfer throughout the body can be tracked, and a larger surface area for analysis gives a more holistic view of blood flow. The information is not locked to a single point, allowing the system to infer motion, orientation, and even local perfusion differences without strict control of the environment. As such, it is not inaccurate to suggest that iPPG can be considered more informative.

These two techniques had been considered distinct for awhile, and thus used in different environments. PPG remained the mainstay for wearables and medical-grade finger or ear clips where light control could be guaranteed, while iPPG was relegated to academic demonstrations, computer-vision research, and experimental monitoring at a distance. The split was partly cultural and partly technical: one camp optimized for signal strength and reliability under constrained conditions, the other for rich spatial information under less predictable ones. 

Hardware complexity, computational overhead, and legacy design assumptions discouraged any crossover. Traditional PPG designers favored compact circuits and minimal processing, while iPPG researchers leaned on high-resolution cameras and heavy post-processing pipelines, making a unified approach seem impractical.

# Enter PulsePoint

PulsePoint aimed to bridge this gap. It was specifically designed for near-sighted CMOS monitoring of local areas of the body to capture the best of both worlds, controlling the environment to get maximum data from target areas for better analysis. Real time analysis was cast aside in an effort to provide a more user friendly 30 day analysis to assist in or solely identify heart diseases. It was believed that with the right constraints, components and hurdle overcoming that a more effective heart monitoring wearable could be designed and implemented. If Holter monitors were the gold standard and the Apple Watch was the consumer standard, we wanted to fill the market gap of in between, or even better.

For those more well-versed in the field, it may be clear that even with proper optical and light components, some portions of skin create too much noise and dilute any signal one would hope for internally. As in, the ratio of light, even for penetrating wavelengths, that reach the depth of interest worsens depending on skin thickness and skin color. These are true, but our hypothesis was that with more nuanced medical data, that noise and signal could be more readily discriminated against and allow for a proper path for diagnosis. A 0D signal, no matter how good, simply lacked the necessary nuance. Acceleration-tracked, depth knowledgeable and self-adjusting, Pulsepoint would solve both the pain points described by doctors and patients of bulky consumer wearables for long-term monitoring for cardiac patient analysis in one swoop.

# Engineering Challenges

Something like this by default appeared rather unfeasible, and uncertain if true value would be comparable to its expected costs. We decided to put in the effort to confirm/deny these assumptions. Some challenges, while not exhaustive were clear:
Near-Field Optimal Geometry, Power and Heat, Data Processing Algorithms & Machine Learning, & Per-User Specifications were all highly technical challenges. Eventually, manufacturing came into importance, but this was towards the end of the journey. As each deserves their own section, how Pulsepoint was designed will be touched on piece by piece. If you are more interested in the business experience, you can click here.


Ultimately, this was considered an engineering problem, not a research problem. Before diving in, our problem statement:

 The design team will develop an AF screening to monitor the patient's heart rate and rhythm and be able to detect minute differences between regular and irregular heart rhythms to identify all occurrences of AF. The device shall record all data in a user-friendly, reliable, and accessible manner, and should be safe, durable and does not interfere with daily functions. 

This was created during our senior year. It shifted somewhat after the year, but the main idea progressed.

# User Specifications

We cannot design a device without knowing what the end-user will require of it. If the device came in the form of a 20lb suitcase that you held to get your hand artery's elasticity, few users, even under direction of a doctor, would use it. Our initial user specifications looked like this: 

| Category                      | Requirements |
|:-----------------------------:|--------------|
| **Main Function(s)**         | <ul><li>Patient-centered, reliable detection and reporting of atrial fibrillation</li><li>Sense the presence of atrial fibrillation</li><li>Provide frequency information</li><li>Provide duration information</li></ul> |
| **Performance**              | <ul><li>Accurate symptom history</li><li>High specificity</li><li>High sensitivity</li><li>Ergonomic (comfortable for long-term wear/use)</li></ul> |
| **Safety**                   | <ul><li>Does not interfere with regular heart rhythm</li><li>Does not interfere with daily activities</li><li>Does not conflict with existing treatment plan</li><li>Maintains a low risk of failure and adverse reactions</li></ul> |
| **Product Life**             | <ul><li>Product life should exceed the patient's expected lifespan need</li></ul> |
| **Packaging**                | <ul><li>Easy to disassemble</li><li>Durable and protective</li><li>Sterile and decontaminated on delivery</li></ul> |
| **Decontamination**          | <ul><li>Easy to clean and maintain</li></ul> |
| **Materials**                | <ul><li>Biocompatible</li><li>Able to withstand the operating environment</li></ul> |
| **Aesthetics, Appearance, & Finish** | <ul><li>Aesthetically pleasing</li><li>Low profile</li><li>Identifiable (branding/markings as needed)</li></ul> |
| **Production Quantity**      | <ul><li>Efficiently mass manufactured</li></ul> |

These aren't design specifications, of course. Those changed more elastically after college and were largely defined by customer discovery. But that will be in the business side.

# Near-Field Optimal Geometry 

Of of the most critical (and critiqued externally) was handling the optics to target the necessary blood flow areas. See, there had been historically some confusion as to how to best acquire PPG data because of a bit of a mystery in the PPG world: **why green wavelength LEDs led to better data than red ones.**

Theoretically speaking, wavelengths between 


# Ergonomics & Heat Management



# Data Collection & Machine Learning



# Per-User Specification



# Central Computing
Every device needs a brain, and for PulsePoint, the choice wasn’t trivial. Our prototype had to process image data on the fly, store it reliably, and still remain wearable. A simple Arduino or Raspberry Pi couldn’t keep up with the kind of bandwidth we envisioned. Custom silicon was out of the question at this stage.

That left us with FPGAs. They offered the flexibility to handle parallel image pipelines while giving us direct control over timing and power. The trade-off, of course, was complexity — programming an FPGA is far from plug-and-play — but the payoff in performance would make it worthwhile.

Each component needs to be powered and controlled, naturally. For the MVP, we opted to use an FPGA instead of a custom board or an Arduino (and equivalents) for the first real prototype. Why? We anticipated on-board image processing that the majority of low-cost components lacked. 

In order to make a choice for which FPGA, we searched for the following:

- Self-Powered for >1h
- Weigh Total <1lb 
- Capable of powering each component necessary simultaneously
- Capable of storing >100mb of images
- Handle >5 1960x1080 images processed and stored onto storage per second.

That last requirement proved quite narrowing. HD video frames taken on a battery-powered wearable has not been commonly seen.

There was also a consideration of physical dimensions, since for the sake of demonstration we desired the MVP to be wearable, even if it did not represent its final form factor. 

Based on our specifications, we landed upon the following:
![CoraZ7](/assets/img/cora.png)

The [Cora Z7](https://digilent.com/reference/programmable-logic/cora-z7/reference-manual?__cf_chl_tk=_4YqFlM25p62cZIs048.AS.2smdxG5rc6vkYuBtWhIc-1758492159-1.0.1.1-DWIIwPpE9sfKdSgOIvoFvUbpHQc1AySa6O11Qjlzz7U). It contained 512mb of DDR3 with a 16-bit bus moving at 525MHz and a microSD slot that could store images. It had a 3.3V analog I/O with a 2A maximum current, exactly enough to power the specified camera, and could handle an external battery pack as a power source. Coupled with separate I/O capabilities for other accessory components, it was clear that this was indeed an ideal choice.



# Power

Based on prior specifications, the device needs to power a:

1. 3.3V Rail
2. 1.0V Rail
3. 1.8V Rail
4. 1.35V Rail
5. Micro-SSD Writing


# Component Purchase & Integration 

Now, the most fun part about any MVP: Integration. And by fun, I mean most difficult. 








[WIP]
