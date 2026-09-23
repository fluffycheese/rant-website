---
title: "The Road to Beta: From Zombie Panes to QR Codes"
pubDate: 2026-09-23
description: "RANT is officially moving to Beta. Here's a look at the latest features for mobile discovery, and the complex state-management bugs we squashed to get here."
author: "fluffycheese"
---

When I first started building RANT, the goal was simple: bridge the gap between error-prone spreadsheets and heavy-duty enterprise DCIMs. I wanted a tool that let you walk into a comms room, look at a rack, and document it in ten minutes without needing a Postgres cluster to do so. 

Today, RANT is officially moving from Alpha to Beta. The core feature set is solidifying, the database schema is stabilising, and we're ready for broader usage. The journey to get here involved tackling some tricky UI state issues and building features designed specifically for the person standing in front of the rack. 

Here's what it took to get to Beta, and what's new.

## Designing for the Comms Room

Most network documentation tools assume you are sitting at a desk with a massive monitor. But infrastructure doesn't live on a desk. You're usually balancing a laptop on a crash cart or trying to squint at your phone screen while tracing a patch cable.

To solve this, we've introduced three new features aimed directly at on-the-ground discovery:

**1. Mobile Scanner Views**
We built a stripped-down, focused UI specifically for mobile devices. The full app management remains on desktop where it belongs, but if you need to quickly diagnose an issue or discover what a specific port connects to, the mobile view gives you exactly the information you need, instantly.

**2. QR Code Labels**
Documentation is only useful if you can find it. You can now generate and print Avery-compatible (L7651) QR code labels directly from RANT. Stick them on your racks, patch panels, or wall panels. Scan the label with your phone, and the mobile app will jump you straight to that asset's documentation. Discovery is now as simple as pointing your camera.

**3. Client-Side CSV Exports**
Sometimes you just need raw data to hand to a contractor or audit. You can now export your single device ports and connections directly to CSV. It's fully generated on the client-side, making it incredibly fast and completely portable.

## Squashing the Bugs

Getting the app stable enough for Beta meant diving deep into React state management to fix some subtle but maddening edge cases in the UI.

**The Infamous "Zombie Pane"**
Our split-view trace feature is arguably RANT's superpower: letting you see end-to-end cable paths across multiple racks. But we had a nagging bug where closing the split view after a cross-site patch would leave behind a broken, uncloseable "zombie pane". It turned out the patching context was auto-clearing the target rack ID slightly too early. We fixed this by ensuring the RackTree explicitly retains the split-view state during cross-site patching, keeping the panels pristine.

**The Wall Panel Passthrough**
A less obvious bug involved our `wall_panel` device category. When tracing a cable through a patch panel, the app correctly followed the connection out the back slot. But wall panels, which have the exact same front/back passthrough behaviour (e.g., front to an AP, back to a patch panel), were acting as dead ends. We corrected the trace walk logic to treat wall panels with the respect they deserve, ensuring your traces don't mysteriously stop at the wall socket.

**Slot-Aware Endpoints**
Patching from the back slot of a device was notoriously clunky. We overhauled the Endpoints Table to generate one row per port *per slot*, making it fully slot-aware. Now, when you click the edit or patch button on a back-slot row, the app knows exactly what you're trying to do. No more wrong device modals or invisible connections.

## What's Next?

With the Beta release, the focus shifts towards polish, edge-case testing, and preparing for the 1.0 milestone. If you've been holding off on trying RANT because of the Alpha warning, now is a great time to spin up the container and take it for a spin. 

Thank you to everyone who has submitted issues, tested the demo, or just offered feedback. 
