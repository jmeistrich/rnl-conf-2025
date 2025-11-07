---
theme: default
highlighter: shiki
transition: none
mdc: true
defaults:
    layout: center
    transition: view-transition
---

<div class="pb-12">

# Take React Native on Desktop Seriously

# Legend Apps + Expo

</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div class="flex gap-x-5">
    <div>𝕏 @jmeistrich</div>
    <div>🦋 @jayz.us</div>
  </div>
  <div class="text-gray-400">React Native Live — 2025</div>
</div>

---

# Why isn't everyone using this?

<!--
I've been seeing these talks from Microsoft about react native on desktop and it seemed cool, but I haven't heard much about other apps using it.

But then I tested it out just to see if LegendList would work in it, and I was blown away by how incredibly good it is.

I've been playing with it and making some apps, and it's seriously really good, though it has some rough edges.

I got so excited I joined Expo to explore if there's an opportunity to make desktop apps better.

I've been exploring all the benefits and downsides and I think it's now time to take React Native on desktop seriously.
 -->

---

# Take React Native on desktop seriously

## In 2025

- Version 0.81
- Fabric support
- New docs

<!--
It's now just about caught up with the latest React Native version

TOOD: Tiny recap of what Saad just said
 -->

---

# It's really good

- Small binaries
- Really fast
- Low CPU and memory usage


---

# The state of desktop

<div class="grid grid-cols-2 gap-12 mt-10 text-left">
  <div class="bg-white/5 rounded-2xl p-6 flex flex-col items-center gap-8">
    <div class="text-xl font-medium uppercase text-gray-400">Native</div>
    <div class="flex items-stretch justify-center gap-4 w-full">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-xl p-4">
        <div class="text-sm uppercase text-gray-400 pb-2">macOS app</div>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/swift/swift-original.svg" alt="Swift logo" class="size-16 object-contain" />
        <span class="text-base text-gray-200 text-center">Swift / Objective-C</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-xl p-4">
        <div class="text-sm uppercase text-gray-400 pb-2">Windows app</div>
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/csharp/csharp-original.svg" alt="C# logo" class="size-16 object-contain" />
        <span class="text-base text-gray-200 text-center">C# / .NET</span>
      </div>
    </div>
  </div>
  <div class="bg-white/5 rounded-2xl p-6 flex flex-col items-center gap-8">
    <div class="text-xl font-medium uppercase text-gray-400">Web</div>
    <div class="flex flex-col items-center gap-3 bg-white/10 rounded-xl p-4 w-full">
      <div class="text-sm uppercase text-gray-400 pb-2">Mac / Windows app</div>
      <div class="flex items-center justify-center gap-10">
        <div class="flex flex-col items-center gap-2">
          <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/electron/electron-original.svg" alt="Electron logo" class="size-16 object-contain" />
          <span class="text-base text-gray-200">Electron</span>
        </div>
        <div class="flex flex-col items-center gap-2">
          <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/tauri/tauri-original.svg" alt="Tauri logo" class="size-16 object-contain" />
          <span class="text-base text-gray-200">Tauri</span>
        </div>
      </div>
    </div>
  </div>
</div>

<!--
But first we need to ground this on the current state of desktop apps.

When people are building a desktop app they have basically two choices: build it fully native or as a webview.

We've been through this in mobile before with Cordova, Ionic, etc... We all know how much better React Native apps are than those. React Native is so good now that it can be indistinguishable from native apps.
 -->

---

Show a tweet about v0? https://x.com/vashishtaditya_/status/1985370323416694833

So why do we accept webview apps on desktop?

---

# Electron

The most popular tool for building desktop apps is Electron. Electron builds in a full chromium browser and Node.js into every app. It even bundles in ffmpeg because chromium needs it, even if your app doesn't.

A Hello World app is 280 MB, runs 4 separate processes, and uses X memory.

A lot of the apps you use every day are Electron apps.

Show names and icons of biggest apps along with file size.

Surprisingly the biggest one is not Electron, but that's a whole other thing (XCode).

These apps are huge, they use a ton of memory, and they don't feel as good as they should.

I've heard that nobody builds desktop apps anymore, but I don't think that's true. Electron is actually downloaded almost as much as Expo.

So it's time to…

---

# Reverse the Enshittification

Poop emoji -> fire emoji.

We're about to repeat the same situation on desktop that we had on mobile. Native apps are delightful but hard to build and need dedicated teams. So companies build cross-platform web apps. And React Native comes in to save everyone with delightful cross-platform apps.

Currently a company will build: Web, Mac/Windows (web), iOS/Android (RN).

But I propose we shift it to: Web, Mac/Windows (RN), iOS/Android (RN).

Or maybe even RN everywhere: Web (RN), Mac/Windows (RN), iOS/Android (RN).

Why?

---

# React Native MacOS is awesome

Binary size (compare hello world).

Memory usage.

CPU usage.

Startup time.

Apps are fast AF.

---

# Faster than native?

Demo: Legend Photos to show all that + speed.

---

# Things web can't do

Because it's native we can easily do native things.

Multiple windows (oooh).

Music skia visualizer.

Music blur popup.

Does all that with significantly better resource usage (cpu + memory usage while playing spotify vs. LM).

---

# DX

Familiar RN experience.

Fast refresh.

devtools.

Multi-window is easy. In Electron you have to post message between processes. In RN it's just regular state.

native code easier: In Electron and Tauri you have a renderer and main process and they use IPC to communicate with each other. So you have to manage the communication across the IPC and call it from the main process, then post it back. In React Native you just invoke a native module from wherever.

Use image from: https://tauri.app/concept/inter-process-communication/

react-native-storybook.

---

# Libraries

expo-file-system/next super fast.

Some not all expo modules:

@expo/log-box
expo-asset
expo-constants
expo-crypto
expo-eas-client
expo-file-system
expo-font
expo-keep-awake
expo-linking
expo-local-authentication
expo-manifests
expo-mesh-gradient
expo-modules-core
expo-sqlite
expo-updates
expo-web-browser

reanimated / gesture handler.

react-native-webview.

Legend List for fast lists.

skia and webgpu.

Ask Margelo.

MMKV - not sure if it works?

Nitro modules?

https://microsoft.github.io/react-native-macos/docs/guides/native-development#examples-of-community-modules-with-macos-support

---

# Downsides

Not all modules work on macos.

Desktop specific functionality needs native modules.

But, AI is very good at building native modules. I don't really know how to write native modules, but that's been actually pretty easy for me.

Expo CLI doesn't support macos (yet).

We've got a chicken and egg problem. Not enough people are trying to make apps so libraries aren't bothering to add support. So let's crack some eggs and get this ecosystem going.

---

# Take React Native on desktop seriously

Add macos support to your libraries.

Try making apps.

Talk to me if you're interested and I'll help or connect you to the right people.

---

<div class="bg-white absolute inset-0 flex items-center justify-center">
    <img src="/media/endpage.001.png" class="max-h-[520px]" />
</div>
