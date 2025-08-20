---
layout: post
title: "PulsePoint: The Cardiac Monitoring Device of Tomorrow"
date: 2024-05-05 01:00 +0700
#modified: 2020-03-03 16:49:47 +07:00
tags: [biomed, engineering, health]
description: From Medtronic to NIH, my team's magnum opus upon graduating provided insights impossible to gain any other way.
image: 
---

The concept of the device started simple enough, but evolved into a strange technical problem. Most devices of this nature focused into one camp and did not traverse. Photoplethysmography (PPG) and Imaging Photoplethsymography (iPPG) were clearly different things, right?
Well, they are in fairly distinct ways in not input, but outcome and important metrics:

## Generic PPG is precise.
Generic PPG utilizes one or two photodiodes1 and various LEDs at specific orientations, sensitivity levels and wavelengths to translate through and capture differentials in tissue diffused blood. Generally, wavelengths between 540-940nm are emitted from the LEDs, depending on desired penetration distance. Historically, green LEDs at around 530 wavelength have proven the most successful, as such are most commonplace in consumer-grade devices such as the Apple Watch and Fitbit, although this is likely not a hard rule. Additionally generic PPGs tend to be extremely close to the skin. PPGs viewing finger oxygenation levels are in enclosed and dark environments by design with direct skin contact. Both of these presumed specifications lead to a specific result:

	(Image1) (image2)

Figure 1 one provides the environment a generic PPG will collect during use and figure 2 showcases its collected voltage differentials over time. By design, Generic PPGs are highly sensitive, but not particularly specific. Ultimately, the photodiode picks up light signals without compromise from whatever direction they come from. This feature is regulated by external aspects, but even in isolation, there comes issues related to crosstalk, saturation and lots of noise. Despite these, it is the most common use since these can indeed be externally regulated, but it comes with a particular downside: The acquired signal has limited informative content. If the goal is identifying physical ailments, irregular signals may not be enough, even under more consistent circumstances.

## iPPG is informative
iPPG, taken at varied distances, utilizes CMOS cameras and high powered LEDS to collect particular wavelengths of light upon specified areas of the body. Generally, wavelengths between 520 nm – 600 nm are used, chosen for their balance between penetration depth and visible contrast in captured images. Generally covering a much larger portion (like the face), subtle differentials are detected between pixels in the recording to calculate tissue blood flow. Much like Generic PPG, it depends on periodic light absorption changes in hemoglobin, but rather than treating the signal as a single stream of voltage variation, it distributes that signal spatially. Every pixel becomes a micro-detector.

	(Image)

By its nature, it collects more information theoretically. With a high enough frame rate, blood transfer throughout the body can be tracked, and a larger surface area for analysis gives a more holistic view of blood flow. The information is not locked to a single point, allowing the system to infer motion, orientation, and even local perfusion differences without strict control of the environment. As such, it is not inaccurate to suggest that iPPG can be considered more informative.

These two techniques had been considered distinct for awhile, and thus used in different environments. PPG remained the mainstay for wearables and medical-grade finger or ear clips where light control could be guaranteed, while iPPG was relegated to academic demonstrations, computer-vision research, and experimental monitoring at a distance. The split was partly cultural and partly technical: one camp optimized for signal strength and reliability under constrained conditions, the other for rich spatial information under less predictable ones. 
Hardware complexity, computational overhead, and legacy design assumptions discouraged any crossover. Traditional PPG designers favored compact circuits and minimal processing, while iPPG researchers leaned on high-resolution cameras and heavy post-processing pipelines, making a unified approach seem impractical.
## Enter PulsePoint
	PulsePoint aimed to bridge this gap. It was specifically designed for near-sighted CMOS monitoring of local areas of the body to capture the best of both worlds, controlling the environment to get maximum data from target areas for better analysis. Real time analysis was cast aside in an effort to provide a more user friendly 30 day analysis to assist in or solely identify heart diseases. It was believed that with the right constraints, components and hurdle overcoming that a more effective heart monitoring wearable could be designed and implemented.
For those more well-versed in the field, it may be clear that even with proper optical and light components, some portions of skin create too much noise and dilute any signal one would hope for internally. As in, the ratio of light, even for penetrating wavelengths, that reach the depth of interest worsens depending on skin thickness and skin color. These are true, but our hypothesis was that with more nuanced medical data, that noise and signal could be more readily discriminated against and allow for a proper path for diagnosis. A 0D signal, no matter how good, simply lacked the necessary nuance. Acceleration-tracked, depth knowledgeable and self-adjusting, Pulsepoint would solve both the pain points described by doctors and patients of bulky consumer wearables for long-term monitoring for cardiac patient analysis in one swoop.
Engineering Challenges
Something like this by default appeared rather unfeasible, and uncertain if true value would be comparable to its expected costs. We decided to put in the effort to confirm/deny these assumptions. Some challenges, while not exhaustive were clear:
Near-Field Optimal Geometry, Power and Heat, Data Processing Algorithms & Machine Learning, & Per-User Specifications were all highly technical challenges. As each deserves their own section, how Pulsepoint was designed will be touched on piece by piece. In due time, a link to each will be provided, if not already.
Ultimately, this was considered an engineering problem, not a science problem. 
