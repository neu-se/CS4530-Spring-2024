---
layout: page
title: Traffic Light Activity Handout
nav_exclude: true
---

## Overview
In this activity, you will do a simple example to help you recognize failure of the five code-level design principles 

### Step 0: Getting started
Start by downloading the [(Starter Code)]({{ site.baseurl }}{% link Activities/module04-traffic-light-activity.zip %}).
Run `npm install` to download the dependencies for this project, and then open it in your IDE of choice. 
Run `npm run test` to run the tests of the existing code (or run the script in VSC).  You will see that the tests pass.

### The Problem

Your team is writing a simulator for a traffic light in a large city.  Whitley, an intern in your group, has written the code in TrafficLight.ts, and the tests in TrafficLight.test.ts

Your team leader, Adrian, has asked you to critique Whitley's code based on the Five Code-Level Design Principles.  Adrian says that he has already received some feedback from the client, who complained that the traffic engineers like to be able to fine-tune the durations of the red, yellow, and green based on actual traffic flow.  They also pointed out that some traffic lights have options other than red, yellow, and green, such as "left-turn green".

### Part 1: Critique Whitley's code.

In a text file, list several ways in which Whitley's code violates the 5 Principles.

### Part 2: Improve Whitley's code

Write a new file, called betterTrafficLight.ts, which Principles, and addresses some of the problems raised by the customer.  Test your code by changing TrafficLight.test.ts to import from betterTrafficLight.ts

### Part 3: Critique your own code

Now look at your own code.  Based on your experience with traffic lights, list 3 assumptions that your code makes about the behavior of traffic lights in a large city.  In what ways is the client likely to be unhappy with the assumptions you made?  (Stretch goal: how could you redesign your code to make it more flexible?)

### Submit your work

Instructions for submitting this activity will appear in Canvas.