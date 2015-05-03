# Aristeia

Aristeia is an experiment in building a CI system that I'm happier about using as a developer than the systems which I've used so far.
I've used quite a few, but things frequently return to Jenkins, because I most frequently work in an environment where a few conditions prevail:
* I need something driven from a private SCM
 * It may not be Git
* I may need other triggers
* It's got to be more complex in scheduling than a simple script
* It frequently involves branches
* The CI system is the center of an ecosystem of delivery systems
* I want a great developer experience

However, these needs often give me issues:

### SCMs
The experience of integrating with some SCMs is not great. 
I have plenty of experience with Perforce, for example, but the experience of using it is frequently sub-optimal in a number of CI systems.

### Branches
Many CI systems use configuration that comes from the CI system.
This frequently creates problems since it cannot deal properly with changes to the code that require changes to what you need to do with it.
These changes may need to be done, and then ported across branches, each time being recreated.

### User Experience
Many CI systems have some strange UX choices at their core - for a developer, there are frequently things that I want to get to easily and quickly, like logs for build failures, which are hidden multiple clicks away.
Tweaking these things in many CI systems is non-trivial.

#### Personalization
I also frequently want to personalize the front page of my CI system - 
* What's the latest passing build? 
* What's the URL or path for it?
* What's a human-readable status for the latest deployment?
* Where are *my* pending builds in the queue?

#### Extension
If I want to extend a CI system, I often need to create an extension in Java.
If I'm not a Java developer, this is really an impediment.

#### Performance
CI systems frequently do a lot, including storing a lot of results, logs and more.
They're frequently designed to be standalone systems, so these things are integrated with the CI system.

However, they can often do these things poorly (like filling up the local disk with loads of logs, getting really slow if you don't prune them).
If we rely on other systems to handle these tasks, we can take much better advantage of systems that are designed to handle them, can be better maintained by system administrators, have dedicated hardware or infrastructure backing them.

## What is Aristeia supposed to do about this?
*Note: this is a wishlist, not a feature list for complete features*

### Configuration lives with the Code
I've done this with internal systems I've built for years, and it works well with DSL systems, Travis CI and others.

### The CI System is a REST Web Service
First and foremost, the interface to Aristeia should be REST - this should make it easier to build other systems that integrate with it.

### The Interface is yours
With a REST API, the interface can be implemented using pure HTML (or anything else). 
Create a whole custom interface, or just tweak how things look.

#### All the HTML
The intent is to build the default interface from WebComponents, making it easier to decouple, swap and reuse elements wherever you want.
Similarly, the intent is also to use newer features like websockets.

### The CI System is smaller
Rather than package or build a DB, or system for storing logs. Aristeia will rely on other systems as much as it can.
* Use Splunk, Greylog or ELK for the logs, as long as you can get to them in your UI
* Use a database
* Use S3 or whatever you want for storing builds

This will mean that there's more work expected on the behalf of the person running the system.

### Plugins
Plugins in Artisteia are inspired by Packer.io plugins - they're standalone executables (or web services).
The implementation spec will be provided so that you can create them with scripts or executables written in whatever language you want.
