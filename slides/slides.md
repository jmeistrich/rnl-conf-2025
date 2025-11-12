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
🤗
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

TODO: Tiny recap of what Saad just said
-->

---

# It's really good

- Small binaries
- Really fast
- Low CPU and memory usage
- Familiar RN DX

<!--
And it's just really good. It makes tiny fast apps and it's that familiar React Native dev experience that we all love.
-->

---

# The state of desktop

<div class="grid grid-cols-2 gap-12 mt-10 text-left">
  <div class="bg-white/5 rounded-2xl p-6 flex flex-col items-center gap-8">
    <div class="text-xl font-medium uppercase text-gray-400">Native</div>
    <div class="flex items-stretch justify-center gap-4 w-full">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-xl p-4">
        <div class="text-sm font-medium uppercase text-gray-400 pb-2">macOS app</div>
        <img src="/media/swift-original.svg" alt="Swift logo" class="size-16 object-contain" />
        <span class="text-base text-gray-200 text-center">Swift / Objective-C</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-xl p-4">
        <div class="text-sm font-medium uppercase text-gray-400 pb-2">Windows app</div>
        <img src="/media/csharp-original.svg" alt="C# logo" class="size-16 object-contain" />
        <span class="text-base text-gray-200 text-center">C# / .NET</span>
      </div>
    </div>
  </div>
  <div class="bg-white/5 rounded-2xl p-6 flex flex-col items-center gap-8">
    <div class="text-xl font-medium uppercase text-gray-400">Web</div>
    <div class="flex flex-col items-center gap-3 bg-white/10 rounded-xl p-4 w-full">
      <div class="text-sm font-medium uppercase text-gray-400 pb-2">Mac / Windows app</div>
      <div class="flex items-center justify-center gap-10">
        <div class="flex flex-col items-center gap-2">
          <img src="/media/electron-original.svg" alt="Electron logo" class="size-16 object-contain" />
          <span class="text-base text-gray-200">Electron</span>
        </div>
        <div class="flex flex-col items-center gap-2">
          <img src="/media/tauri-original.svg" alt="Tauri logo" class="size-16 object-contain" />
          <span class="text-base text-gray-200">Tauri</span>
        </div>
      </div>
    </div>
  </div>
</div>

<!--
But first we need to ground this in the current state of desktop apps.

When people are building a desktop app they have basically two choices: build it fully native or as a webview.

Going fully native requires building two entirely new apps, on top of their existing web and mobile apps, so most will choose the webview compromise. That's usually the best decision, to be able to actually release apps on every platform.

But it's a big compromise, and users are punished with subpar web apps pretending to be desktop apps.
-->

---

# Electron

<div class="flex flex-row gap-6 mt-10 w-full">
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

<!--
The most popular tool for building desktop apps is Electron. Electron builds in a full chromium browser and Node.js into every app. It even bundles in ffmpeg because chromium needs it, even if your app doesn't.

A Hello World app is 268 MB, runs 4 separate processes, and uses 83 MB of memory.
-->

---

<img src="/media/ipc.png" class="max-h-[540px] rounded-lg" />

<!--
The way it works is you have a renderer process for the webview, and a main process for the app itself that communicate through interprocess communcation. Then main process then runs through node.js to do system things, and passes the result all the way back up the bridges.

And that adds a lot of DX and performance overhead.
-->

---

# IPC

<style>
#slide-ipc .slidev-code-wrapper {
    width: auto !important;
}
#slide-ipc .slidev-code-wrapper pre {
    padding-right: 32px !important;
}
</style>
<div id="slide-ipc" class="flex gap-x-12 mt-16">
<div>
<h3>Web Renderer</h3>

```js
const { ipcRenderer } = require('electron')

const openFile = (filename) => {
  const json = JSON.stringify({ filename })
  ipcRenderer.send("open-file", json)
}
```

</div>

<div>
<h3>Main Renderer</h3>

```js
const { ipcMain, shell } = require('electron')
const { shell } = require('electron')

ipcMain.on('open-file', (event, json) => {
  const data = JSON.parse(json)
  const filename = data.filename
  shell.openExternal(filename)
});
```

</div>
</div>

<!--
This is a simple example of how you run native code. You have to postMessage from the web process over to the main process, stringify and parse your data across the bridge, then run your native code.

Sending back a response is even more complicated and not worth going into here. But let me just tell you, it's not fun.
-->

---

# Tauri

<div class="flex flex-row gap-6 mt-10 w-full">
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
The newer alternative to Electron is Tauri. It's also a webview app, but it uses the system webview so it doesn't bundle chromium and is much smaller.

But then you have to deal with platform differences of Safari on Mac vs. Edge on Windows, and old OS versions will have old webview versions.

And it uses Rust for code in the main process, which is great if you love Rust, but otherwise it's a whole other language to learn.

And it's still a webview.
-->

---

<div class="flex max-w-full gap-x-4 justify-center">
    <img src="/media/electronL.png" class="max-h-[460px] flex-1 rounded-lg" />
    <img src="/media/electronR.png" class="max-h-[416px] flex-1 rounded-lg" />
</div>

<!--
A lot of the apps you use every day are Electron apps.

I don't mean to throw shade on Electron or these apps. Electron is probably the best choice for most of them. If they had to build two whole other apps in two other lanugages, they would probably just never ship desktop apps.

So this is just the world we live in where our desktop apps are web apps.

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
I've been hearing that nobody builds desktop apps anymore, but I don't think that's true. Electron is actually downloaded almost as much as Expo.

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
        <img src="/media/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/apple-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
We've been through this in mobile before with Cordova, Ionic, etc... Native apps are more delightful but hard to build and need dedicated teams. So companies built cross-platform web apps across web and mobile. And everything was bad.
-->

---

# <div class="text-[60px]">📱 🔥</div>

<div class="flex justify-center gap-6 mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">Web</div>
    <div class="flex flex-wrap gap-4 justify-center">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
    </div>
    </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">React Native</div>
    <div class="flex gap-6">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/apple-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
But then React Native came in and saved everyone with delightful cross-platform apps. And everything was good.

TODO: Well it wasn't good for a while but now it's good
-->

---

<img src="/media/tweet.png" class="max-h-[540px] rounded-lg" />

<!--
And React Native is so good now that our apps can be indistinguishable from native apps.

But now we've repeated history again on desktop.
-->



---

# <div class="text-[60px]">🖥️ 💩</div>

<div class="flex justify-center gap-6 mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">Web</div>
    <div class="flex gap-4 justify-center">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
       <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/finder-icon.png" alt="Mac icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Mac</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/windows8-original.svg" alt="Windows icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Windows</span>
      </div>
    </div>
    </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">React Native</div>
    <div class="flex gap-6">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/apple-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
Companies are building cross-platform web apps across web and desktop, while using React Native on mobile.

And we have an opportunity again for React Native to come in and save everyone.
-->

---

# <div class="text-[60px]">🖥️ 🔥</div>

<div class="flex justify-center gap-6 mt-16 w-full">
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">Web</div>
    <div class="flex gap-4 justify-center">
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
    </div>
  </div>
  <div class="bg-white/5 rounded-3xl p-8 flex flex-col gap-6 w-full max-w-4xl">
    <div class="text-2xl font-medium uppercase text-gray-300 text-center">React Native</div>
    <div class="flex gap-6">
       <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/finder-icon.png" alt="Mac icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Mac</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/windows8-original.svg" alt="Windows icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Windows</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/apple-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
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
        <img src="/media/chrome-original.svg" alt="Web icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Web</span>
      </div>
       <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/finder-icon.png" alt="Mac icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Mac</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/windows8-original.svg" alt="Windows icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Windows</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/apple-original.svg" alt="iOS icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">iOS</span>
      </div>
      <div class="flex flex-col items-center gap-3 bg-white/10 rounded-2xl px-6 py-5 min-w-[140px]">
        <img src="/media/android-original.svg" alt="Android icon" class="size-12 object-contain drop-shadow" />
        <span class="text-lg font-medium text-white">Android</span>
      </div>
    </div>
  </div>
</div>

<!--
And with react-native-web getting better, maybe we could eventually get to React Native everywhere, but that's a whole other thing.

So the big question is why React Native on desktop?

TODO: "The react native on web story"
-->

---

# React Native macOS is awesome

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

Apps are tiny. They're fast because Hermes is fast, native modules are fast, and they don't have to communicate across a webview bridge.

And because React Native is native, it can be as fast as a native app and easily use native features.
-->

---

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
        <td class="px-8 py-5 text-2xl text-white font-semibold" style="padding:20px 32px">Electron</td>
        <td class="px-8 py-5 text-2xl font-medium text-white" style="padding:20px 32px">268&nbsp;MB</td>
        <td class="px-8 py-5 text-2xl font-medium text-white" style="padding:20px 32px">83&nbsp;MB</td>
      </tr>
      <tr class="border-t border-white/5 bg-white/5">
        <td class="px-8 py-5 text-2xl text-white font-semibold" style="padding:20px 32px">Tauri</td>
        <td class="px-8 py-5 text-2xl font-medium text-white" style="padding:20px 32px">9&nbsp;MB</td>
        <td class="px-8 py-5 text-2xl font-medium text-white" style="padding:20px 32px">64&nbsp;MB</td>
      </tr>
      <tr class="border-t border-white/5">
        <td class="px-8 py-5 text-2xl text-white font-semibold" style="padding:20px 32px">React Native</td>
        <td class="px-8 py-5 text-2xl font-medium text-white" style="padding:20px 32px">17&nbsp;MB</td>
        <td class="px-8 py-5 text-2xl font-medium text-white" style="padding:20px 32px">28&nbsp;MB</td>
      </tr>
    </tbody>
  </table>
</div>

<!--
Resource usage is much lower with React Native. It doesn't include a full Chromium browser but it does include Hermes so apps are slightly bigger than Tauri but use much less memory than both.

We all know size matters but what's more important is how the app feels.
-->

---

<SlidevVideo src="/media/photos.mov" autoreset="slide" autoplay mute loop class="max-h-[460px]"  />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-photos</a></div>

<!--
I vibe coded a photo library app to see what kind of performance I could get with it. The app opens instantly. LegendList powers a gallery view and timeline with no blanking. I made a shared element transition animation just using Animated which looks awesome. And the photo loading is so fast it looks like a movie.

That video is not sped up, it's just that fast.
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
We can open multiple windows (ooooh) because it's not a webview. We can even use the platform native drag/drop behavior to drag between windows. Ooooh.

Clearly not many of you have used Electron because this should be blowing your minds.
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

<SlidevVideo src="/media/overlayblur.mp4" autoreset="slide" autoplay mute loop  />

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
    <div class="overflow-hidden rounded-3xl border border-white/10">
        <table class="w-full text-left text-base text-gray-200">
            <thead class="bg-white/5 text-gray-400 uppercase text-md">
            <tr>
                <th class="px-4 py-5 !font-medium" style="padding:20px 24px">App</th>
                <th class="px-4 py-5 !font-medium" style="padding:20px 24px">CPU</th>
            </tr>
            </thead>
            <tbody>
            <tr class="border-t border-white/5">
                <td class="px-4 py-5 text-2xl text-white font-semibold" style="padding:20px 24px">Spotify</td>
                <td class="px-4 py-5 text-2xl font-medium text-white" style="padding:20px 24px">26%</td>
            </tr>
            <tr class="border-t border-white/5 bg-white/5">
                <td class="px-4 py-5 text-2xl text-white font-semibold" style="padding:20px 24px">Music</td>
                <td class="px-4 py-5 text-2xl font-medium text-white" style="padding:20px 24px">2%</td>
            </tr>
            <tr class="border-t border-white/5">
                <td class="px-4 py-5 text-2xl text-white font-semibold" style="padding:20px 24px">React Native</td>
                <td class="px-4 py-5 text-2xl font-medium text-white" style="padding:20px 24px">2%</td>
            </tr>
            </tbody>
        </table>
    </div>
    <img src="/media/musiccpu.png" class="max-h-[320px] rounded-lg" />
</div>

<!--
Compared to other Electron apps, React Native uses 90% less CPU to play a local mp3 file. It's ludicrous that it should take 26% of my CPU to play an mp3 file.

Since React Native is native code with a lightweight JS engine, it's effectively the same as native CPU usage. Spotify as an Electron app is running a full Chrome browser and playing through the web platform, so it does a lot more work.
-->

---

# Memory

<div class="flex items-center justify-center gap-x-8 mt-10">
    <div class="overflow-hidden rounded-3xl border border-white/10">
        <table class="w-full text-left text-base text-gray-200">
            <thead class="bg-white/5 text-gray-400 uppercase text-md">
            <tr>
                <th class="px-4 py-5 !font-medium" style="padding:20px 24px">App</th>
                <th class="px-4 py-5 !font-medium" style="padding:20px 24px">Memory</th>
            </tr>
            </thead>
            <tbody>
            <tr class="border-t border-white/5">
                <td class="px-4 py-5 text-2xl text-white font-semibold" style="padding:20px 24px">Spotify</td>
                <td class="px-4 py-5 text-2xl font-medium text-white" style="padding:20px 24px">806 MB</td>
            </tr>
            <tr class="border-t border-white/5 bg-white/5">
                <td class="px-4 py-5 text-2xl text-white font-semibold" style="padding:20px 24px">Music</td>
                <td class="px-4 py-5 text-2xl font-medium text-white" style="padding:20px 24px">149 MB</td>
            </tr>
            <tr class="border-t border-white/5">
                <td class="px-4 py-5 text-2xl text-white font-semibold" style="padding:20px 24px">React Native</td>
                <td class="px-4 py-5 text-2xl font-medium text-white" style="padding:20px 24px">72 MB</td>
            </tr>
            </tbody>
        </table>
    </div>
    <img src="/media/musicmemory.png" class="max-h-[320px] rounded-lg" />
</div>

<!--
And comparing memory usage while playing a song is just as ludicrous. Running a full browser with 6 different processes is taking 806 MB. React Native uses 90% less memory at just 72.

It even beats Apple Music, the most native of native apps. Obviously it doesn't have as many features yet, but it's comparable.
-->

---

## Expo Orbit

<SlidevVideo src="/media/expoorbit.mp4" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[420px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/expo/orbit</a></div>

<!--
There's some really cool react native macos apps out there. Like Expo Orbit for launching builds or updates from EAS.
-->

---

## Sol

<img src="/media/sol.webp" class="max-h-[420px] rounded-lg" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/ospfranco/sol</a></div>

<!--
And Sol, this great open source mac launcher
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
We get the devtools to inspect the component tree
-->

---

<img src="/media/devtools2.png" class="max-h-[560px] rounded-lg" />

<!--
and the nice new performance tab to optimize everything. I use this tab a lot, I love it.
-->

---

<style>
#slide-native .slidev-code-wrapper {
    width: auto !important;
}
#slide-native .slidev-code-wrapper pre {
    padding-right: 32px !important;
}
</style>

<div id="slide-native">

```tsx
function PlayButton() {
    const onPressPlay = () => {
        AudioPlayerModule.playTrack(filename)
    }

    return (
        <Button onPress={onPressPlay} />
    )
}
```

</div>

<!--
I don't know how to write native modules, but if I want to do something natively, I just ask AI to make me a native module. And then I can do native things from React. I don't have to post messages across the IPC and all those shenanigans. It's just functions I can call from anywhere.
-->

---

<SlidevVideo src="/media/storybook.mp4" autoreset="slide" autoplay mute loop class="rounded-lg"  />

<!--
React Native Storybook fully supports macOS so we can run its macOS app to work with our desktop app component system.
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

<div class="flex gap-x-16">
    <div>

- @expo/log-box
- expo-asset
- expo-constants
- expo-crypto
- expo-eas-client
- expo-file-system
- expo-font
- expo-keep-awake

</div>
<div>

- expo-linking
- expo-local-authentication
- expo-manifests
- expo-mesh-gradient
- expo-modules-core
- expo-sqlite
- expo-updates
- expo-web-browser

</div>
</div>

<!--
A lot of Expo modules work on mac already. I specifically love expo file system, it's super fast. But there's a lot that aren't supported yet.
-->

---

# Desktop Features need libraries

- Capture screen / other apps
- Context menus
- Dock icon
- Dropdowns
- Global hotkeys
- Media playback
- Menu bar
- Processes
- Stoplight buttons
- Window management

<!--
And there's a whole ton of new libraries that need to be built for new desktop specific features that don't exist on mobile.
 -->

---

# Downsides

- Expo CLI doesn't support macOS/Windows (yet)
- Some libraries don't work on macOS/Windows
- Desktop specific functionality needs native modules

<!--
So there are still some downsides and rough edges.

It's not supported in the Expo CLI so it needs to be built separately.

And we need a lot more libraries to support desktop platforms.
-->

---

# <div class="text-[144px]">🐣</div>

<!--
So we've got a chicken and egg problem here. There's not a lot of people making React Native desktop apps, so libraries aren't adding desktop support, so it's hard to build apps because of a small ecosystem.

So while I would very much recommend react-native-macos to all of you legends, I think we need to smooth out some rough edges and support in more libraries before I'd suggest it to new developers.

So let's crack some eggs and get this ecosystem going.
-->

---

<img src="/media/wildwildwest.jpg" class="max-h-[500px] rounded-lg" />

<!--
It's a wild west out there. Lots of libraries need to be built, and you could make your library the defacto goto just with first mover advantage.

If you have a library with native code, please add mac support. As Saad said, it's usually pretty easy to port from iOS.

If you want to make a mac app, try doing it in react native. It's pretty cool.
-->

---

# Take React Native on desktop seriously

- Add macos support to your libraries

- Try making apps

- Talk to me or Saad

<!--
Talk to me or Saad if you're interested and I'll help or connect you to the right people
-->

<!--
TODO:
Add slides for Sol and Orbit
Make an awesome react native macos github repo
 -->