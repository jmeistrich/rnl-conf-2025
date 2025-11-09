---
theme: default
highlighter: shiki
transition: none
mdc: true
defaults:
    layout: center
    transition: view-transition
---

# Why isn't everyone using this?

<div class="text-[72px] text-center">
🤔
</div>

<!--
I've been seeing these talks from Microsoft about React Native on desktop and it seemed cool, but I haven't heard much about other apps using it.

But then I tested it out just to see if LegendList would work in it, and I was blown away by how incredibly good it is.

I've been playing with it and making some apps, and it's seriously really good, though it has some rough edges.

I've been exploring all the benefits and downsides and I think it's now time to
 -->

---

# Take React Native on desktop seriously

<div class="text-[72px] text-center">
😎
</div>

<!--
take React Native on desktop seriously.

I got so excited about this that I joined Expo to explore if there's an opportunity to make desktop apps better.
 -->


---

# In 2025

- Version 0.81
- Fabric support
- New docs

<!--
With all the recent updates it's now just about caught up with the latest React Native version

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
But first we need to ground this in the current state of desktop apps.

When people are building a desktop app they have basically two choices: build it fully native or as a webview.

We've been through this in mobile before with Cordova, Ionic, etc... We all know how much better React Native apps are than those.
 -->

---

<img src="/media/tweet.png" class="max-h-[540px] rounded-lg" />

<!--
React Native is so good now that it can be indistinguishable from native apps.

MAYBE: I recently worked on the v0 app, which by using new architecture, Legend List, and react-native-keyboard-controller let us achieve such native feel that people can't even tell it's react native anymore.

So why do we accept webview apps on desktop?
-->

---

# Electron

<div class="flex flex-col lg:flex-row gap-6 mt-10 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col items-center gap-8 flex-2">
    <div class="text-2xl uppercase text-gray-400">Binary Size</div>
    <div class="text-4xl font-bold text-white">268&nbsp;MB</div>
    <div class="grid grid-cols-3 gap-4 w-full text-center">
      <div class="bg-white/10 rounded-2xl p-4 flex flex-col gap-2">
        <div class="text-sm uppercase text-gray-400 font-medium">Chromium</div>
        <div class="text-2xl font-semibold text-white">246 MB</div>
      </div>
      <div class="bg-white/10 rounded-2xl p-4 flex flex-col gap-2">
        <div class="text-sm uppercase text-gray-400 font-medium">Node.js</div>
        <div class="text-2xl font-semibold text-white">19 MB</div>
      </div>
      <div class="bg-white/10 rounded-2xl p-4 flex flex-col gap-2">
        <div class="text-sm uppercase text-gray-400 font-medium">Misc</div>
        <div class="text-2xl font-semibold text-white">3 MB</div>
      </div>
    </div>
  </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col items-center gap-8 flex-1">
    <div class="text-2xl uppercase text-gray-400 text-center">Memory</div>
    <div class="text-4xl font-bold text-white">83&nbsp;MB</div>
  </div>
</div>

TODO: Rebuild Electron after optimized

<!--
The most popular tool for building desktop apps is Electron. Electron builds in a full chromium browser and Node.js into every app. It even bundles in ffmpeg because chromium needs it, even if your app doesn't.

A Hello World app is 268 MB, runs 4 separate processes, and uses 83 MB of memory.
-->

---

<img src="/media/ipc.png" class="max-h-[540px] rounded-lg" />

<!--
The way it works is you have a renderer process for the webview, and a main process for the app itself that communicate through interprocess communcation. Then main process communicates through node.js to do system things.

So that adds a lot of DX and performance overhead.
 -->

---

# Tauri

<div class="flex flex-col lg:flex-row gap-6 mt-10 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col items-center gap-8 flex-2">
    <div class="text-2xl uppercase text-gray-400">Binary Size</div>
    <div class="text-4xl font-bold text-white">9&nbsp;MB</div>
  </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col items-center gap-8 flex-1">
    <div class="text-2xl uppercase text-gray-400 text-center">Memory</div>
    <div class="text-4xl font-bold text-white">64&nbsp;MB</div>
  </div>
</div>

<!--
The alternative to Electron is Tauri. It's also a webview app, but it uses the system webview so it doesn't bundle chromium and is much smaller.

But then you have to deal with platform differences of Safari on Mac vs. Edge on Windows, and old OS versions will have old webview versions.

And it uses Rust for the main process, which is great if you love Rust, but otherwise it's a whole other language to learn.

And it's still a webview.
-->

---

<div class="flex max-w-full gap-x-4 justify-center">
    <img src="/media/electronL.png" class="max-h-[460px] flex-1 rounded-lg" />
    <img src="/media/electronR.png" class="max-h-[416px] flex-1 rounded-lg" />
</div>

<!-- A lot of the apps you use every day are Electron apps.

Show names and icons of biggest apps along with file size.

I don't mean to throw shade on Electron or these apps. Electron is probably the best choice for most of them. It lets teams share more code across multiple apps and reuse their javascript knowledge. The alternative is building full native apps in Swift and C#, which takes a ton more resources.

This is just the world we live in where our desktop apps are web apps.

You can even see my own app, Legend, in the list there. That's part of why I'm so passionate about this, having dealt with it for years.
-->
---

<img src="/media/xcode.png" class="max-h-[300px] flex-1 rounded-lg" />

<!--
Of course the biggest app on my computer is not Electron, but that's a whole other thing.
-->


---

<img src="/media/downloads.png" class="max-h-[540px] rounded-lg" />

<!--
I've heard that nobody builds desktop apps anymore, but I don't think that's true. Electron is actually downloaded almost as much as Expo.

But these apps are huge, they use a ton of memory, and they don't feel as good as they should.

So I think it's time to…
-->

---

# Reverse the Enshittification

<div class="text-[72px] text-center">
💩 <span class="text-gray-300">→</span> 🔥
</div>

<!--
Reverse the enshittification
-->

---

# <div class="text-[60px]">📱 💩</div>

<div class="flex justify-center mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">Web</div>
    <div class="flex flex-wrap gap-4 justify-center">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/chrome/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/swift/swift-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/android/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
We've actually been here before on mobile. Native apps are more delightful but hard to build and need dedicated teams. So companies built cross-platform web apps across web and mobile. And everything was bad.
-->

---

# <div class="text-[60px]">📱 🔥</div>

<div class="flex justify-center gap-6 mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">Web</div>
    <div class="flex flex-wrap gap-4 justify-center">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/chrome/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
    </div>
    </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">React Native</div>
    <div class="flex gap-6">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/swift/swift-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/android/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
But then React Native came in and saved everyone with delightful cross-platform apps. And everything was good.
-->

---

# <div class="text-[60px]">🖥️ 💩</div>

<div class="flex justify-center gap-6 mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">Web</div>
    <div class="flex gap-4 justify-center">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/chrome/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
       <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/apple/apple-original.svg" alt="Mac icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Mac</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/windows8/windows8-original.svg" alt="Windows icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Windows</span>
      </div>
    </div>
    </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">React Native</div>
    <div class="flex gap-6">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/swift/swift-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/android/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
Now we're repeating history on desktop.

Companies are build cross-platform web apps across web and desktop, while using React Native on mobile.

And we have an opportunity again for React Native to come in and save everyone.
-->

---

# <div class="text-[60px]">🖥️ 🔥</div>

<div class="flex justify-center gap-6 mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">Web</div>
    <div class="flex gap-4 justify-center">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/chrome/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
    </div>
  </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">React Native</div>
    <div class="flex gap-6">
       <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/apple/apple-original.svg" alt="Mac icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Mac</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/windows8/windows8-original.svg" alt="Windows icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Windows</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/swift/swift-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/android/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>


<!--
So I propose since we're already building with these two separate technologies anyway, we make React Native the cross-platform solution on desktop as well.
-->

---

# <div class="text-[60px]">🖥️ 🔥🔥🔥</div>

<div class="flex justify-center gap-6 mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">React Native</div>
    <div class="flex gap-6">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/chrome/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
       <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/apple/apple-original.svg" alt="Mac icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Mac</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/windows8/windows8-original.svg" alt="Windows icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Windows</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/swift/swift-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/android/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
And with react-native-web getting better, maybe we could eventually get to React Native everywhere, but that's a whole other thing.

So the big question is why React Native on desktop?
-->

---

# React Native MacOS is awesome

<div class="flex flex-row justify-center gap-6 mt-10">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col items-center gap-8">
    <div class="text-2xl uppercase text-gray-400">Binary Size</div>
    <div class="text-4xl font-bold text-white">17&nbsp;MB</div>
  </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col items-center gap-8">
    <div class="text-2xl uppercase text-gray-400 text-center">Memory</div>
    <div class="text-4xl font-bold text-white">28&nbsp;MB</div>
  </div>
</div>

<!--
Because it's awesome.

Apps are tiny. They're fast because Hermes is very fast, native modules are fast, and they don't have to communicate across a webview bridge.

And because React Native is native, it can be as fast as a native app and easily use native features.
-->

---

# Small

<div class="mt-10 overflow-hidden rounded-3xl border border-white/10">
  <table class="w-full text-left text-base text-gray-200">
    <thead class="bg-white/5 text-gray-400 uppercase text-md">
      <tr>
        <th class="px-8 py-5 !font-medium" style="padding:20px 32px">Stack</th>
        <th class="px-8 py-5 !font-medium" style="padding:20px 32px">Size</th>
        <th class="px-8 py-5 !font-medium" style="padding:20px 32px">Memory</th>
      </tr>
    </thead>
    <tbody>
      <tr class="border-t border-white/5">
        <td class="px-8 py-5 text-lg text-white font-semibold" style="padding:20px 32px">Electron</td>
        <td class="px-8 py-5 text-xl font-medium text-white" style="padding:20px 32px">268&nbsp;MB</td>
        <td class="px-8 py-5 text-xl font-medium text-white" style="padding:20px 32px">83&nbsp;MB</td>
      </tr>
      <tr class="border-t border-white/5 bg-white/5">
        <td class="px-8 py-5 text-lg text-white font-semibold" style="padding:20px 32px">Tauri</td>
        <td class="px-8 py-5 text-xl font-medium text-white" style="padding:20px 32px">9&nbsp;MB</td>
        <td class="px-8 py-5 text-xl font-medium text-white" style="padding:20px 32px">64&nbsp;MB</td>
      </tr>
      <tr class="border-t border-white/5">
        <td class="px-8 py-5 text-lg text-white font-semibold" style="padding:20px 32px">React Native</td>
        <td class="px-8 py-5 text-xl font-medium text-white" style="padding:20px 32px">17&nbsp;MB</td>
        <td class="px-8 py-5 text-xl font-medium text-white" style="padding:20px 32px">28&nbsp;MB</td>
      </tr>
    </tbody>
  </table>
</div>

<!--
Resource usage is much lower with React Native. It doesn't include a full Chromium browser but it does include Hermes.

We all know size matters but it'what's more important is how the app feels.
-->

---

<SlidevVideo src="/media/photos.mov" autoreset="slide" autoplay mute loop  />

<!--
I vibe coded a photo library app to see what kind of performance I could get with it. The app opens instantly. LegendList powers a gallery view and timeline with no blanking. I made a shared element transition animation just using Animated which looks awesome. And the photo loading is so fast it looks like a movie.
-->

---

# Native

Video of liquid glass

<!--
And because it's native we can easily do native things.

Of course get liquid glass for free with native views.

TODO: Ask Saad for liquid glass video
-->

---

<SlidevVideo src="/media/windows.mov" autoreset="slide" autoplay mute loop  />

<!--
We can open multiple windows (ooooh) because it's not a webview. We can even use the platform native drag/drop behavior to drag between windows.

Clearly not many of you have used Electron because your mind isn't blown by this.
-->

---

<img src="/media/medium.png" class="max-h-[420px] rounded-lg" />

<!--
And you haven't read enough Medium articles about how to postMessage between the isolated Chromium processes running multiple windows.

In React Native this is just easy.
-->

---

<SlidevVideo src="/media/overlay1.mov" autoreset="slide" autoplay mute loop  />

<!--
We can even do cool animations on windows. When the song changes this overlay at the top of the screen unblurs and fades in, and blurs and fades out.
-->

---

<SlidevVideo src="/media/overlayblur.mov" autoreset="slide" autoplay mute loop  />

<!--
Let's look at that in slow motion to see how cool it is. Codex made me a native blur filter in Swift to do that in a few minutes.
-->

---

<SlidevVideo src="/media/overlay2.mov" autoreset="slide" autoplay mute loop  />

<!--
Multiple windows can easily share state. This actually just happened and I never thought about it, because they're both subscribed to the same global state.

Again in the alternatives you have to postMessage between isolated chromium processes to do this.

Now let's step it up a notch. React Native Skia has mac support.
-->

---

<SlidevVideo src="/media/visbar.mov" autoreset="slide" autoplay mute loop  />

<!--
So we can display this dope visualizer by updating our native audio module to tap into the raw frequency data. That's pretty cool, but let's step it up a notch.
-->

---

<SlidevVideo src="/media/visaurora.mov" autoreset="slide" autoplay mute loop  />

<!--
We can build visualizers with shaders in skia, so we can display it as this cool glowing radial cloud.

Or we could step it up another notch and go full 3D.
-->

---

<SlidevVideo src="/media/viscube.mov" autoreset="slide" autoplay mute loop  />

<!--
We can make full 3D visualizers using the power of the GPU. And why not, this is a native app.
-->

---

<img src="/media/musicsize.png" class="max-h-[320px] rounded-lg" />

<!--
This complete app using Nativewind, a bunch of libraries and expo modules, and skia comes out to 35 MB. Zipped, it's an 11 MB download.
-->

---

# CPU

<div class="flex items-center justify-center gap-x-8 mt-10">
    <div class="mt-10 overflow-hidden rounded-3xl border border-white/10">
    <table class="w-full text-left text-base text-gray-200">
        <thead class="bg-white/5 text-gray-400 uppercase text-md">
        <tr>
            <th class="px-4 py-5 !font-medium" style="padding:20px 24px">App</th>
            <th class="px-4 py-5 !font-medium" style="padding:20px 24px">CPU</th>
        </tr>
        </thead>
        <tbody>
        <tr class="border-t border-white/5">
            <td class="px-4 py-5 text-lg text-white font-semibold" style="padding:20px 24px">Spotify</td>
            <td class="px-4 py-5 text-xl font-medium text-white" style="padding:20px 24px">26%</td>
        </tr>
        <tr class="border-t border-white/5 bg-white/5">
            <td class="px-4 py-5 text-lg text-white font-semibold" style="padding:20px 24px">Music</td>
            <td class="px-4 py-5 text-xl font-medium text-white" style="padding:20px 24px">9%</td>
        </tr>
        <tr class="border-t border-white/5">
            <td class="px-4 py-5 text-lg text-white font-semibold" style="padding:20px 24px">React Native</td>
            <td class="px-4 py-5 text-xl font-medium text-white" style="padding:20px 24px">9%</td>
        </tr>
        </tbody>
    </table>
    </div>
    <img src="/media/musiccpu.png" class="max-h-[320px] rounded-lg" />
</div>

<!--
Compared to other music apps, playing a local mp3 file uses much less CPU. Since React Native is just running native code, it's effectively the same as native CPU usage. Spotify as an Electron app is running a full Chrome browser and playing through the web platform, so it does a lot more work.
-->

---

# Memory

<div class="flex items-center justify-center gap-x-8 mt-10">
    <div class="mt-10 overflow-hidden rounded-3xl border border-white/10">
    <table class="w-full text-left text-base text-gray-200">
        <thead class="bg-white/5 text-gray-400 uppercase text-md">
        <tr>
            <th class="px-4 py-5 !font-medium" style="padding:20px 24px">App</th>
            <th class="px-4 py-5 !font-medium" style="padding:20px 24px">Memory</th>
        </tr>
        </thead>
        <tbody>
        <tr class="border-t border-white/5">
            <td class="px-4 py-5 text-lg text-white font-semibold" style="padding:20px 24px">Spotify</td>
            <td class="px-4 py-5 text-xl font-medium text-white" style="padding:20px 24px">687 MB</td>
        </tr>
        <tr class="border-t border-white/5 bg-white/5">
            <td class="px-4 py-5 text-lg text-white font-semibold" style="padding:20px 24px">Music</td>
            <td class="px-4 py-5 text-xl font-medium text-white" style="padding:20px 24px">172 MB</td>
        </tr>
        <tr class="border-t border-white/5">
            <td class="px-4 py-5 text-lg text-white font-semibold" style="padding:20px 24px">React Native</td>
            <td class="px-4 py-5 text-xl font-medium text-white" style="padding:20px 24px">155 MB</td>
        </tr>
        </tbody>
    </table>
    </div>
    <img src="/media/musicmemory.png" class="max-h-[320px] rounded-lg" />
</div>

<!--
And comparing memory usage while playing a song file is even more ludicrous. Running a full browser with 6 different processes is taking 687 MB. React Native takes 20% of that.

It even beats Apple Music, the most native of native apps.

TODO: This used to be 90. What happened?
-->


---

# Familiar DX

<!--
And because it's React Native we get the great developer experience we're used to.

Of course Fast Refresh works, so it's easy to tweak the design.
-->

---

<img src="/media/devtools1.png" class="max-h-[560px] rounded-lg" />

<!--
We get the devtools to inspect the component tree and the nice new performance tab to optimize everything.
 -->

---

<img src="/media/devtools2.png" class="max-h-[560px] rounded-lg" />

<!--
We get the devtools to inspect the component tree and the nice new performance tab to optimize everything.
 -->

---

# Native modules

<!--
I don't know how to write native modules, but if I want to do something natively, I just ask AI to make me a native module. And then my javascript code can do native things. I don't have to manage the communication across the IPC and call it from the main process, then post it back. It's just functions I can call from anywhere.
-->

---

# Storybook

<!--
We can even use React Native Storybook to work with our component system.
-->



---

# RN Ecosystem

- @gorhom/portal
- Async Storage
- Gesture Handler
- Legend List
- MMKV
- Nativewind
- React Native Clipboard
- React Native SVG
- React Native Webview
- Reanimated
- Skia
- WebGPU

<!--
And we get access to the huge ecosystem of React Native libraries.

But this is where we get into both pros and cons. JS-only libraries mostly just work. And a lot of the major native libraries already support macOS. But a lot don't yet.
-->

---

# Some Expo Libraries

- @expo/log-box
- expo-asset
- expo-constants
- expo-crypto
- expo-eas-client
- expo-file-system
- expo-font
- expo-keep-awake
- expo-linking
- expo-local-authentication
- expo-manifests
- expo-mesh-gradient
- expo-modules-core
- expo-sqlite
- expo-updates
- expo-web-browser

<!--
A lot of Expo modules work on mac already. I specifically love expo file system, it's super fast. But there's a lot that aren't supported yet.
-->

---

# Desktop Features need libraries

- Capture screen / other apps
- Context menus
- Dock icon
- Dropdowns
- Keyboard handling
- Media playback
- Menu bar
- Processes
- Stoplight buttons
- Window management

<!--
And there's a whole ton of new libraries that need to be built for desktop specific features that don't exist on mobile.
 -->

---

# Downsides

- Expo CLI doesn't support macos (yet)
- Some libraries don't work on macos
- Desktop specific functionality needs native modules

<!--
So there are still some downsides and rough edges.

It's not supported in the Expo CLI so it needs to be built separately.

Some libraries don't work on macOS yet and there's a bunch of desktop specific features which don't have libraries for them yet.

But, AI is very good at building native modules. I don't really know how to write native modules, but AIs have done a good enough job that it's not really a problem for me.
-->

---

# <div class="text-[144px]">🐣</div>

<!--
So we've got a chicken and egg problem here. There's not a lot of people making React Native desktop apps, so libraries aren't bothering to add support. So it's hard to build apps because of a small ecosystem.

So while I would very much recommend react-native-macos to all of you legends, I think we need to smooth out some rough edges and support in more libraries before I'd suggest it to new developers.

So let's crack some eggs and get this ecosystem going.
-->


---

# Take React Native on desktop seriously

- Add macos support to your libraries

- Try making apps

- Talk to me

<!--
Talk to me if you're interested and I'll help or connect you to the right people
 -->


<!--
TODO:
Get correct icon for ios
Download icons to media folder
 -->
