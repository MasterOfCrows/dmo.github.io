---
layout: post
title: "[WIP] PulsePoint: The Cardiac Monitoring Device of Tomorrow"
date: 2024-12-10 01:00 +0700
modified: 2025-9-30 16:49:47 +07:00
tags: [biomed, engineering, health]
description: From Medtronic to NIH, my team's magnum opus upon graduating provided insights impossible to gain any other way.
image: 
---

![alt text](/assets/img/PP_concepts.png)
> Figure 1. The talented [Eurydice Molina's](https://poly.rpi.edu/staff/eurydice-molina/) first sketches of our concept for the wearable frame. Much appreciated!

The ability to measure blood flow noninvasively has transformed both medical diagnostics and consumer health monitoring. Photoplethysmography (PPG), the optical technique at the heart of most wearables, provides a simple yet powerful way to detect cardiovascular dynamics by shining light into tissue and observing how it scatters and is absorbed by blood. From hospital-grade oximeters to fitness trackers on millions of wrists, PPG has become the default standard because it is inexpensive, compact, and reliable under controlled conditions.

Yet, this very simplicity is also its limitation. Traditional PPG condenses a rich physiological process into a single waveform. Still useful for detecting broad patterns like heart rate, but prone to noise, crosstalk, and misinterpretation when specificity is required. For detecting subtle irregularities such as atrial fibrillation, the signal alone often falls short.

In parallel, imaging-based PPG (iPPG) emerged in academic and research settings. By leveraging CMOS cameras and advanced processing, iPPG transforms every pixel into a detector, mapping spatial and temporal blood flow variations across the skin. Though richer in information, iPPG historically remained impractical for wearable applications due to hardware and computational demands.

This split between robust but shallow PPG and informative but cumbersome iPPG created a technological gap. Our project, PulsePoint, was conceived to bridge that divide! We would combine near-field optics with modern processing to capture the specificity of PPG and the spatial nuance of iPPG, in a form factor usable for long-term cardiac monitoring.

## Generic PPG is precise.
Generic PPG utilizes one or a few photodiodes and various LEDs at specific orientations, sensitivity levels and wavelengths to translate through and capture differentials in tissue diffused blood. Generally, wavelengths between 540-940nm are emitted from the LEDs, depending on desired penetration distance. Historically, green LEDs at around 530 wavelength have proven the most successful, as such are most commonplace in consumer-grade devices such as the Apple Watch and Fitbit, although this is likely not a hard rule. Additionally generic PPGs tend to be extremely close to the skin. PPGs viewing finger oxygenation levels are in enclosed and dark environments by design with direct skin contact. Both of these presumed specifications lead to a specific result:

![alt text](/assets/img/ppg_basic.png)
> Figure 2. [Photo taken by University of Cambridge](https://www.mdpi.com/2673-4591/2/1/80) and is not PulsePoint. 

Figure 2a one provides the environment a generic PPG will collect during use and figure 21b showcases its collected voltage differentials over time. By design, Generic PPGs are highly sensitive, but not particularly specific. Ultimately, the photodiode picks up light signals without compromise from whatever direction they come from. This feature is regulated by external aspects, but even in isolation, there comes issues related to crosstalk, saturation and lots of noise. Despite these, it is the most common use since these can indeed be externally regulated, but it comes with a particular downside: The acquired signal has limited informative content. If the goal is identifying physical ailments, irregular signals may not be enough, even under more consistent circumstances.

## iPPG is informative
iPPG, taken at varied distances, utilizes CMOS cameras and high powered LEDS to collect particular wavelengths of light upon specified areas of the body. Generally, wavelengths between 520 nm – 600 nm are used, chosen for their balance between penetration depth and visible contrast in captured images. Generally covering a much larger portion (like the face), subtle differentials are detected between pixels in the recording to calculate tissue blood flow. Much like Generic PPG, it depends on periodic light absorption changes in hemoglobin, but rather than treating the signal as a single stream of voltage variation, it distributes that signal spatially. Every pixel becomes a micro-detector.

![alt text](/assets/img/ippg.png)
> Figure 3. [Taken by Department of ECE, Karunya Institute of Technology and Sciences](https://www.mdpi.com/2227-9709/9/3/57), and is displayed for informative purposes only. 

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

> **The design team will develop an AF screening to monitor the patient's heart rate and rhythm and be able to detect minute differences between regular and irregular heart rhythms to identify all occurrences of AF. The device shall record all data in a user-friendly, reliable, and accessible manner, and should be safe, durable and does not interfere with daily functions.**

This was created during our senior year capstone. It shifted somewhat after the year, but the main idea was maintained.

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

These aren't design specifications, of course. Those changed more elastically after college and were largely defined by customer discovery. But that will be in the business side, to post later.

# Near-Field Optics 

Of of the most critical (and critiqued externally) was handling the optics to target the necessary blood flow areas. See, there had been historically some confusion as to how to best acquire PPG data because of a bit of a mystery in the PPG world: **why green wavelength LEDs led to better data than red ones.**

Theoretically speaking, wavelengths between 800-916nm has been sited as the wavelenth blood cells can absorb readily. PPG technology had historically attempted to use such for blood oximetry and blood-volume data collection. But studies had shown that for one reason or another, different wavelengths ~550nm, green light, had often produced more reliable data. The rationale for why had been unclear, but it had been theorized. [In a 2016 experiment reported from Vanderbilt University](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0165413), green light performed unequivably better than its near infrared (NIR) counterpart in terms data discernability and signal amplitude, alongside characterization of what the team labeled as 'hot spots', locations highly active and easily discernable in imaging. Their theory as to why is what our team considered as fact: green wavelengths likely help detect deformations in tissue near capiliaries, and with additional external pressure to said tissue this effect is magnified. 

Green light is indeed absorbed by the skin, and as such the scattering effect is lessened for detection than IR. Since arteries with significant blood flow are significantly deeper below the skin (3mm artery vs 0.3-0.8mm capiliaries), a wavelength that is absorbed and reflected does so much more predictably by avoiding deeper targets. With this information in mind, we wanted to have our near IPPG device attempt to 'map' these deformations across time. 

Knowing this, that means that for Near-Field Optics, we must be rather careful about where our focal point is!

![Focal](/assets/img/focal-length-graphic.jpg)
> Figure 4. Focal length is a static ratio a thin lens has that determines the necessary ratio between distance of the object and distance of the image sensor. Without apertures to modify, it sets the focused image permanently.

If you wished to see an object far or near, you must be able to focus your eyes, changing the focal point through fine-motor muscle deformations of your lens. Our inorganic counterparts, cameras, lack this ability, but can be mechanically shifted with various shuttles or fixed with static lenses. In an ideal scenario, we would be able to adjust the focal point for maximum detail in the final product. But for the MVP, we selected a particular lens capable of viewing objects milimeters away. 

Due to the physics of near-field imaging, it actually turned out to be fairly trivial to get a small setup capable of handling our use-case:

Focal length dictates the effective distances and magnification of objects to any one camera setup.

\\[ \begin{align}
\frac{1}{f} = \frac{1}{d_o} + \frac{1}{d_i},
\\ \\ \frac{1}{d_o} = \frac{1}{f} - \frac{1}{d_i},
\\ \\ d_o = (\frac{1}{f} - \frac{1}{d_i})^{-1}
\end{align} \\]
*with \\(d_o = \text{distance from object}, d_i = \text{distance from image sensor}\\), and \\(f = \text{focal length}\\)*
> Equation 1. Important relation was toyed with to figure out if a wearable iPPG system was possible.

With this relationship, the distance from the image sensor (being the lens length) had to be greater than the focal length. But a more important and somewhat distressing relationship appeared. The closer the focal length is to the distance from the image sensor, the further away the object has to be to get in focus! 

In order to not reduce our range for acceptable distances too greatly, or be too sensitive to any user movements, we went for a happy medium: the [Wide-Angle 4.8mm M12 Lens](https://commonlands.com/products/wide-angle-5mm-m12-lens-cil948?syclid=186435c9-0f49-4b49-87d6-2d5c6701c2d9). It has an effective focal length of 4.8mm, a distortion rate of -9% (concerning but somewhat unavoidable) and a max resolution of 3MP, enough to do our work! We opted not to have the IR filter in case the light ends up useable for our models.

![M12Lens](\assets/img/WAM12Lens.png)
> Figure 5. The M12 lens purchased. It had a length of 18mm, accounted for during Equation 1 calculation for image distance.

Looking back at *Equation 1*, a distance of only ~6.46 milimeters from any targeted body part is needed to get crisp imaging! That gives us a decent enough range to collect what we could need and room for adjustment. 

So we have diodes of varying light emissions & a lens to focus said light. Now we need the light sensor itself. Getting ahead, we purchased the aptly named 'Full HD Sony® Starvis™ IMX462 Ultra Low Light USB 3.1 Gen 1 Camera' after a long search period. Why? Glad you asked:

![IMX](\assets\img\imx462.png)
> Figure 6. The IMX462. It did come with a lens, but for much more normal ranges, counted in feet. We replaced it with ours upon arrival.


For our goals, we want to essentially gather information from two potential target sites: the veins and proximal recipients of capilliary blood. These have two vastly different distances and light absorption rates. Not to mention, one is further below a wall of cells designed to absorb and reflect most wavelengths. If attempted to view a vein in the arm, for example, it would be like trying to view a baseball game 10 yards away through two dirty glass panes with flashlights pointed at you: possible, but dominated by noise. 

The capiliaries are easier to identify, in a roundabout way and are often what is acquired in general PPG applications. Much closer to the surface, it can theoretically even be mapped in flow, with a high enough FPS. Future applications, of course, but the math has been done. 

Additionally, the heart can emit heart rates as high as 200 beats per minute (bpm) after high activity! At rest, tachycardia can be faster than normal heart rates of around 100bpm, while a normal resting heart rate can be 60-100. With these numbers in mind (and considering nyquist frequency), we would need a fps of at least 3.34 for resting heart rates, and 6.67 for active heart rates. Luckily, most cameras operate at ranges greater than 30FPS generally. 

This says nothing about the type of camera, though. CMOS or CCD was the next question. A short, but in depth tour of the scence made CMOS the choice, although it could be argued that more expensive types would be more ideal. Innovation in the CMOS scene had gone to outpace any potential downsides in comparison to CCD.

We needed a sensor that could operate under low power (+-5V), pick up Near-IR and green light, process at decent speeds and take photos capable of high detail. In an ideal world, the camera is more expensive than we we ended up with, but under budget and time constraints, our MVP chose the IMX462. With it's near-IR capabilities, it was a small hope that the IR and red light may pick up further depth light absorption differentials as well. Whether that panned out will be in the second to last section.

At 1937 x 1097 Pixels (2MP), 5V requirements with a wattage of 0.8-2.4W, we barely met our constraints. Luckily, they were still attainable with some clever series voltage tricks.

# Per-User Specification

(WIP)

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

That last requirement proved quite narrowing. HD video frames taken on a battery-powered wearable has not been commonly seen. At 1960x1080, this equates to 254 megabytes per second at 5 frames, and we would most certainly need more. We would expect our RAM/processors to be able to handle at least this much data at any given second. 

Based on our specifications, we landed upon the following:

![CoraZ7](/assets/img/cora.png)
> Figure 7. There was also a consideration of physical dimensions, since for the sake of demonstration we desired the MVP to be wearable, even if it did not represent its final form factor. It came in at a perfect 2.28x4.00 inches.

The [Cora Z7](https://digilent.com/reference/programmable-logic/cora-z7/reference-manual?__cf_chl_tk=_4YqFlM25p62cZIs048.AS.2smdxG5rc6vkYuBtWhIc-1758492159-1.0.1.1-DWIIwPpE9sfKdSgOIvoFvUbpHQc1AySa6O11Qjlzz7U). It contained 512mb of DDR3 with a 16-bit bus moving at 525MHz and a microSD slot that could store images. It had a 3.3V analog I/O with a 2A maximum current, exactly enough to power the specified camera (with external voltage assistance), and could handle an external battery pack as a power source. Coupled with separate I/O capabilities for other accessory components such as an accelerometer and adequate CPU, it was clear that this was indeed an ideal choice.

# Ergonomics, Power & Heat Management

With each component picked out for the MVP, we needed to be able to both house each component & a power source of some kind. It would have been trivial to have a wall-powered setup, but I personally demanded it be wearable and individually powered  for demonstration and feasibility purposes.

![Initial_Des](\assets/img/init.png)
> Figure 8. The wonderful [Eurydice Molina](https://poly.rpi.edu/staff/eurydice-molina/) made these assets as well. Much appreciated!


Screw fit for the FPGAs, non-flamable, electroresistent glue for the batteries, and a unique component (that cannot yet be revealed) to ensure accurate close contact readings from the wrist; we had all we needed to integrate the various components and wire them as needed for our first MVP!

We went simple with power. The design allowed for two [Duracel MN908 6V](https://www.amazon.com/Duracell-MN908-Non-Rechargeable-Alkaline-Batteries/dp/B005TLJX9E) alkaline batteries. In parallel, they were expected to provide around 26 Amps of power, enough to power the Cora Z7 and the associated camera for over 6 hours at max power by our calculation. More than enough for our demo.

# Data Collection & Machine Learning

(WIP, much to showcase.)

# Component Purchase & Integration 

Unfortunately, this is where things started to fall apart. I regret to inform that the device was never fully integrated. Not because of technical limitations, but temporal and financial ones. 

In stroke of fantastic luck, our August/September orders intended to arrive merely a couple weeks turned into months. By the time our components came in November, school deadlines and came, and NSF funding shifted internally for political reasons. My team had gone full force into their other responsibilities (such as graduate school or employment). Come December, our provisional patent collapsed and most of our work became public domain. 

The email chain that showed that one of the components were lost due to an address problem. Would have been caught sooner if my school email wasn't disabled after graduating and ended up in proprietary spam!

A tragic set of events for what was an incredible journey joined started by us all. Still, I do believe there are insights to be gleaned from this effort, and while it was but a peek into what was possible, I do believe in time that these efforts will be vindicated. 

**I cannot thank Martin, Hannah, Eurydice, Maddy, Dr. Hisham Mohamed, Dr. Yan Pingkun, and many others for their assistance, time and collaborations! Thank you professor Brett Orzechoswki who recommended us to the national NIH, despite it all. Not to mention all of the potential consumers, doctors, nurses, BMETs and so many more for their time and provided to help push forward a small dream. Finally, thank you RPI & NIH for providing us the opportunities to prove our mettle, and Medtronic/BMES for sponsoring our first forray into the medical device world back in 2023.** 