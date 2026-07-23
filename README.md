# Smart Agriculture System — Irrigation for Hilly Terrain

*A soil-moisture-driven pump controller built for terrace farming in mountainous regions*

**Smart India Hackathon 2025** &nbsp;·&nbsp; Problem Statement `SIH25062` &nbsp;·&nbsp; Team SLASHA
Theme: Agriculture, FoodTech & Rural Development &nbsp;·&nbsp; Category: Hardware

| | |
|---|---|
| **Status** | Working hardware prototype |
| **Core idea** | Automated irrigation triggered by live soil readings, not a fixed schedule |
| **Stack** | Arduino Uno · FC-28 soil sensor · relay · DC pump |

---

### Contents
[The Problem](#the-problem-we-were-solving) · [What We Built](#what-we-built) · [Hardware](#hardware) · [Circuit](#how-the-circuit-is-wired) · [Control Logic](#the-control-logic) · [Design Trade-offs](#design-decisions-worth-explaining) · [Demo](#demo) · [Roadmap](#where-this-idea-goes-next) · [Impact](#why-this-matters-the-numbers-we-pitched)

---

## The problem we were solving

Hilly regions like Sikkim farm on terraces, and irrigation there is a different problem than on flat land. Water doesn't stay where you put it, dry summers hit hard, and there's no smart infrastructure to manage any of it — so farmers either over-water and lose topsoil, or under-water and lose yield. We wanted to fix the "nobody's watching the soil" part of that problem first, since it's the one a low-cost sensor setup can actually solve.

## What we built

A soil-moisture-driven irrigation controller: an Arduino reads a soil sensor continuously, and switches a pump on or off through a relay based on real, live soil conditions instead of a fixed schedule. No app, no manual valve-turning — the field waters itself when it needs to and stops when it doesn't.

This is the hardware core of a bigger idea (dashboard, SMS alerts, rainwater harvesting, solar power) that we scoped out but didn't have time to build for the hackathon — noted honestly below instead of overstating what exists.

## Hardware

| Component | Role in the system |
|---|---|
| Arduino Uno | Reads sensor, drives relay logic |
| FC-28 soil moisture sensor | Analog reading of soil condition |
| 1-channel relay module | Switches pump power on/off |
| DC water pump | Delivers water to the field |
| 9V battery | Isolated power line for the pump |

## How the circuit is wired

```
FC-28 sensor --(analog)--> Arduino --(digital out)--> Relay --> Pump
                                                          ^
                                                       9V supply
```

The sensor and the pump don't share a power rail — the relay is what bridges Arduino logic (5V, low current) to the pump circuit (9V, higher current), which is the standard way to keep a microcontroller from getting fried by a motor's back-EMF or current draw.

## The control logic

The core idea is a threshold with a gap in it, not a single cutoff:

- Reading goes **above** the dry threshold → soil is too dry → pump turns **on**
- Reading drops **below** a separate, lower wet threshold → soil is wet enough → pump turns **off**

Using two thresholds instead of one matters here — a single cutoff would make the pump flicker on and off rapidly right at that boundary as the reading naturally jitters. The gap between thresholds absorbs that noise.

## Design decisions worth explaining

| Decision | Why we chose it | Trade-off / what we'd revisit |
|---|---|---|
| FC-28 resistive sensor | Cost and availability for a hackathon build | Corrodes faster outdoors long-term — a capacitive sensor would be more durable |
| Relay module for switching | Simple to prototype and debug fast | Slower switching and shorter lifespan than a MOSFET |
| Two-threshold (hysteresis) logic | Prevents rapid on/off flicker at the boundary | Threshold values still need per-soil calibration |

**With more time**, we'd move to a capacitive moisture sensor for durability, and add a real-time clock so watering also respects a max-runtime safety cutoff independent of the sensor — in case the sensor fails and reads permanently "dry."

## Demo

[`demo.mp4`](./demo.mp4) — working prototype in action.

## Where this idea goes next

The hackathon submission scoped a larger system beyond what's in this repo:

- Solar/battery power so the pump can run at night or off-grid
- A web/mobile dashboard with SMS alerts for remote monitoring
- Rainwater harvesting with tank-level sensors
- Frost protection and terrace-runoff mitigation for hill-specific conditions

## Why this matters (the numbers we pitched)

| Metric | Estimate |
|---|---|
| Yield improvement | ~20–30%, from consistent need-based watering vs. guesswork |
| Water savings | ~30–40%, versus flood/manual irrigation |
| Who it's for | Small and marginal farmers, FPOs, rural development agencies |

## Team

SLASHA — Smart India Hackathon 2025

## References

- Analysing Challenges and Strategies in Land Productivity in Sikkim
- Hill Agriculture: Challenges and Opportunities
- Agriculture in Hilly and Mountainous Landscapes: Threats, Monitoring and Sustainable Management
