---
title: "Why I Built RANT - There Was Nothing in Between"
pubDate: 2026-09-01
description: "The frustration that led to building a lightweight tool for documenting physical network infrastructure, and why nothing else quite fit."
author: "fluffycheese"
---

I've spent more years than I'd like to admit maintaining network infrastructure across multiple sites. Comms rooms, server racks, patch panels, the kind of stuff that works perfectly until someone moves a cable and nobody writes it down. Over the years I've tried every reasonable approach to documenting what's plugged in where, and each one left me frustrated in a different way.

This is the story of why RANT exists.

## The Spreadsheet Era

Like most people in this position, I started with spreadsheets. A shared Excel file on a network drive, columns for rack, device, port, destination. It worked (barely) for a single rack. The moment you had two racks with patch panels connecting them, the spreadsheet became a maze of cross-references. Tracing a cable from a wall port through a patch panel, across a trunk, through another patch panel and into a switch port meant opening multiple tabs, Ctrl+F-ing for port labels, and hoping whoever last updated the sheet hadn't made a typo.

They had. They always had.

The spreadsheet approach has a fundamental problem: **it has no concept of a connection**. Every row is an isolated fact. There's no way to ask "where does this port *actually* go?" without manually walking the chain yourself, row by row, tab by tab. And when the answer matters, when a user's phone has stopped working and you need to trace the cable path *now*, that manual walk is painfully slow.

## Trying the Existing Tools

So I went looking for something better. I found three categories of tool, each excellent at what it does, but each leaving part of the problem unsolved.

**ECCM** was the closest to what I wanted. Its "click to patch" interface is genuinely brilliant: you select two ports, click connect, and the relationship is recorded. I could visualise cable paths instantly. But ECCM has no concept of racks or sites. Everything lives in a single flat view in browser localStorage. For one rack in a home lab, that's fine. For multiple comms rooms across multiple buildings, it falls apart. And the localStorage limitation means your documentation lives and dies with a single browser profile.

**Rackula** solved a different piece. Beautiful drag-and-drop rack elevations with real device images, the kind of thing you'd put on a wall in a server room. But it has zero cable awareness. You can see *what's in the rack* but not *what's connected to what*. Two entirely separate problems, two entirely separate tools, and no way to connect them.

**NetBox** does everything. Racks, cables, IPAM, VLANs, circuits, power, tenancy: it's an enterprise-grade platform with REST and GraphQL APIs, webhooks, and a plugin ecosystem. It is genuinely impressive engineering. But running it requires PostgreSQL, Redis, a task queue, background workers, and real operational commitment. I tried deploying it for a relatively small environment and spent more time maintaining NetBox than I did documenting cables. For a team of one managing a few dozen devices across two or three sites, it's like buying a container ship to cross a river.

## The Gap

The pattern was clear. On one side: simple, focused tools that each solve one piece of the puzzle (cables *or* racks, never both) with minimal overhead. On the other side: full DCIM platforms that solve everything but demand serious infrastructure just to run.

**There was nothing in between.**

Nothing that combined rack layout with cable documentation in a single tool. Nothing that supported multiple sites without requiring a database cluster. Nothing that a single person could deploy in five minutes and immediately start documenting their infrastructure.

That gap is exactly where RANT sits.

## What RANT Does Differently

RANT takes the proven data model from NetBox (Sites, Racks, Devices, Ports) and strips away everything that isn't about the physical layer. No IPAM, no VLAN management, no circuit provisioning. Just the thing I actually needed: **a shared, persistent record of what's in each rack and what's connected to what**.

The cable documentation uses the same click-to-patch pattern that made ECCM so intuitive, but extends it with front and back port awareness, cross-site connections, and full end-to-end cable tracing. Click a port anywhere in the chain and see the complete path, from wall plate through patch panel, across a trunk link, through another patch panel, and into the switch port. The kind of trace that took fifteen minutes with a spreadsheet takes one click.

The deployment story is deliberately simple. SQLite for the database, no PostgreSQL, no Redis, no background workers. A single Docker container, a Nix build, or Cloudflare Pages with D1. Same codebase, three deployment targets. If you can run a container, you can run RANT.

## Early Days

RANT is still in alpha. The database schema is not yet stable, the API will change, and there are rough edges everywhere. I'm building it in public because I think the gap it fills is real, and I'd rather get feedback early than polish in private.

If you've felt the same frustration (the spreadsheet that's always slightly wrong, the tool that does cables but not racks, the platform that does everything but needs a team to run) I'd encourage you to [try the demo](https://demo-rant.fluffycheese.co.uk) or [look at the source](https://github.com/fluffycheese/rant). Contributions, feedback, and honest criticism are all welcome.

The goal is simple: if you can draw your network on a whiteboard in ten minutes, you should be able to document it in RANT in ten minutes.
