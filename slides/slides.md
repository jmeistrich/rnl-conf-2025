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

<div class="absolute bottom-0 w-124 gap-y-2">
  <div class="flex w-full justify-between mb-16">
    <div class="flex flex-col gap-y-1">
        <div>Saad Najmi</div>
        <div>𝕏 @SaadNajmi</div>
        <div>🦋 @saadnajmi.bsky.social</div>
    </div>
    <div class="flex flex-col gap-y-1 text-right">
        <div>Jay Meistrich</div>
        <div>𝕏 @jmeistrich</div>
        <div>🦋 @jayz.us</div>
    </div>
    </div>
  <div class="text-gray-400 pb-1">React Native London - Nov 14, 2025</div>
</div>

<!--
Good morning!

Big props to Theodo for putting together this awesome conference.

I'm 2 for 2 now and I hope to keep back coming every year!

So without further ado let's get into it and talk about React Native macOS.
-->

---

# Jay Meistrich

<center>
<img src="/media/jay.jpg" class="max-h-[200px] rounded-lg my-10" />
</center>

- CTO @ Bravely
- Legend List, Legend State, Legend Motion
- Expo

<!--
I'm Jay. That's me in a minion hat coding in the snow in -20 degrees, as I do.

I'm the CTO of Bravely, a mental health startup. And I make some open source libraries, Legend List and Legend State, and I recently joined Expo to see what could be possible with desktop apps.

I'm just here to be your hype man.
-->

---

# Saad Najmi

<center>
<img src="/media/saad.jpeg" class="max-h-[200px] rounded-lg my-10" />
</center>

- Senior Software Engineer @ Microsoft
- Tech lead for React Native macOS

<!--
Saad is the actual hero who leads the React Native macOS team at Microsoft.

He'll tell you what's actually happening in React Native macOS.
 -->

---

# Why isn't everyone using this?

<div class="text-[72px] text-center">
🤔
</div>

<!--
For a few years I've been seeing these talks from Microsoft about React Native on desktop and it seemed cool, but I haven't heard much about other apps using it, so I never paid much attention.

But then I tried it out just to see if LegendList would work in it, and I was blown away by how incredibly good it is.

I've been playing with it and making some apps, and it's seriously really good, though it has some rough edges.

I've been exploring all the benefits and downsides and I think
-->

---

# Take React Native on desktop seriously

<div class="text-[72px] text-center">
🤗
</div>

<!--
it's now time to take React Native on desktop seriously.

I got so excited about this that I joined Expo to explore how to make desktop apps better.
 -->

---

# It's really good

<!--
Because React Native macOS is just really good. It makes tiny fast apps and it's got that familiar React Native dev experience that we all love.
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

Going fully native requires building two entirely new apps in two entirely different languages, on top of any existing web and mobile apps they already have. So choosing a webview app is usually the best decision, to have time to actually release desktop apps at all.

But it's a big compromise, and it punishes us users with these mediocre web apps pretending to be desktop apps.
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
The most popular tool for building desktop apps is Electron.

Electron builds in a full chromium browser and Node.js into every app. It even bundles in ffmpeg because chromium depends on it, even though most apps would never use it.

A Hello World app is 268 MB, runs 4 separate processes, and uses 83 MB of memory.

And that's because it's a whole web browser.
-->

---

<img src="/media/ipc.png" class="max-h-[540px] rounded-lg" />

<!--
The way it works is you have a renderer process for the webview, which is an instance of Chromium. And you have a main process for the app itself that communicate through interprocess communcation.

The main process then runs Node.js to do system things, and passes the result all the way back up the bridges.

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
This is a simple example of how you run native code. You have to postMessage from the web process over to the main process, stringifying and parsing your data across the bridge, then run your native code.

Sending back a response is even more complicated. You can implement a Response Stream using Message Channels, or there's some libraries to wrap it up. But that's a whole complex thing for what should just be calling a function.
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

And you'll use Rust for the code in the main process, which is great if you love Rust, but otherwise it's a whole other language to learn.

And it's still a webview.
-->

---

<div class="flex max-w-full gap-x-4 justify-center">
    <img src="/media/electronL.png" class="max-h-[460px] flex-1 rounded-lg" />
    <img src="/media/electronR.png" class="max-h-[416px] flex-1 rounded-lg" />
</div>

<!--
A lot of the apps you use every day are Electron apps.

Now I don't mean to throw shade on Electron or these apps. Maintaining multiple native apps in different languages is a huge overhead, so Electron is probably the best choice for most apps.

You can even see my own app, Legend, in there. That's part of why I'm so passionate about this, because I've been mad about this for 8 years now.

So this is just the world we live in where our desktop apps are web apps.
-->

---

<img src="/media/xcode.png" class="max-h-[300px] flex-1 rounded-lg" />

<!--
Of course the biggest app on my computer is not Electron, but that's a whole other thing.
-->


---

<img src="/media/downloads.png" class="max-h-[540px] rounded-lg" />

<!--
When I've been ranting to everybody I know about React Native on desktop, they tell me that nobody really builds desktop apps anymore.

But I don't think that's true. Electron is actually downloaded almost as much as Expo is.

And desktop apps are just not as good as they should be.
-->

---

# Because they're bad?

<div class="img-crop h-[280px] rounded-lg mt-10" style="--crop-top: 80px; --crop-bottom: 10px;">
  <img src="/media/minionsbad2.gif" class="" />
</div>

<!--
And maybe that's the reason why they aren't super popular, because they're bad?

They're basically just 300 megabyte web apps that use more memory and fill your hard drive. So you might as well just use the web app.

But what if they were actually good?

I really believe that if desktop apps were significantly better than web apps, people would use them. Because they're so much better.
-->

---

# Reverse the Enshittification

<div class="flex justify-center">

<img src="/media/minionspunch.gif" class="max-h-[580px] rounded-lg mt-8" />

</div>

<!--
So I think it's time to reverse the enshittification and make good apps again.
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
We've been through this before on mobile with Cordova and Ionic apps being wrappers around web apps. They were cross platform and easier to build, but bad for users.

Native apps are more delightful but hard to build and need dedicated teams. So companies built cross-platform web apps. And everything was bad.
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

Well to be fair, it was rough for a while but now it's really good.
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

And again we have an opportunity for React Native to come in and save everyone.
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
So since we're already building with React Native anyway, we can make React Native the cross-platform solution on desktop as well.

Then instead of desktop apps being basically just giant heavy web apps, we can build incredible native experiences and share code across all desktop and mobile platforms.

And fall back to the mediocre web experience when we don't have an app.
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
And maybe we could eventually get to React Native everywhere, but that's a whole other thing.

So the big question is why React Native on desktop?
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

Apps are tiny.

And they're fast because Hermes is fast, native modules are fast, they don't have to run a whole browser, and they don't have to communicate across a webview bridge.

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

<SlidevVideo src="/media/photos.mov" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-photos</a></div>

<!--
I started into React Native macOS by vibe coding a photo app to see what kind of performance I could get with it.

The app opens instantly. It has a gallery view and timeline that look super smooth. Shared element transitions look awesome. And the photos load so fast it looks like a movie.

Looking back at it that video looks sped up, but it's just that fast.
-->

---

<SlidevVideo src="/media/windows.mov" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-music</a></div>

<!--
And then I made a music app to try to save my battery life. It can open multiple windows (ooooh) because it's not a webview. It can even drag and drop between windows. Ooooh.

I notice I'm not hearing all of you cheering. Clearly not many of you have used Electron because this should be blowing your minds right now.
-->

---

<img src="/media/medium.png" class="max-h-[420px] rounded-lg" />

<!--
And you haven't read enough Medium articles about how to postMessage between the isolated Chromium processes.

In React Native this is just easy, it's using platform native drag drop events.
-->

---

<SlidevVideo src="/media/overlay1.mov" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-music</a></div>

<!--
We can even do cool animations on the windows. When the song changes this overlay window at the top of the screen unblurs and fades in, and after two seconds it blurs and fades out.
-->

---

<SlidevVideo src="/media/overlayblur.mp4" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-music</a></div>

<!--
Let's look at that in slow motion to see how cool it is. Codex made me this native blur filter in Swift which looks awesome.
-->

---

<SlidevVideo src="/media/overlay2.mov" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-music</a></div>

<!--
And multiple windows can easily share state. Both of these windows use the same PlaybackArea component, and since it's subscribed to global state they're fully synchronized.

I didn't even think about this until it was already done. This is just a given with React.

And again in the alternatives you have to postMessage between isolated chromium processes to do this.

Now let's step it up a notch. React Native Skia has mac support.
-->

---

<SlidevVideo src="/media/visbar.mov" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-music</a></div>

<!--
So we can display this dope visualizer by having our native audio module tap into the raw frequency data.

That's pretty cool right? But let's step it up a notch.
-->

---

<SlidevVideo src="/media/visaurora.mov" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-music</a></div>

<!--
We can build visualizers with shaders in skia, so we can display it as this cool glowing radial cloud.

Or we could step it up another notch and go full 3D.
-->

---

<SlidevVideo src="/media/viscube.mov" autoreset="slide" autoplay mute loop class="max-h-[480px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/LegendApp/legend-music</a></div>

<!--
We can make full 3D visualizers in shaders using the power of the GPU. And why not, this is a native app, we can do whatever we want.
-->

---

<img src="/media/musicsize.png" class="max-h-[320px] rounded-lg" />

<!--
This complete app using Nativewind, a bunch of libraries and expo modules, and skia comes out to 35 MB.

Zipped, it's an 11 MB download.
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

Just think about what's happened to us as a society that playing an mp3 file uses more processing power now than it did 25 years ago on my Pentium 2.
-->

---

<img src="/media/boomer.gif" class="max-h-[480px] rounded-lg" />

<!--
Back in my day people cared about performance.
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
                <td class="px-4 py-5 text-2xl text-white font-semibold" style="padding:20px 24px">Apple Music</td>
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
And comparing memory usage while playing a song is just as ludicrous. Running a full browser with 6 different processes takes 800 megabytes of memory.

800! This should not be acceptable.

React Native uses over 90% less memory at just 72.

It even beats Apple Music, the most native of native apps. Obviously my app doesn't have as many features yet, but the memory usage is comparable to native.
-->

---

# React Native macOS is awesome

<!--
So React Native is pretty awesome. And it's been getting a lot better this past year.

And Saad will tell you all about that.
-->

---

# What have we been working on?

<v-click>

- Paying down Tech Debt

- Implementing the new architecture

</v-click>

<!--

What has our team been working on? I can break down the work to mostly two buckets: "Paying down tech debt", and "implementing the new architecture". I'd love to have spent all my time on the latter, but I also realized in order for us to move fast and be a library people use, we need to work on the former. Let's talk about some of the tech debt.
-->

---

# Stabilizing our releases


<!--
The first bit of tech debt we had to pay off was to stabilize our releases pipelines. Up until now, our release tooling was a hodgepodge of scripts built up over time. Some of it came from React Native, some open source tools Microsoft has, and some our own one off bash scripts. This made it really hard to do releases, and we'd consistently take an extra week or two just remembering what levers to turn on and off. We ended up using `nx release` because we wanted to try something new and cool, but there's a lot of options. We also added a bunch more PR checks to keep the repo green. In particular, Modern Yarn has a cool feature called constraints that helps with this. I'll talk more about that later. Many thanks to Tommy Nguyen, who took on this project with me and helped me a lot.
-->

---

# Stabilizing our releases

Did it pay off?

<v-click>

<img src="/media/gabriel-79-merge.png" class="max-h-[360px] rounded-lg" />

</v-click>

<!--
Did this pay off? I think so, because for the first time, React Native macOS had a release made by the open source community. Gabriel, the maintainer of Expo Orbit (which by the way, is a React Native macOS app) wanted to have 0.79, so I went and documented a release guide so we could get that out ASAP.
-->

---


# Documentation

<img src="/media/windows-docs.png" class="max-h-[360px] rounded-lg" />

<div class="flex justify-center gap-x-4" >🔗 <a href="microsoft.github.io/react-native-windows">microsoft.github.io/react-native-windows</a></div>

<!--
Speaking of docs, that's our next area of tech debt we had to address. Historically, the macOS and windows docs were all on one website, but it was mmostly React Native Windows docs. There were only two pages for macOS docs, that really only told you how to make a hello world app. That's bad for our users, and that's on me for not updating them enough.
-->

---


# Documentation

<img src="/media/macos-docs.png" class="max-h-[360px] rounded-lg" />

<div class="flex justify-center gap-x-4" >🔗 <a href="microsoft.github.io/react-native-macos">microsoft.github.io/react-native-macos</a></div>

<!--
To fix that, we made a new docs site! It lives in the React Native macOS repo, so it's super easy to find and update. And it has a dark mode!
-->

---

# Documentation

<img src="/media/macos-docs-ios-port.png" class="max-h-[360px] rounded-lg" />

<div class="flex justify-center gap-x-4" >🔗 <a href="microsoft.github.io/react-native-macos">microsoft.github.io/react-native-macos</a></div>

<!--
As I worked on macOS over the last several months, if I ran into something I felt I know that others should also know, I'd make a note to add it to the website. We have a guide for how to port an iOS library to macOS, that I wrote from my personal experience of porting WebGPU and Skia.
-->

---

# Documentation

<img src="/media/macos-docs-props.png" class="max-h-[360px] rounded-lg" />

<div class="flex justify-center gap-x-4" >🔗 <a href="microsoft.github.io/react-native-macos">microsoft.github.io/react-native-macos</a></div>

<!--
We also have an API section where we list a bunch of the macOS only props we have. I found most of these as we had to reimplement them for the new architecture. I hope the new docs site is useful, encourage yall to raise issues and contribute if something is confusing, because I'm not going to know every case that people run into.
-->

---

# Documentation

<img src="/media/macos-docs-expo.png" class="max-h-[360px] rounded-lg" />

<div class="flex justify-center gap-x-4" >🔗 <a href="microsoft.github.io/react-native-macos">microsoft.github.io/react-native-macos</a></div>

<!--
I also asked others to contribute, and Gabriel added some docs for how to use Expo Modules with macOS, and bundle with the Expo CLI. Thanks Gabriel!
-->

---

# The New Architecture

<!--
OK, that's enough about tech debt. Let's talk about the new architecture. Let me first give you a recap of where macOS is.
-->

---

# The New Architecture

Microsoft 🤝 Meta

<v-click>

- RNM 0.71 - Technical Preview

</v-click>

<v-click>

- RNM 0.81 - On by default

</v-click>

<!--
Several years ago, we partnered with Meta to help us implement the new architecture. They had a team that wanted to build desktop apps. They internally made a fork, of our fork, of React Native macOS, and will contribute back changes upstream to us after it's been tested in their apps. Back in 0.71, we had a technical preview of the new architecture, where it rendered and ran but didn't do much. Looking forward, in our 0.81 release, I merged a bunch more of Meta's implementation, and we plan to have the new architecture on by default.
-->

---

# The New Architecture

- Hermes
- Fabric


<!--
There are two main areas where we needed to make progress on the new architecture: Hermes, and Fabric. Hermes was much simpler, and was mostly a version lookup problem, while Fabric is where we had to reimplement a lot of stuff from Paper. Let's talk about Hermes first.
-->

---

# Hermes

- It mostly worked
- Versioning problem

<!--
Hermes, the JS engine, has always worked on macOS, but it didn't consistently work on React Native macOS.Every now and then your React Native macOS build might fail to compile. It was bad enough that we couldn't test Hermes in our CI, even though it mostly worked for end users. What's happening here? Turns out the answer is simple. We're a fork of React Native, and our scripts that choose what version of Hermes to use didn't properly account for that. Our main branch would try the latest commit of Hermes, even if we were 1000-2000 commits behind React Native Core. Our release branches would always pull the latest minor release, even if we weren't synced that ahead yet.
-->

---

# Hermes

Solution?

<img src="/media/hermes-pr.png" class="max-h-[360px] rounded-lg" />

<!--
The solution was quite simple. We just had to make sure we looked up the right version of Hermes. On our main branch, we can use the git merge-base to grab the right commit of Hermes. On our release branches, we can add a peer dependency to the React Native Core release we are compatible with, and use that to grab Hermes. We made those fixes, and backported to 0.74. Now, we have Hermes enabled by default! Many thanks to my coworker Adam, who did most of the work to fix this.
-->

---

# Hermes

React Native Devtools

<SlidevVideo src="/media/devtools-rntester.mp4" autoreset="slide" autoplay loop mute class="max-h-[360px] rounded-xl" />

<!--
Hermes also means one more thing. We also now fully support React Native Devtools, so you get the same familiar experience with the paused in debugger overlay and the ability to reconnect after reloads. In my own personal dev, this was so much more useful.
-->

---

# Fabric

<!--
Let's talk about the next piece of the new architecture, Fabric. Fabric the native renderer, that takes the props and components from Javascript, and parses it into a native UI tree. This is the biggest piece that was missing from React Native macOS, and where most of the work went from both us and Meta.
-->

---

# Fabric

- All the macOS props work
- Bug squashing through core props

<!--
In 0.71, we had Fabric at the state where it could render the UI, but most of the props didn't work. As of right now, we have ported over all the macOS specific props, and we're mostly bug squashing our way through the core props. We haven't quite worked through all of them, so we haven't release 0.81, but I hope to soon.
-->

---

# Fabric

It's much better

<!--
Along the way of porting over Meta's fabric commits, I saw firsthand how much better written Fabric is over Paper. There are so many little and big decisions that we had a chance to redo, and it was honestly pretty fun to write stuff with the new APIs. There's more than I can talk about now, but I'd like to dive into one of the ways Fabric is better, particularly for out of tree platforms, prop parsing. Let's dive into some source code!
-->

---

# Fabric

Prop Parsing

<!--
In Paper for iOS, this was done by passing JSON to a bunch of macros. This was pretty versatile, but kinda ugly to look at. It also had no guarnatees of type safety, since the JSON can be anything. Performance also isn't great, since your bottleneck is how fast you can serialize and deserialize JSON.
-->

---

# Fabric

<img src="/media/prop-parsing-paper.png" class="max-h-[360px] rounded-lg" />

<center>
Paper
</center>

<!--
Both Paper and Fabric need to pass props from JS to native code, so they can be parsed into native props. In Paper for iOS, this was done by passing JSON to a bunch of macros. This was pretty versatile, but kinda ugly to look at. It also had no guarnatees of type safety, since the JSON can be anything. Performance also isn't great, since your bottleneck is how fast you can serialize and deserialize JSON.
-->

---

# Fabric

<img src="/media/prop-parsing-fabric.png" class="max-h-[360px] rounded-lg" />

<center>
Fabric
</center>

<!--
In Fabric, all of the prop parsing moved into a shared C++ layer. Instead of parsing JSON, with Fabric we know the types in advance, so we can just create C++ structs and classes that match what we're sending from JS. This is much faster and much cleaner to look at. And it can be shared across platforms, which is awesome for us.
-->

---

# Fabric

Platform specific props

"The Host Platform Model"

<!--
How do we represent platform specific props? It's actually quite easy, thanks to something I'll call "the host platform model". We can have a base class (in this case, BaseViewProps), that is all the common props between platforms.
-->

---

# Fabric

<div class="pl-5 text-gray-400">
Platform specific props
</div>

<img src="/media/host-platform-props-ios.png" class="max-h-[360px] rounded-lg" />

<center class="text-gray-400">
iOS
</center>

<!--
Then, each platform can define it's own "host platform props". For iOS, React Native just aliases and reuse the base props. For Android, React Native extends the class to add Android specific props, like `elevation`.
-->

---

# Fabric

<div class="pl-5 text-gray-400">
Platform specific props
</div>

<img src="/media/host-platform-props-android.png" class="max-h-[360px] rounded-lg" />

<center class="text-gray-400">
Android
</center>

<!--
For Android, React Native extends the class to add Android specific props, like `elevation`.
-->

---

# Fabric

<div class="pl-5 text-gray-400">
Platform specific props
</div>

<img src="/media/host-platform-props-macos.png" class="max-h-[360px] rounded-lg" />

<center class="text-gray-400">
macOS
</center>

<!--
This makes it easy for macOS to do the same thing, and add all of our props one by one. Bonus points, it also makes it easy to add documentation for what our macOS specific props are, now that they're in an easy to read header. I know this is the right pattern, because React Native Windows uses it too.
-->

---

# Fabric

<div class="pl-5 text-gray-400">
This feels too easy?
</div>

<v-click>

<img src="/media/host-platform-pr.png" class="max-h-[360px] rounded-lg" />

</v-click>

<center>&nbsp;</center>

<!--
If this all feels too easy to add a new platform, it's because it was designed that way. Remember that partnership we had with Meta? One of their engineers, Eric Rozell, contributed these APIs to make it easier to work on the desktop platforms, while he was working on it for React Native Windows. This also makes it easier to add future platforms, like what what we're seeing with TV and VR. Thanks Eric!
-->

---

# Looking forward

- 0.81 - On by default

- 0.82 - The only architecture

<!--
Looking forward, I can't wait to release our next version of React Native macOS where you all can play around with Fabric. As you might know, on React Native 0.82 and up, you can no longer disable the new architecture. For us, that means that we will probably stay at 0.81 for a while. We still have a lot of work to do internally to get our apps like Office onto the new architecture, and properly stress test it with our millions of users. We're hard at work, and I'm excited to see what the future holds. Let me pass it back to Jay now, so he can tell show you some cool macOS apps in the wild.
-->

---

## Expo Orbit

<SlidevVideo src="/media/expoorbit.mp4" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[420px]" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/expo/orbit</a></div>

<!--
I want to show you some really cool React Native macOS apps.

Like Expo Orbit for launching builds or updates from EAS. It's this nice menu bar app which uses deep links to be activated from the web and run OS simulators with your builds. It's pretty cool.
-->

---

## Sol

<img src="/media/sol.webp" class="max-h-[420px] rounded-lg" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/ospfranco/sol</a></div>

<!--
And Sol, this great open source customizable mac launcher app is also pretty cool.
-->

---

# Reactotron

<img src="/media/reactotron2.png" class="max-h-[230px] rounded-lg" />
<img src="/media/reactotron1.png" class="max-h-[180px] rounded-lg" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/infinitered/reactotron-macos</a></div>

<!--
And Infinite Red is currently rewriting their powerful React Native debugging tool Reactotron using React Native macOS
 -->

---

# BrowserUI

<img src="/media/browserui.png" class="max-h-[410px] rounded-lg" />

<div class="mt-4 flex justify-center gap-x-4" >🔗 <a>https://github.com/DanielSRS/BrowserUI</a></div>

<!--
There's even a guy building a browser with React Native
-->

---

# Familiar DX

<!--
And because it's React Native we get the great developer experience we're used to.

Of course Fast Refresh works, so it's easy to tweak the design.
-->

---

<img src="/media/devtools1.png" class="max-h-[520px] rounded-lg" />

<!--
We get the devtools to inspect the component tree
-->

---

<img src="/media/devtools2.png" class="max-h-[520px] rounded-lg" />

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
I don't actually know how to write native modules, but if I want to do something natively, I just ask AI to make me a native module.

And then I can do native things from React. I don't have to post messages across the IPC and all those shenanigans. It's just functions I can call from anywhere.
-->

---

<SlidevVideo src="/media/storybook.mp4" autoreset="slide" autoplay mute loop class="rounded-lg"  />

<!--
And React Native Storybook fully supports macOS so we can run the Storybook mac app to work with our desktop app component system.
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
And there's a whole ton of new libraries that still need to be built for new desktop specific features that don't exist on mobile.
-->

---

# Downsides

- We're experimenting with Expo CLI support
- Some libraries don't work on macOS/Windows
- Desktop specific functionality needs native modules

<!--
So there are still some downsides and rough edges.

We're still experimenting with Expo CLI support.

And we need a lot more libraries to support desktop platforms.
-->

---

# <div class="text-[144px]">🐣</div>

<!--
So we've got a chicken and egg problem here. There's not a lot of people making React Native desktop apps, so libraries aren't adding desktop support, so it's hard to build apps because of a small ecosystem.

So while I would very much recommend react-native-macos to all of you legends, I think we need to smooth out some rough edges and have more desktop support in libraries before I'd suggest it to new developers.

So let's crack some eggs and get this ecosystem going.
-->

---

<img src="/media/wildwildwest.jpg" class="max-h-[500px] rounded-lg" />

<!--
It's a wild west out there. Lots of libraries need to be built, and you could make your library the defacto goto by just being first.

If you have a library with native code, please add mac support. As Saad said, it's usually pretty easy to port from iOS.

If you want to make a mac app, try doing it in react native. It's pretty cool.
-->

---

# Take React Native on desktop seriously

- RNM 0.81 coming soon with Fabric

- Add macOS support to your libraries

- Try making apps

- Talk to me or Saad

- https://github.com/jmeistrich/awesome-react-native-desktop

<div class="absolute bottom-2 w-170 gap-y-2">
  <div class="flex w-full justify-between">
    <div class="flex flex-col gap-y-1">
        <div>Saad Najmi</div>
        <div>𝕏 @SaadNajmi</div>
        <div>🦋 @saadnajmi.bsky.social</div>
    </div>
    <div class="flex flex-col gap-y-1 text-right">
        <div>Jay Meistrich</div>
        <div>𝕏 @jmeistrich</div>
        <div>🦋 @jayz.us</div>
    </div>
    </div>
</div>

<!--
So let's take React Native on desktop seriously

I made this little repo with links to the documentation and apps I showed in the slides, so check that out.

We're well aware that it has rough edges. So please talk to me or Saad if you're interested and wel'll either help you or connect you to the right people.
-->
