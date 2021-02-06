---
layout: post
title:      "Dynamic Reducer Actions in Redux"
date:       2021-02-06 20:05:10 +0000
permalink:  dynamic_reducer_actions_in_redux
---


Redux is great. It eases the complexity of passing information up down and all around the Component trees of React and allows global state to be held and modified all in one place. The `case` statements in the reducer file control the changes in global state, but what if you want to impliment several related cases?

In my Ship-Interface application, the program keeps track of a theoretical space-ship, and tacks the status of its various parts (Hull, Navigation, and Weapons). The reducer for the ship only needed a single object, Ship, with key value pairs of the different subsystems and their statuses. Changing each system's status was going to look the same as all the others, so there was no need to impliment cases for `APPLY_HULL_DAMAGE`, `APPLY_NAV_DAMAGE`, and `APPLY_WEAPONS_DAMAGE`, if only I could create an `APPLY_DAMAGE` that would know which system to affect. The `APPLY_DAMAGE` action needed to change the correct system's damage from "green" to "yellow" or from "yellow" to "red" (`APPLY_REPAIR` would handle the movement in the other direction).

In order for this to work, the reducer needs to know the correct system, which can be passed to it as `payload` within the action, so the dispatch function ends up being

```applyDamage: (system) => dispatch({type: 'APPLY_DAMAGE', payload: system})```

Which then gets passed to the `<Buttons/>` component to be used on button click. The button passes `"hull", "nav", or "weapons"` to dispatch, which gives the reducer access.

Okay, so the reducer has access to the correct system, not we have to use it. First, let's assign our key as a variable `dKey`

```let dKey = action.payload```

Now, we need to check what value is assigned to the key which equals `dKey`. Handily, we can do that with square bracket notation.

```if(state.ship.[dKey] === "green"){...```

Without square bracket notation, the computer would be trying to find a key of `dKey` in the Ship object, and failing. By passing a variable through that way, we avoid having to write out separate actions for damaging each of the three systems. Instead, we are left with one action, rather than three.
