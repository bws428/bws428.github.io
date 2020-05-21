---
title: Rocket Stability and Performance
date: 2007-10-12
image: assets/images/arduino-2.jpg
categories: rocketry
tags: featured
---

The [Rocket-Mounted Video Camera](/web/20120313000532/http://principialabs.com/rocket-mounted-digital-video-camera) project is nearing completion. Prior to the flight test phase, however, prudence demands that we run a few simulations to ensure that the rocket will have a safe and stable flight.

## Initial Evaluation

Before beginning construction, we made a few ballpark calculations using (a free trial version of) [RockSim](http://www.apogeerockets.com/rocksim.asp), a Windows-based model rocket design tool, to verify that the addition of the camera wouldn’t adversely affect the model’s flight characteristics. To increase performance, we also swapped the specified D12 engine for a higher-impulse E9.

The images below show the results of this evaluation. You can see from the flight profile graph on the left that the predicted maximum altitude with the E9 motor is nearly 1000 feet. The stability diagram on the right shows the center of gravity (CG) position with the E9 engine, but without the addition of the video camera in the nose.

[![Flight Profile](/web/20120313000532im_/http://principialabs.com/images/flight_profile_E9-8_sm.jpg "Click to enlarge")](/web/20120313000532/http://principialabs.com/images/flight_profile_E9-8.jpg) [![CP/CG Analysis](/web/20120313000532im_/http://principialabs.com/images/rocksim_2d_sm.jpg "Click to enlarge")](/web/20120313000532/http://principialabs.com/images/rocksim_2d.jpg)

Although the CG was a little further aft than is desirable, we determined that the addition of the video camera and battery pack to the nose of the rocket would only improve stability, and that the E9 motor would help compensate for the loss of performance that the camera’s weight would create. Based on this preliminary analysis, we decided to go ahead with rocket construction, using an Estes E9-8 engine, the video PCB and a 2AAA battery pack.

Now that the build is complete and the camera is installed, we need to revisit these calculations with the actual mass measurements, to assure that we have an accurate picture of the flight profile before the first test flight.

## Rocket Stability

Like the feathers of an arrow, the fins of a model rocket keep it flying straight and true. Air moving over the fins creates a lift force which helps to straighten out the rocket’s flight path when small perturbations like wind gusts or engine thrust asymmetry attempt to push it off course. The position of two imaginary points on the rocket body, the Center of Gravity (CG) and the Center of Pressure (CP), determine how stable the rocket flight will be.

As with any flight vehicle, forces on a rocket will cause it to pivot about its CG. The CG of the rocket is best determined when the model is prepped and ready to fly: that is, after the engine, recovery system and heat-resistant wadding are installed.

In the preliminary analysis, we used RockSim to estimate the CG position on the rocket’s fuselage based on the average masses of engines, parachutes, and modeling clay (in the nosecone, to move the CG forward). Now that the actual build is complete, we can determine the true flight CG by prepping the rocket for flight as described above, and balancing it.

Using this method, we determined that the CG of our model is 286mm from the aft end of the tailcone. This includes the installed E9-8 engine, the parachute and wadding, the video camera and 2AAA battery pack, along with some modeling clay in the very tip of the nosecone. The clay is necessary because the Canadian Arrow kit is undesirably tail-heavy by design.

Now, for a stable rocket flight, the CG needs to be at least one _caliber_ (the diameter of the fuselage) forward of the CP. We know that the CG position can change depending on how we load the rocket (i.e., where we put the weight). The Center of Pressure, however, is dependent only on the design of the rocket. The rocket’s length and the shape, size and position of the fins determine the location of the CP. The CP can be determined longhand using the [Barrowman Equations](http://my.execpc.com/~culp/rockets/Barrowman.html), or automagically by using a program like RockSim or [SpaceCAD](http://www.spacecad.com/).

Since we didn’t alter the design of the rocket aerodynamically, the CP we found using RockSim in our preliminary analysis will still be correct. In our case, the CP is 208mm from the aft end of the tailcone. The fuselage diameter is 66mm, and the difference between CP and CG is 78mm — about 1.18 calibers stability. This isn’t ideal, but it is better than the marginal 0.95 that we came up with originally.

With these calculations we can be reasonably assured that our rocket won’t go spiralling off into oblivion on its first flight.

For more detailed information on this topic, consult the "Rocket Stability" page from NASA’s "[Beginner's Guide to Rockets](https://www.grc.nasa.gov/WWW/K-12/rocket/shortr.html)."

## Rocket Performance

The performance of our rocket — acceleration, velocity, altitude — depends on the vehicle’s weight, aerodynamic drag, and the engine’s thrust and burn time. Because the biggest engine Estes makes is the E9, we figured we’d throw that sucker in and be good to go. The preliminary analysis showed no problems. But during the course of the build, our rocket got a little … uh … heavy.

The [Estes Engine Chart](/web/20120313000532/http://principialabs.com/pdf/estes_engine_chart.pdf) (PDF 20K) recommends 283 grams as the maximum total liftoff weight for a model rocket using the E9-8 engine. Well, our little prima donna weighed in this afternoon at a hefty 363 grams. My first concern was that this fatty wouldn’t even get off the pad with an E9, but that turned out not to be the case.

The same chart lists maximum liftoff weights for the E9-6 and E9-4 at 340g and 425g respectively. All three E-impulse engines have the same propellant weight, the same total impulse, the same max thrust, and the same thrust duration. So why the difference in max recommended liftoff mass?

The answer, of course, is the ejection delay. Heavier rocket, shorter ejection delay. Why? Well, since the thrust curve of all three engines are identical, the boost phase for a given rocket riding all three engines should be identical. But it’s all about the coast phase. The time from engine burnout to apogee is (obviously) less for a heavier rocket than for a lighter one. So, my theory was that with the long-delay engines — the E9-8 and E9-6 — the ejection charge would occur way after apogee, with the vehicle velocity steadily increasing as it plummeted back to earth.

I needed to run some simulations.

Since it had been more than 30 days since I first downloaded RockSim, the trial period had expired and I was locked out of the software. Damn! But, not to worry, there is always [Markworld!](http://webalt.markworld.com/webalt.html)

Mark Sullivan has created a simple and effective HTML rocket altitude predictor that can be used for performance calculations. Just fill in some data fields about your rocket and engine type, and voila!

I ran three simulations, one for each E-impulse engine type, to determine max altitude, velocity and the point of parachute deployment. The results are as follows:

### Estes E9-4

![E9-4](/web/20120313000532im_/http://principialabs.com/images/E9-4.gif "E9-4")

The E9-4 graph is characteristic of the perfect model rocket flight: a quick boost to apogee, velocity goes to zero, and the recovery system is deployed shortly afterward. (The three dots on the blue altitude curve mark engine burnout, apogee and ejection.) Maximum altitude is significantly lower than originally predicted — 136 meters (448 ft) — due to the increased mass of the finished rocket. The airframe glides at a constant velocity under the canopy to a soft touchdown rougly 29 seconds after liftoff at a gentle 6.2 m/s. Chute deployment occurs at a velocity of 3.15 m/s, creating a shock on the airframe of 1 g. Nice flight.

### Estes E9-6

![E9-6](/web/20120313000532im_/http://principialabs.com/images/E9-6.gif "E9-6")  
Notice that the boost profile and max altitude of the E9-6 are _identical_ to that of the E9-4, since the thrust profile is the same. What differs is the time and velocity of the chute deployment. In this case, ejection occurs at 21.04 m/s, creating a shock of 10.38 g. That’s a big difference, and we’re endangering our rocket by creating unnecessary stress on the shock cord and recovery system.

### Estes E9-8

![E9-8](/web/20120313000532im_/http://principialabs.com/images/E9-8.gif "E9-8")  
We’ve got three of these engines sitting in the package and ready to fire. They’ll boost the rocket to apogee just as well as the other two E-impulse, but recovery stress borders on unacceptable. Descent velocity climbs to a whopping 33.25 m/s, creating a shock of **27.50 g!** We might try these engines just for fun, but it’s clear that there is a significant danger of losing the vehicle.

So, we know now that our rocket is safe and ready to fly, and that the E-impulse engines should be sufficient to huck this hefty beast off the launchpad. But our choice of an 8-second ejection delay is unacceptable for the first flight test. We’ll pick up a pack of E9-4s before we head out to the range.

For more detailed information on model rocket performance, visit NASA’s [Rocket Engine Performance](http://exploration.grc.nasa.gov/education/rocket/rktengperf.html) page.

### Author's Note

This article was originally published on [Principia Labs](https://web.archive.org/web/20120313000532/http://principialabs.com/rocket-stability-and-performance/).
