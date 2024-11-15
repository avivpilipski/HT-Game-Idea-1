#Minimum Viable Product

##Features

### Score Tracker (P1)
### Monkey can grab onto vines (P0)
### jungle background + art for monkey and vines (P1)
### Losing the game (P0)
### You are able to start a game (P0)
### basic physics (P0)

## Score Tracker
**Priority:** P1
**Implementation Timeline:** Day 3-4

**Core Requirements:**
- Arithmetic
- Tracking monkey's horizontal velocity at all times

**Simplifications:**
- With bad physics, the monkey's horizontal velocity won't make complete sense


## Monkey can grab onto vines
**Priority:** P0
**Implementation Timeline:** Day 1-2

**Core Requirements:**
- Collision detection with vines
- 

## jungle background + art for monkey and vines
**Priority:** P1
**Implementation Timeline:** Day 3-4

**Dependencies:**
- Monkey and vines... exist


## Losing the game
**Priority:** P0
**Implementation Timeline:** Day 1-2

**Core Requirements:**
- Collision detection with ground

**Simplifications:**
- No obstacles

## Starting the game
**Priority:** P0
**Implementation Timeline:** Day 1-2

**Core Requirements:**
- Game exists


## Basic Physics
**Priority:** P0
**Implementation Timeline:** Day 1-2

**Core Requirements:**
- Collision detection with ground

**Dependencies:**
- Grabbing into vines
- Collision detection

**Simplification:**
- Not good physics. Hopefully will add some good physics later



## MVP Implementation Plan

### Day 1-2 (Core Framework)
- Starting the game
- Losing the game
- Monkey can grab onto vines

### Day 3-4 (Essential Features)
- basic physics (gravity, intial velocity of monkey based on vine angle and speed, etc)
- Score tracker
- background + art

### Day 5 (Enhancement & Testing)
- randomness in vines' swing speed and length and distance between each other
- Final testing and refinement