---
title: "Rebuild, Don't Repair"
pubDate: 2026-09-13
description: "Why I design infrastructure to be disposable, reproducible and predictable, and how that philosophy shaped RANT."
author: "fluffycheese"
---

There is a phrase I use a lot when talking about infrastructure:

**Rebuild, don't repair.**

It sounds slightly ridiculous when you first hear it. If something is broken, surely you fix it. That's what troubleshooting is for. And sometimes you absolutely should. 

But I've spent enough time maintaining infrastructure to learn that there is a point where repairing something stops being engineering and starts being archaeology. You're no longer fixing the system. You're trying to work out why it was configured that way three years ago, which undocumented change broke it last Tuesday, whether somebody manually edited a configuration file, and whether changing one thing will quietly break three others.

That's the point where I'd rather throw it away and build it again.

## The Jenga Tower of IT

Traditional infrastructure tends to accumulate state.

A server starts with a clean operating system. Someone installs an application. Someone changes a configuration file. Someone adds a firewall rule. A package gets pinned because an upgrade broke something. A temporary workaround becomes permanent.

A year later, the server still works. That's the dangerous part.

Because working infrastructure creates an incentive not to disturb it. Every manual change to a server is like pulling a brick from a Jenga tower. It keeps working, until one day it doesn't. Nobody remembers which change tipped it, and nobody wants to be the one who touches it next.

The configuration becomes tribal knowledge. Maybe there's a spreadsheet. Maybe there are some notes in a wiki. Maybe the configuration is backed up somewhere. Eventually, the person who originally built it leaves. Now you have a perfectly functioning server that nobody really understands.

The first time it breaks, you're not repairing infrastructure anymore. You're reconstructing history.

## Infrastructure Should Be Disposable

My preferred solution is to make the infrastructure itself less important.

If I can destroy a server and recreate an identical one from a known configuration, the server isn't precious anymore. 

Instead of treating the system like a puzzle to solve:

```text
Something is broken.
        ↓
Log into it.
        ↓
Find out what changed.
        ↓
Try to fix it.
        ↓
Hope nothing else breaks.
```

I'd rather rely on a reproducible state:

```text
Something is broken.
        ↓
Determine the desired state.
        ↓
Destroy the broken instance.
        ↓
Rebuild it.
        ↓
Verify it.
```

The second approach isn't necessarily faster for every failure, but it is considerably more predictable. And predictability is usually more valuable than cleverness.

## Desired State Over History

This isn't an excuse to blindly reinstall everything whenever something goes wrong. Databases contain data. Storage systems contain data. Sometimes the machine itself isn't the problem. 

"Rebuild, don't repair" is really shorthand for something more specific: **Don't make manual mutation of a system the only way to recover it.**

If the desired state exists somewhere outside the machine, rebuilding becomes an option. If it doesn't, you're dependent on the machine remembering how it became the machine it is. 

This is why I like declarative infrastructure. I don't particularly care that a server currently has a particular configuration. I care that I can describe what the server *should* look like and have the system converge on that state.

Imperative infrastructure tends to record history (install this, change that, edit this file). Declarative infrastructure records intent (this is the machine I want). When something goes wrong, I'd much rather have the second. It gives you something to rebuild from.

## The Same Principle Applies to Documentation

This is where the idea became interesting to me when building RANT.

RANT isn't trying to record the history of a network. It's trying to describe its current state. 

A documentation system can easily become just another historical record. Someone connected this cable here, someone moved this device there, someone changed this port configuration six months ago. That information might seem useful, but it isn't what you actually need when a critical service is down and you are standing in a cold comms room at 2 AM trying to understand the network.

What I actually want to know is:

```text
What exists?
        ↓
Where is it?
        ↓
What is connected to it?
        ↓
What is at the other end?
```

RANT's job is to act as the source of truth for that state. The database isn't intended to become a diary of every change that has ever happened. It provides a structured description of what the infrastructure is supposed to look like right now. It doesn't automate the physical changes, but it holds the explicit blueprint that reality should match.

That makes it much closer to the way I think about infrastructure itself.

## The Model Should Be Rebuildable Too

This philosophy also influences the way I approach RANT's data model.

It's tempting for infrastructure software to grow endlessly. Every time a new requirement appears, another field gets added. Then another relationship. Then another special case. Eventually the software becomes as complicated as the infrastructure it was supposed to make understandable.

I don't want that.

The core model should remain relatively small:

```text
Sites
  ↓
Locations
  ↓
Racks / Devices
  ↓
Ports
  ↓
Connections
```

Everything else should justify its existence. The complexity should come from the infrastructure being documented, not from the software required to describe it. A small, explicit model is easier to understand, easier to validate, and ultimately, easier to rebuild.

## RANT Is Not About Preserving the Past

I'm not particularly interested in making RANT a historical archive of everything that has happened to a piece of infrastructure. I'm interested in making it an accurate description of what exists.

If a switch is replaced, the important question isn't "What switch used to be here?" It's "What switch is here now, and what is it connected to?"

If a connection changes, the important thing is that the model reflects the new connection. The system should describe reality rather than becoming another source of historical baggage. History has value, but it shouldn't be required to understand the current state.

## Dismantling the Jenga Tower

This philosophy is incredibly powerful when you inherit an undocumented, tangled mess of cables: the physical Jenga tower.

RANT can document that tower exactly as it is today. But because the state is now captured outside the physical racks, rebuilding it becomes a safe, calculated option. 

If I want to rebuild a rack, I can export the current "London" site as a JSON file, and import it as a new site called "London Rebuild". From there, I can delete devices, replan the patch panel layouts, and trace out all the new cable runs virtually. I can figure out exactly what the new environment should look like without unplugging a single live connection.

Once the physical migration is complete and reality matches the blueprint, I just delete the old "London" site and rename the new one. 

You don't have to try and repair the spaghetti. You model the rebuild, and then you execute it. It turns a high-risk, weekend-ruining migration into a calm, predictable operation.

## Predictability Over Cleverness

I prefer boring systems that I understand. I prefer rebuilding something from a known state to spending hours trying to repair an unknown one. I prefer explicit configuration to undocumented magic. And I prefer software that answers the question I actually have rather than software that gives me 200 additional things I didn't ask for.

For me, "rebuild, don't repair" is a design principle.

Build systems where the desired state is known. Keep important information outside the thing that can fail. Make infrastructure reproducible. Avoid making undocumented manual changes part of the system's identity. 

And when something becomes untrustworthy, make replacement a normal operation rather than a disaster.

That's the same principle I've built into RANT. The goal isn't to sell you the biggest, most complex enterprise platform possible. It's to give you a small, understandable, and completely trustworthy representation of your physical infrastructure. If you are tired of playing Jenga with your network and want a tool that helps you actually plan your way out of the mess, that is exactly what RANT is designed to do.

**Rebuild, don't repair.**

Not because repair is always wrong. Because a system you can reliably rebuild is a system you don't have to be afraid of breaking.
