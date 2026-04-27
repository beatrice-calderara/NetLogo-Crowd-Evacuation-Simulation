# NetLogo-Crowd-Evacuation-Simulation

# Crowd Evacuation Simulation

Agent-Based Model in NetLogo for simulating crowd evacuation during mass-event emergencies, with a focus on congestion, panic behavior, injuries, and environmental hazards. The project explores how individual behavior and arena conditions affect evacuation time, safety, and mortality.

## Overview

In emergency evacuations, the main danger often comes not only from the triggering event itself, but from crowd dynamics: bottlenecks at exits, herding behavior, panic, and the vulnerability of slower individuals can rapidly increase injuries and deaths.

This project models those dynamics through a NetLogo simulation of a crowd evacuating from an arena-like environment. The model also supports optional hazards such as broken glass, fire, and smoke, allowing the study of how environmental risks interact with human behavior under stress.

## Main Features

- Arena configurable in size, wall thickness, and exit layout 
- Population size from 1 to 30,000 agents 
- Demographic differentiation by age and gender 
- Heterogeneous movement speed and crowding vulnerability 
- Pre-alarm wandering phase based on weighted random walk 
- Evacuation phase with destination assignment and movement logic 
- Panic behavior with herding effects 
- Health and injury system inspired by the Abbreviated Injury Scale (AIS), with levels from 0 to 6 
- Optional hazards: broken glass, fire, and smoke 
- Real-time plots for evacuation progress, average speed, and injury distribution 

## Simulation Logic

Before the alarm, agents move freely in the arena through a weighted random walk, representing normal crowd movement without coordination or explicit goals.

When the alarm is triggered, agents enter evacuation mode and are assigned an exit. Aware agents choose the nearest gate, while unaware agents are assigned a random gate, creating different navigation behaviors and congestion patterns.

During evacuation, agents follow a priority-based movement strategy. They can move forward on free floor patches, detour around congestion, reroute from crowded exits, avoid nearby fire, and react to walls or obstacles when necessary.

Panicking agents do not always behave rationally. Depending on their panic level, they may abandon optimal movement and instead follow the crowd toward the most occupied neighboring patch, producing herding behavior and dangerous bottlenecks.

## Population Model

The simulation includes heterogeneous agents with demographic attributes that affect both speed and survivability.

- Female agents move one step per tick slower than male agents 
- Children move two steps per tick slower than adults 
- Elderly agents move three steps per tick slower than adults 
- Children suffer 3x crowding damage, elderly agents 2x, and adults 1x 
- These penalties are cumulative, making children and elderly individuals the most vulnerable categories in the model 

An optional setting can normalize movement speed so that every agent moves at one step per tick, independently of demographic differences.

## Damage Model

Each agent starts with a `health_state` of 100, which decreases over time according to the hazards encountered during the evacuation.

From this continuous value, the model derives a discrete injury level from 0 to 6, ranging from healthy to fatal. This injury representation is inspired by the Abbreviated Injury Scale (AIS) and is tracked through counters and plots during the simulation.

### Sources of Damage

#### 1. Crowding

Crowding is the main source of harm in the simulation. At each tick, an agent loses health proportionally to the number of other alive agents occupying the same patch, scaled by a global injury weight parameter.

#### 2. Broken Glass

When enabled, the floor is partially treated as covered with broken glass. At each movement step, agents can slip with probability `slipping_chance / 100`; if they slip, they lose 5% of their current health and do not move during that step.

#### 3. Fire and Smoke

When the fire hazard is enabled, a fire starts after a configurable delay and spreads stochastically using a probabilistic cellular automaton.

Agents standing on burning patches lose health directly, while nearby smoke causes inhalation damage and may also disorient them. If smoke density exceeds a threshold, an agent can lose its exit direction and be redirected toward a random gate.

Aware agents can partially counter fire through a deliberative layer driven by parameters such as `perception_radius` and `safety_weight`, while unaware agents only react locally and are more exposed to danger, especially under panic.

## Key Parameters

Some of the main configurable parameters in the model include:

- `scale`
- `wall_thickness`
- `real_exits`
- `population size`
- `demographic sliders`
- `speed_enabled`
- `injury_weight`
- `panic_fraction`
- `aware_fraction`
- `glass_bottles_switch`
- `slipping_chance`
- `fire_hazard_enabled`
- `fire_delay`
- `fire_spread_rate`
- `smoke_radius`
- `smoke_visibility_threshold`
- `perception_radius`
- `safety_weight`

## Output Metrics

The simulation tracks multiple outcome measures useful for analysis:

- Total evacuation time 
- Number of evacuated agents over time
- Average speed of alive agents 
- Injury-level distribution 
- Number of healthy, injured, and dead agents 

## How to Run

1. Install NetLogo [web:1].
2. Clone this repository.
3. Open the `.nlogo` model file in NetLogo.
4. Configure the parameters from the interface.
5. Run the simulation and observe the plots and counters.

## Research Goal

The goal of this project is to study the trade-off between evacuation efficiency and human safety. The model highlights that strategies minimizing evacuation time alone may unintentionally increase congestion, injuries, and deaths, especially in high-density scenarios.

## Possible Extensions

- Add social groups or family cohesion behavior.
- Model security staff or guided evacuation.
- Introduce multiple alarm types and false alarms.
- Add learning or adaptive routing.
- Export simulation statistics for batch experiments and reproducibility.

## Author

Beatrice Calderara 

