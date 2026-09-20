## \# Modern Dormer Configurator

## 

## !\[Modern Dormer Configurator](Doc/Modern\_Dormer\_Hero.png)

## 

## \*\*Interactive Technical 3D configurator created as a Technical 3D Artist assignment.\*\*

## 

## \*\*Tools:\*\* Blender · Verge3D · Webflow

## 

## \## 🚀 Live Demo

## 

## ▶ \*\*\[Open Interactive Configurator](https://furniture-ui.webflow.io/)\*\*

## 

## > Best viewed on desktop for the full interactive 3D experience.

## 

## \## Project OverviewProject Overview

Modern Dormer Configurator is an interactive 3D prototype that allows
users to configure a dormer in real time. The project focuses on modular
3D construction, clear configuration logic, animation, material
customization, and browser-based interaction.

The 3D scene and interactive logic were built with **Blender** and
**Verge3D**, while the final user interface was created in **Webflow**
and connected to Verge3D through `window.postMessage()`.

## Main Features

* Overall Dormer Width: **1800 / 2200 / 2600 mm**
* Overall Dormer Height: **2000 / 2200 / 2400 mm**
* Front Window: **Single / Double / Triple**
* Side Windows: **None / Left / Right / Both**
* Window orientation switching
* Horizontal / Vertical cladding
* Cladding material customization
* Window-frame color customization
* Turn / Tilt window animation
* Direct window interaction
* Real-time 3D preview

> \*\*Dimension Note:\*\* Width and Height values represent the \*\*overall
> dormer dimensions\*\*, based on the dimensions provided in the technical
> drawing. They do not represent only the internal frame or cladding
> area.

## Technical Approach

The configurator uses a **modular variant system** rather than
generating geometry procedurally at runtime.

Shared components are reused whenever possible, while geometry that
genuinely changes between configurations is prepared as separate
variants in Blender. Verge3D Puzzles control visibility, transforms,
morph targets, materials, and animation.

This keeps the prototype predictable, easier to debug, and suitable for
a predefined configuration set.

### Configuration State

The main configuration is controlled through independent state values:

* `current\_width`
* `current\_Height`
* `Side\_Option`
* `Cladding\_Type`
* `Front\_window`

Each UI control changes only the state that it owns, then the scene is
refreshed using the current combination of states. This prevents one
configuration option from unintentionally overwriting another.

## Front Window Logic

Dormer Width   Available Front Windows

\---

1800 mm        Single
2200 mm        Single / Double
2600 mm        Single / Double / Triple

Morph targets are used for the front cladding geometry, while the
appropriate window object is shown for the selected configuration.

## Window Animation

The windows use a tilt-and-turn style interaction.

* **Turn:** frames 1--24
* **Tilt:** frames 48--72
* Global controls animate the available windows.
* Individual windows can also be clicked directly.

Each window variant uses its own armature to avoid unintended
dependencies between configurations.

## Materials

**Cladding:** Brown · White · Oak

**Window Frame:** Black · Anthracite · White

## Webflow ↔ Verge3D Integration

``` text
Webflow UI
    |
    | window.postMessage()
    v
Verge3D Message Receiver
    |
    | triggers hidden internal controls
    v
Verge3D Puzzles
    |
    v
3D Scene
```

The visible Webflow controls send commands to the Verge3D iframe.
Verge3D receives each command and triggers the corresponding internal
control, allowing the existing Puzzle logic to remain stable.

## Problem-Solving Notes

### 1\. Inconsistent Technical References

Some drawing views contained details that did not fully match. The
clearest front and side views were treated as the primary references,
while explicit dimensions remained authoritative. Unspecified details
were estimated only where necessary to maintain a visually consistent
prototype.

### 2\. Increasing Configuration Complexity

Combining width, height, windows, side options, cladding, materials, and
animation quickly increased the number of possible states. The solution
was to separate the configuration into independent state dimensions
rather than creating a unique state for every combination.

### 3\. Blender → Verge3D Hierarchy

Some parented objects behaved differently after export because parent
and child transforms could be applied redundantly. The hierarchy was
simplified and transforms were controlled at the appropriate object
level.

### 4\. Hidden Armature Dependency

A window variant moved unexpectedly even though it had no parent. The
actual dependency came from an Armature Modifier referencing another
variant's armature. Each variant was changed to use its own armature.

**Lesson:** No parent does not necessarily mean no dependency.
Modifiers, constraints, drivers, and animation relationships also need
to be checked.

### 5\. Configuration State Problem

Categorical state values initially behaved incorrectly when
uninitialized variables were used as constants. Literal text values such
as `"Horizontal"`, `"Vertical"`, `"Left"`, `"Right"`, and `"Both"` were
used instead.

### 6\. State Ownership

Earlier logic allowed one control to change state belonging to another
system. The final architecture gives each control responsibility for
only its own state dimension.

### 7\. Webflow ↔ Verge3D Integration

Because Webflow and Verge3D run across an iframe boundary,
`window.postMessage()` is used as a simple communication layer between
the external UI and the 3D application.

### 8\. Browser Cache During Testing

Updated Verge3D logic sometimes appeared not to work even though the
generated JavaScript was correct. A hard refresh loaded the current
published version.

**Lesson:** Verify browser cache and the loaded version before changing
working logic.

## Testing

The final prototype was tested across:

* All **9 Width × Height** combinations
* Front-window options allowed by each width
* Side Window: None / Left / Right / Both
* Horizontal ↔ Vertical cladding
* All cladding materials
* All window-frame colors
* Turn / Tilt animation
* Direct window interaction
* Configuration changes after animation and material changes
* Published Webflow ↔ Verge3D communication

**Final default state:** 1800 × 2000 / Single Front / Both Side /
Horizontal / Brown / Black

## Project Structure

``` text
Modern\_Dormer/
├─ assets/
├─ media/
├─ Doc/
│  ├─ Modern\_Dormer\_Hero.png
│  └─ Puzzle/
├─ Technical\_Assignment\_Colengo\_V001.blend
├─ Technical\_Assignment\_Colengo\_V001.gltf
├─ Technical\_Assignment\_Colengo\_V001.bin
├─ Technical\_Assignment\_Colengo\_V001.html
├─ Technical\_Assignment\_Colengo\_V001.css
├─ Technical\_Assignment\_Colengo\_V001.js
├─ visual\_logic.js
└─ visual\_logic.xml
```

> Company-provided technical reference documents are intentionally not
> included in the public repository.

## What I Learned

This assignment showed me how quickly an interactive 3D configurator can
become complex when several configuration systems need to work together.

The most important lesson was not only how to build each feature, but
how to structure the logic so that **geometry, state, animation,
materials, and UI remain independent and predictable**.

It also gave me practical experience connecting a Blender/Verge3D
application to an external Webflow interface and debugging issues across
the complete pipeline.

\---

**Created with Blender · Verge3D · Webflow**

