---
theme: default
highlighter: shiki
transition: none
mdc: true
defaults:
  layout: center
  transition: view-transition
---

# State of React Native macOS

RNL Conference 2025  
Saad Najmi & Jay Meistrich

---

# The last year has been busy

- Paying down tech debt across the platform
- Implementing the new architecture end-to-end

---

# Paying down tech debt

- Focus buckets: documentation and release stability
- Goal: create a place for the team and community to learn, ship, and contribute faster

---

# Documentation cleanup

- Docs historically lived at `microsoft.github.io/react-native-windows`
- That split site was awkward for macOS developers
- Consolidated everything under `microsoft.github.io/react-native-macos`

---

# Documentation refresh

- Captured tribal knowledge directly in the docs as we went
- Rebuilt the guidance with the new architecture in mind
- Added real macOS-centric examples instead of Windows carry-overs

---

# Stabilizing releases

- Updated the entire release pipeline while rolling out the new docs
- Every fix now lands with a doc update so it is searchable later
- Playbooks remove “ask the expert” bottlenecks

---

# Did it pay off?

- Yes — releases are boring again in the best way
- Issues are resolved faster because the answers already live in docs
- Contributors outside Microsoft can follow the same path we use internally

---

# New architecture

- Multi-year investment now landing on macOS
- Unlocks better performance, tooling, and reliability

---

# Three pillars

- **Turbo Modules** — shipped and working
- **Hermes** — mostly there, and now the default
- **Fabric** — still maturing, but usable today

---

# Hermes: catching up

- Repo had drifted from React Native core branches
- Main and release branches assumed latest Hermes
- “Adam on duty” to keep branches aligned with upstream

---

# Hermes: wins

- Hermes now works on every branch 0.74+
- Set as the default JavaScript engine on macOS
- Hermes unlocks React Native DevTools parity on desktop

---

# Fabric focus

- Partnered closely with Meta to land Fabric on macOS
- React Native macOS 0.71 → technical preview
- React Native macOS 0.81 → Fabric on by default

---

# Why Fabric

- Architecture is cleaner than the legacy Paper renderer
- Props are strongly typed instead of JSON blobs
- Less overhead crossing the React ↔ native bridge

---

# Fabric progress

- All macOS-specific props have been ported
- Core functionality is now in bug-squash mode
- Desktop parity with iOS unlocks shared component work

---

# Deep dive: Paper vs Fabric

- Paper passed JSON back and forth with macro-heavy Obj-C
- Fabric moves view logic to modern C++ structures
- No more JSON serialization when syncing props

---

# Deep dive: host platforms

- Host platform pattern makes adding `macos` straightforward
- Windows followed the same approach, validating the design
- Credit to Eric Rozelle for pioneering the pattern in RNW/RNM

---

# Finish

- Special thanks to Eric Rozell, Shawn Wanton, Devan Buggay, Nick Lefever
- Adam Gleitman, Julio Rocha, Tommy Nguyen, Andrew Coates
- Gabriel Donadel, Jamon Holmgren, Jamie Birch, Jay Meistrich
- We cannot wait to see what you build with React Native macOS
