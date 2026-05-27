---
theme: default
highlighter: shiki
transition: none
comark: true
mdc: true
defaults:
  layout: center
  transition: view-transition
---

<div class="pb-12">

# How to Build the Fastest Apps:

# Break the Rules

</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div>@jmeistrich</div>
  <div>Bravely, Legend, Margelo</div>
</div>

<!--
Hi, I'm Jay.

Last year's App.js was my first conference talk and it was so much fun! It's great to be back!

Now I'm here to talk about performance.
-->

---

# Performance Optimization Techniques

<div class="flex flex-col gap-y-4 max-w-[280px] mx-auto text-xl pt-6">
    <div>Less re-renders</div>
    <div>Object pooling</div>
    <div>Freeze background screens</div>
    <div>Priotize First Contentful Paint</div>
    <div>Files can be faster than SQL</div>
    <div>NitroModules</div>
    <div v-click class="absolute inset-0 flex flex-col items-center justify-center" >
        <img src="/media/red-x.png" />
    </div>
</div>

<!--
My plan for this talk was to cover a bunch of different performance topics. But as I benchmarked them and helped a few companies performance optimize their apps, I realized that one of these is so much more impactful than everything else, and most apps are doing it the slow way without even knowing it.

2. So we're not doing that. Today we're talking about completely rethinking app architecture.
-->

---

# REALLY FAST

<!--
I want to talk about making apps that are REALLY FAST.

Not just fast for a React Native app, better than native.
-->

---

<SlidevVideo src="/media/v0.mp4" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[560px]" />

<!--
I worked with Fernando on animations and list performance for the v0 iOS app. It's a beautiful AI chat app that feels incredible.
-->

---

<img src="/media/v0-rn.png" class="rounded-lg" />

<!--
When it was announced people couldn't believe it was React Native because it felt so good.
-->

---

<SlidevVideo src="/media/md-open.mov" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[560px]" />

<!--
Here's a React Native mac app I'm working on. This video is me double clicking the app, and it's fully open and ready instantly. It opens a 10MB markdown file in 20 milliseconds.
-->

---

<SlidevVideo src="/media/md-open-slow.mp4" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[560px]" />

<!--
Let's look at that in slow motion at 5% speed to see how crazy it is. It's already fully open, parsed, and rendering rich content before the window finishes animating open.

But ignore the white flash, I just haven't hooked up native theming yet.
-->

---

<SlidevVideo src="/media/md-zed.mov" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[560px]" />

<!--
For comparison, here's Zed, a text editor built in Rust known for being fast. It opens the file quickly, but it takes over 4 seconds to parse and syntax highlight. Most other apps I tried either froze indefinitely or just crashed.
-->

---

<SlidevVideo src="/media/visaurora.mov" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[560px]" />

<!--
Here's another app I'm making. It's a React Native mac mp3 player.
-->

---

<div class="flex items-center justify-center gap-8">
  <img src="/media/musiccpu.png" class="rounded-lg max-h-[340px]" />
  <img src="/media/musicmemory.png" class="rounded-lg max-h-[340px]" />
</div>

<!--
It uses less CPU and memory than almost every other music app I tried, even the native Apple Music player.
-->

---
clicks: 1
---

<h1>
    React Native
    <span class="relative inline-block overflow-hidden align-bottom">
        <span class="invisible">in 2026</span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? '-translate-y-full opacity-0' : 'translate-y-0 opacity-100'"
        >
            macOS
        </span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? 'translate-y-0 opacity-100' : 'translate-y-full opacity-0'"
        >
            in 2026
        </span>
    </span>
    <span> is 🔥</span>
</h1>

<!--
These are React Native apps and they're faster than almost every other app on my computer.

They're not just fast relative to Electron webview apps. They're faster than most native apps.

Because React Native macOS is awesome.

2. But mainly, React Native in 2026 is just so good.
-->

---

# React Native in 2026 is 🔥 {.inline-block.view-transition-title}

---

# React Native in 2026 is 🔥 {.inline-block.view-transition-title}

<div class="mx-auto w-[180px]">

New Architecture

Hermes

Nitro Modules

Lists

Redraw

Keyboard Controller

Agent Device / Argent

</div>

<!--
The game has totally changed.

The new architecture unlocks big possibilities.

Hermes keeps getting better.

Nitro Modules are crazy fast.

Lists are no longer a bottleneck.

As we just saw, Redraw makes incredible graphics easy and fast.

Keyboard Controller makes keyboard management super smooth.

Agent Device and Argent have cut my development and debugging time way down.
-->

---

<div class="absolute inset-0 overflow-hidden">
  <img src="/media/tweet1.png" class="absolute w-[560px] rounded-lg shadow-2xl" style="left: 46px; top: 34px; transform: rotate(-5deg);" />
  <img src="/media/tweet2.png" class="absolute w-[560px] rounded-lg shadow-2xl" style="left: 542px; top: 30px; transform: rotate(4deg);" />
  <img src="/media/tweet4.png" class="absolute w-[560px] rounded-lg shadow-2xl" style="left: 286px; top: 138px; transform: rotate(-2deg);" />
  <img src="/media/tweet5.png" class="absolute w-[560px] rounded-lg shadow-2xl" style="left: -16px; top: 266px; transform: rotate(4deg);" />
  <img src="/media/tweet7.png" class="absolute w-[560px] rounded-lg shadow-2xl" style="left: 460px; top: 336px; transform: rotate(-4deg);" />
  <img src="/media/tweet8.png" class="absolute w-[560px] rounded-lg shadow-2xl" style="left: 86px; top: 420px; transform: rotate(3deg);" />
</div>

<!--
But still, React Native is seen as a compromise.

And that's just not actually true. I've seen and worked on React Native apps that are fully best in class, sometimes better than native. I know it's possible.

But this year I've also looked at a lot of codebases for consulting work and helping companies with LegendList issues, and I saw the same problems spread all over.

So I want us to stop fixing the same symptoms over and over, let's try to fix the root cause.

Because React Native is not slow. The way we use it is.
-->

---
clicks: 1
---

<h1>
  Render
  <span class="relative inline-block overflow-hidden align-bottom">
    <span class="invisible">less, less often</span>
    <span
      class="absolute left-0 transition-all duration-500 ease-out"
      :class="$clicks >= 1 ? '-translate-y-full opacity-0' : 'translate-y-0 opacity-100'"
    >
      less, less often
    </span>
    <span
      class="absolute left-0 transition-all duration-500 ease-out"
      :class="$clicks >= 1 ? 'translate-y-0 opacity-100' : 'translate-y-full opacity-0'"
    >
      once
    </span>
  </span>
</h1>

<!--
If you've seen one of my talks before you may know my catchphrase: render less, less often.

But today I want to make it spicier:

2. Render once.
-->

---

# Is re-rendering bad?

<!--
But first, why is rendering too much a problem? Everyone talks about reducing renders, but what's the actual impact?
-->

---

# Rendering benchmark

<br />
<div class="flex three-column-code gap-2">

```tsx
function App() {
  const [time, setTime] = useState(0)

  return (
    <Window>
      <PlaybackArea time={time} />
        <Playlist />
        <BottomToolbar />
      </Window>
    )
}
```

```tsx
function PlaybackArea() {
  const [time, setTime] = useState(0)

  return (
    <View>
      <PlaybackTime time={time} />
      <TheRestOfThePlaybackArea />
    </View>
  )
}
```

```tsx
function ElapsedText() {
  const [time, setTime] = useState(0)

  return (
    <Text>{time}</Text>
  )
}
```

<style>
.three-column-code {
    align-items: flex-start;

    .slidev-code-wrapper {
        width: 310px !important;
    }

    .slidev-code-wrapper::before {
        display: block;
        margin-bottom: 8px;
        color: #9ca3af;
        font-size: 16px;
        font-weight: 500;
    }

    .slidev-code-wrapper:nth-of-type(1)::before {
        content: "App root";
    }

    .slidev-code-wrapper:nth-of-type(2)::before {
        content: "Playback area";
    }

    .slidev-code-wrapper:nth-of-type(3)::before {
        content: "Leaf text";
    }
}
</style>

</div>

<!--
I did a little benchmark in my music app to compare two things:

First, Frequency: Re-rendering 4 times a second vs. 12 times a second

and Second, Size: Re-rendering from the app root vs. in a medium size component in the middle of the tree vs. updating just a single text element leaf node.

When that state passes all the way down from the top it has to do a lot more work along the way than just updating one text element. But it's a small difference, right?
-->

---

# CPU Usage

<div class="flex gap-2 mt-10 text-left">
  <div class="box-not-flashing flex-1 flex-col !items-start">
    <div class="text-xl text-gray-400">App 12hz</div>
    <div class="text-5xl font-bold mt-4">20%</div>
  </div>
  <div class="box-not-flashing flex-1 flex-col !items-start">
    <div class="text-xl text-gray-400">App 4hz</div>
    <div class="text-5xl font-bold mt-4">10%</div>
  </div>
  <div class="box-not-flashing flex-1 flex-col !items-start">
    <div class="text-xl text-gray-400 whitespace-pre">PlaybackArea</div>
    <div class="text-5xl font-bold mt-4">8%</div>
  </div>
  <div class="box-not-flashing flex-1 flex-col !items-start">
    <div class="text-xl text-gray-400 whitespace-pre">ElapsedText</div>
    <div class="text-5xl font-bold mt-4">1%</div>
  </div>
</div>

<!--
No, it's huge.

Even I was surprised by this.

Obviously, reducing the update frequency reduced the CPU usage quite a bit.

But then dropping that update target from the top level down to the tiniest leaf node made a huge difference, reducing the CPU usage 10 times.

So rendering less has a...
-->

---

<img src="/media/deep-impact.jpg" class="rounded-t-3xl rounded-b-xl" />

<!--
Deep impact
-->

---

# Render coordinates everything {.inline-block.view-transition-title}

<!--
But React's state model is designed around re-rendering. Render coordinates everything.
-->

---

<center>

# Render coordinates everything {.inline-block.view-transition-title}

</center>

<div class="flex two-column-code flex-wrap justify-center gap-x-4 gap-y-4 mt-4">
<!--
<div class="box-not-flashing-small flex-1 flex-col !items-start">
    <div class="text-2xl text-bold">UI</div>
    <div class="mt-3">Render UI</div>
</div>-->

```tsx
const { projectId } = useContext(ProjectContext)
```

```tsx
useEffect(() => {
    const stuff = loadImportantStuff()
    setState(stuff)
}, [])
```

```tsx
useEffect(() => {
    reportUnread()
}, [conversationId, roomId])
```

```tsx
useEffect(() => {
    thing.subscribe()
    return () => thing.unsubscribe()
}, [thing])
```

```tsx
const isPageVisible = useVisibility()
const query = useQuery({
  enabled: isPageVisible
  // ...
})
```

```tsx
return (
  <ProjectContext.Provider value={value}>
    <View>
      <Example prop={prop} />
    </View>
  </ProjectContext.Provider>
)
```

</div>

<style>
.two-column-code {
    .slidev-code-wrapper {
        width: 380px !important;
    }
}
</style>

<!--
- Render passes local state down the tree
- It updates context consumers
- It triggers effects
- It updates query properties
- It updates external properties
- It updates subscriptions
- And of course it renders UI

That's a lot of responsibility for "render". And that's where so many normal React patterns become a performance problem.
-->

---

<img src="/media/chat-without-reply.png" class="rounded-2xl" />

<!--
Let's say we're making a chat app. We have a reply feature that changes the background color of a message. That's cool, just re-render the message to update the color.
-->

---

<img src="/media/chat-with-reply.png" class="rounded-2xl" />

<!--
And then you want to also show in the composer that you're replying. So now we need to access the same state in sibling components. So what do you do? Say it with me!
-->

---

<img src="/media/lifting-state-up.png" class="rounded-lg" />

<!--
Lift state up!

Nobody did it, that's okay.

Well, you do what the React docs tell you. You lift state up. And as the docs say, it's one of the most common things you'll do writing React code.
-->

---

```tsx
function ChatScreen() {
  const [replyId, setReplyId] = useState('')

  return (
    <View>
        <ChatMessages replyId={replyId} />
        <ChatComposer replyId={replyId} />
    </View>
  )
}
```

<!--
So you move the state up to the top of the tree and pass it all the way down.
-->

---

<div class="flex flex-col gap-2 pt-8">
  <div class="flex justify-center">
    <div class="box-flashing ml-10">ChatScreen</div>
  </div>
  <div class="flex justify-center items-start">
      <div>
    <div class="box-flashing">ChatMessages</div>
    <div class="flex flex-col flex-1 items-center">
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
    </div>
      </div>
      <div>
        <div class="box-flashing">ChatComposer</div>
      <div class="flex flex-col flex-1 items-center">
        <div class="box-flashing">ReplyRow</div>
        <div class="box-flashing">InputRow</div>
      </div>
      </div>
    <div class="flex justify-center">
        <div class="box-flashing">useEffects</div>
    </div>
  </div>
</div>

<!--
And it re-renders every component all the way down.

It re-renders the chat screen. It re-renders the composer with all of its dialogs and buttons and hooks. It re-renders everything down to the message list. And then it re-renders every single chat message. So that one message can change a background color.

That kills performance.
-->

---

# React Compiler!

<!--
And React Compiler helps a lot, but it can't save you from legitimate changes.
-->

---

```tsx
function ChatScreen() {
  const [replyId, setReplyId] = useState('')

  return (
    <View>
        <ChatMessages replyId={replyId} />
        <ChatComposer replyId={replyId} />
    </View>
  )
}
```

<!--
Wherever a prop is changed, it has to render to propagate state down. Compiler can't help you there. To update a style in one message we have to cascade state changes through the whole screen.
-->

---

<img src="/media/not-need-useeffect.webp" class="rounded-lg" />

<!--
Now let's talk about effects. We all know the "You might not need useEffect" meme, but it is still a core tool for orchestrating apps.
-->

---

```tsx
function ChatScreen() {
    const [isOpen, setIsOpen] = useState(false)

    useEffect(() => {
        if (isOpen) {
            dialogRef.current?.showModal()
        }
    }, [isOpen])

    return (
        <Pressable onPress={() => setIsOpen(true)} />
    )
}
```

<!--
Here's an example from the React docs: controlling a modal dialog. To open a dialog on button click, we could just open the dialog. But the React pattern is to set state, which triggers a render, then use an effect to open the dialog.

It doesn't change what renders. But it re-renders anyway.
-->

---

```tsx
function ChatScreen() {
    const [isPaused, setIsPaused] = useState(false)

    const { data } = useQuery({
        queryKey,
        refetchInterval: isPaused ? false : 1000,
    })

    const pauseQuery = () => {
        setIsPaused(true)
    }
}
```

<!--
Or maybe I want to pause query polling temporarily. I have to set a state to call the useQuery hook again with different parameters.

Setting isPaused doesn't change what renders. But we have to re-render to change it.
-->

---

```tsx
function ChatScreen() {
    const isFocused = useIsFocused()

    useEffect(() => {
        if (isFocused) {
            markConversationRead(conversationId)
        }
    }, [isFocused, conversationId])
}
```

<!--
Or I want to mark this chat as read, only when the window is focused. The useIsFocused hook re-renders when focus state changes, so we run it in an effect.

That doesn't change what renders. But now this screen re-renders every time focus changes.
-->

---

# Render is for coordinating

<!--
This is all because render is needed for coordinating things that aren't even rendering. We're wasting all of this computing to just run side effects.
-->

---

````md magic-move
```tsx
function Component() {
  const [value, setValue] = useState(0)

  const onPress = useCallback(() => {
    console.log(value)
    setValue(v => v + 1)
  }, [])

  return (
    <BigComponent onPress={onPress} />
  )
}
```
```tsx {4-8}
function Component() {
  const [value, setValue] = useState(0)

  const onPress = useCallback(() => {
    console.log(value)
    setValue(v => v + 1)
  }, [])

  return (
    <BigComponent onPress={onPress} />
  )
}
```
```tsx
function Component() {
  const [value, setValue] = useState(0)

  const onPress = useCallback(() => {
    console.log(value)
    setValue(v => v + 1)
  }, [value])

  return (
    <BigComponent onPress={onPress} />
  )
}
```
````

<!--
Now let's talk about callbacks. Shout out if you see the problem here.

** React to crowd => Nobody did it OR oh wow

1. Well, the deps array needs to have value in it or this callback will become stale when value changes and then we'll an old value.

2. So we add value to the deps and it's fixed! But now onPress changes whenever value changes, and that means BigComponent re-renders whenever value changes. You can't easily see that by looking at it, it's a hidden performance problem.

Callbacks accidentally changing is one of the biggest problems I've seen, as it triggers re-renders down a tree for no reason.
-->

---

# React State Is Oversubscribed

<!--
And finally let's talk about state. React's state is extremely blunt and oversubscribed.
-->

---

```tsx
const [state, setState] = useState({ stuff })
```

<!--
useState doesn't just create state. It also subscribes the current component to that state.

That sounds normal because we're used to it, but it's actually a huge limitation.

The owner of the state HAS to re-render when it changes. And then it's responsible for passing it down.

Ownership and subscription are tied together.
-->

---

```tsx
const { stuff } = useContext(Provider)
```

<!--
Context can be even worse.

useContext doesn't only subscribe to the field you need. It subscribes to the whole context value. Whenever anything in the context value changes, ALL subscribers re-render.
-->

---

````md magic-move
```tsx
function MyText({ text }) {
  const { fontScale } = useWindowDimensions()
  const numberOfLines = fontScale >= 1.3 ? 2 : 1

  return (
    <Text numberOfLines={numberOfLines}>
      {text}
    </Text>
  )
}
```
```tsx
function MyText({ text }) {
  const { fontScale, window, height, scale } = useWindowDimensions()
  const numberOfLines = fontScale >= 1.3 ? 2 : 1

  return (
    <Text numberOfLines={numberOfLines}>
      {text}
    </Text>
  )
}
```
````

<!--
Say you want to get `fontScale` in your text element. This will re-render every text element whenever fontScale changes. That's not great, but that doesn't happen too often, right?

2. TRICK!

You actually subscribed to window size too. So now when an iPad user resizes the window, every single text element re-renders and your app freezes. It doesn't actually need the width, but useContext subscribed to it. So now you have a huge performance problem for no reason.
-->

---

# Rendering too much, too often

<!--
So we know that rendering too much, too often is a major cause of performance problems. But render is the orchestrator of everything,

and React encourages re-rendering often, and spreading those renders deep and wide.
-->

---

<img src="/media/everything-everywhere.gif" class="rounded-lg" />

<!--
So that's the model I want to break.

Instead of rendering everything everywhere all at once
-->

---

# Render once

<!--
We just render once.

We don't even need to change the framework. We just have to think about state differently, and coordinate state separately from render.
-->

---
clicks: 1
---

<div class="flex flex-col gap-6">
    <div class="text-4xl">
        render() => render UI
    </div>
    <div class="text-4xl">
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-25' : 'opacity-100'"
        >render() => </span>
        <span>effects</span>
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-100' : 'opacity-0'"
        > re-run themselves</span>
    </div>
    <div class="text-4xl">
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-25' : 'opacity-100'"
        >render() => </span>
        <span>hooks re-run</span>
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-100' : 'opacity-0'"
        > themselves</span>
    </div>
    <div class="text-4xl">
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-25' : 'opacity-100'"
        >render() => callbacks update</span>
    </div>
    <div class="text-4xl">
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-25' : 'opacity-100'"
        >render() => pass down state</span>
    </div>
</div>

<!--
Currently, render coordinates everything and state owners push the state down.

2. Instead, let's let consumers update themselves as needed. All we need to do to change this paradigm is to re-think state.

And all we need is:
-->

---

# Stable state objects

<br />

```tsx
const replyId$ = useObservable('')

const replyId = useValue(replyId$)
```

<!--
Stable state objects that you can subscribe to

Some people call these signals. Some call em observables. Some call them SharedValues.

The key thing here is that useObservable does not subscribe to the value, it just creates it. Any other component can useValue it to subscribe to the value.
-->

---

```tsx
function ChatScreen() {
  const replyId$ = useObservable('')

  return (
    <View>
        <ChatMessages replyId$={replyId$} />
        <ChatComposer replyId$={replyId$} />
    </View>
  )
}

function ReplyRow({ replyId$ }) {
  const replyId = useValue(replyId$)

  return (
    replyId ?
      <Text>Replying to {replyId}</Text> :
      null
  )
}
```

<!--
This separates ownership from subscription. We lift state ownership up, but push state subscription way down.

ChatScreen passes down stable objects that never change, so it never has to re-render. Then the tiny little ReplyRow subscribes to the state change, and re-renders itself whenever it needs to. It does almost nothing so the cost of updating it is tiny, compared to re-rendering the whole chat screen.
-->

---

<div class="flex flex-col gap-2 pt-8">
  <div class="flex justify-center">
    <div class="box-flashing ml-10">ChatScreen</div>
  </div>
  <div class="flex justify-center items-start">
      <div>
    <div class="box-flashing">ChatMessages</div>
    <div class="flex flex-col flex-1 items-center">
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
    </div>
      </div>
      <div>
        <div class="box-flashing">ChatComposer</div>
      <div class="flex flex-col flex-1 items-center">
        <div class="box-flashing">ReplyRow</div>
        <div class="box-flashing">InputRow</div>
      </div>
      </div>
    <div class="flex justify-center">
        <div class="box-flashing">useEffects</div>
    </div>
  </div>
</div>

<!--
So whereas normally the whole app would re-render because the state is owned and subscribed at the top
-->

---

<div class="flex flex-col gap-2 pt-8">
  <div class="flex justify-center">
    <div class="box-not-flashing ml-10">ChatScreen</div>
  </div>
  <div class="flex justify-center items-start">
      <div>
    <div class="box-not-flashing">ChatMessages</div>
    <div class="flex flex-col flex-1 items-center">
      <div class="box-not-flashing">Message</div>
      <div class="box-flashing">Message</div>
      <div class="box-not-flashing">Message</div>
      <div class="box-not-flashing">Message</div>
    </div>
      </div>
      <div>
        <div class="box-not-flashing">ChatComposer</div>
      <div class="flex flex-col flex-1 items-center">
        <div class="box-flashing">ReplyRow</div>
        <div class="box-not-flashing">InputRow</div>
      </div>
      </div>
    <div class="flex justify-center">
        <div class="box-not-flashing">useEffects</div>
    </div>
  </div>
</div>

<!--
With this setup, only the affected components re-render.

The components that actually use the state subscribe themselves to it and re-render themselves. Render doesn't need to coordinate it and pass it all down.
-->

---

```tsx
const onPressReply = useCallback(() => {
    sendReply(replyToId$.get())
}, [])
```

<!--
If a callback needs a state value, it can just get the current value. It doesn't need anything to render, it doesn't need to be constantly re-created. It can just be a stable function that never changes.
-->

---

```tsx
useObserveEffect(() => {
    if (isOpen$.get()) {
        dialogRef.current?.showModal()
    } else {
        dialogRef.current?.close()
    }
})
```

<!--
An observe effect can just re-run itself when the state it cares about changes. It doesn't need a render to update it. It automatically subscribes to any state it accesses. So we can open a modal without any render coordination.
-->

---

```tsx
function ReplyIdProvider({ children }) {
    const replyState$ = useObservable({ replyId: '', isSending: false })

    return (
        <ReplyIdContext.Provider value={replyState$}>
            {children}
        </ReplyIdContext.Provider>
    )
}

function useIsReplyMessage(messageId) {
  const replyState$ = useContext(ReplyIdContext)

  return useValue(() => replyState$.replyId.get() === messageId)
}
```

<!--
useContext is still very useful, but to provide stable state objects. That way, the context provider never re-renders. And consumers can select which specific thing to listen to, to re-render as little as possible.

We could also subscribe to derived values, so rather than subscribing to replyId which would re-render every single message, this re-renders whenever the boolean return value changes. So it will only re-render the one reply message.
-->

---

# Render once doesn't mean nothing updates

<div class="w-[980px] text-[12px] leading-tight deep-chat-tree">
  <div class="rounded-lg border border-[#32343a] bg-[#111214] p-3">
    <div class="text-center text-xl mb-3">ChatApp</div>
    <div class="rounded-lg border border-[#32343a] bg-[#151619] p-3">
      <div class="text-center text-lg mb-3">ChatScreen</div>
      <div class="grid grid-cols-[180px_1fr_180px] gap-3 items-stretch">
        <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-3">
          <div class="text-center text-base mb-3">InboxSidebar</div>
          <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
            <div class="text-center mb-2">RoomList</div>
            <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
              <div class="text-center mb-2">RoomRow</div>
              <div class="box-flashing-small flash-delay-1">UnreadBadge</div>
            </div>
          </div>
        </div>
        <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-3">
          <div class="text-center text-base mb-3">ConversationView</div>
          <div class="grid grid-cols-[150px_1fr_150px] gap-2 items-stretch">
            <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
              <div class="text-center mb-2">RoomHeader</div>
              <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                <div class="text-center mb-2">Presence</div>
                <div class="box-flashing-small flash-delay-2">OnlineDot</div>
              </div>
            </div>
            <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
              <div class="text-center mb-2">MessageList</div>
              <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                <div class="text-center mb-2">DaySection</div>
                <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
                  <div class="text-center mb-2">MessageCluster</div>
                  <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                    <div class="text-center mb-2">MessageBubble</div>
                    <div class="flex justify-center">
                      <div class="box-flashing-small flash-delay-3">ReplyHighlight</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
              <div class="text-center mb-2">Composer</div>
              <div class="box-flashing-small flash-delay-4">ReplyPreview</div>
              <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                <div class="text-center mb-2">InputRow</div>
                <div class="box-flashing-small flash-delay-5">SendButton</div>
              </div>
            </div>
          </div>
        </div>
        <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-3">
          <div class="text-center text-base mb-3">MembersPanel</div>
          <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
            <div class="text-center mb-2">MemberList</div>
            <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
              <div class="text-center mb-2">TypingStatus</div>
              <div class="box-flashing-small flash-delay-6">TypingIndicator</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!--
So when I say render once I don't mean never update. I mean render the app and large screens once. Let leaf nodes re-render themselves. Let effects re-run themselves. Don't orchestrate through render.

This way your apps will just do a lot less work.
-->

---

<img src="/media/legend-state.png" class="rounded-lg" />

<!--
This is not just a wild theory.

I've actually been building all of my apps and libraries like this for 10 years. If you use my state library Legend State, you probably realized a while ago that I've been describing how it works.
-->

---

<div class="flex flex-col gap-y-8">
    <img src="/media/solid.png" class="rounded-lg max-w-[340px]" />
    <img src="/media/svelte-horizontal.png" class="rounded-lg max-w-[300px]" />
    <img src="/media/preact.png" class="rounded-lg max-w-[300px]" />
</div>

<!--
And this is basically how some other frameworks like Solid, Svelte, and Preact work.
-->

---

<img src="/media/reanimated.png" class="rounded-lg" />

<!--
But it's actually already in React Native too. This is how Reanimated works. A SharedValue is a stable state object. When you get a SharedValue within an observing hook, it subscribes and updates itself automatically. You can set it anywhere in your code, and it will automatically update the UI without a re-render.

So we're actually already doing this to get the best performance with animations.
-->

---

<img src="/media/list-cpu.png" class="rounded-lg" />

<!--
And if you use LegendList, you already have this pattern in your app. This is the main reason LegendList is the fastest list library on both web and mobile. Because it just does less rendering work.
-->

---

<div class="flex items-center">
    <div class="flex flex-col gap-4 pt-8">
        <div class="box-not-flashing">App</div>
    </div>
    <div class="flex flex-col gap-4 pt-8">
        <div class="box-not-flashing">LegendList</div>
    </div>
    <div class="flex flex-col gap-4 pt-8">
        <div class="box-not-flashing">Containers[]</div>
    </div>
    <div class="flex flex-col gap-4 pt-8">
        <div class="container-flashing-1 whitespace-pre">Container 0</div>
        <div class="container-flashing-2 whitespace-pre">Container 1</div>
        <div class="container-flashing-3 whitespace-pre">Container 2</div>
    </div>
    <div class="phone-container">
        <img src="/media/phone.png">
        <div class="absolute inset-x-6 rounded-[32px] top-[22.5px] bottom-[21px] overflow-hidden bg-white">
            <div class="animate-up">
                <div class="box border-none absolute inset-x-4 top-0 h-[290px] bg-red-500 font-medium">
                    Container 0
                </div>
                <div class="box border-none absolute inset-x-4 top-[300px] h-[290px] bg-blue-500 font-medium">
                    Container 1
                </div>
                <div class="box border-none absolute inset-x-4 top-[600px] h-[290px] bg-green-500 font-medium">
                    Container 2
                </div>
                <div class="box border-none absolute inset-x-4 top-[900px] h-[290px] bg-purple-500 font-medium">
                    Container 0
                </div>
                <div class="box border-none absolute inset-x-4 top-[1200px] h-[290px] bg-teal-500 font-medium">
                    Container 1
                </div>
                <div class="box border-none absolute inset-x-4 top-[1500px] h-[290px] bg-pink-500 font-medium">
                    Container 2
                </div>
                <div class="box border-none absolute inset-x-4 top-[1800px] h-[290px] bg-red-500 font-medium">
                    Container 0
                </div>
                <div class="box border-none absolute inset-x-4 top-[2100px] h-[290px] bg-blue-500 font-medium">
                    Container 1
                </div>
            </div>
        </div>
    </div>
</div>

<!--
LegendList mounts a pool of absolutely positioned containers, and never re-renders the array of containers again. It signals individual containers to re-render themselves as needed.

So while you're scrolling down it's signaling individual containers to re-render at a new position with a new item. This keeps the size of renders as small as possible for best scrolling performance.
-->

---

# PositionView

```tsx
function PositionView({ id, ...rest }) {
    const [ position ] = useValue(`containerPosition${id}`)

    return <View style={{ top: position }]} {...rest} />
})
```

<!--
When an item's size changes, a tiny PositionView component re-renders itself with a style change.
-->

---

```tsx
function ContainersSizer({ children }) {
    const animSize: Animated.Value = useValue$("totalSize");
    const style = {
        height: animSize
    }

    return (
        <Animated.View style={style}>
            {children}
        </Animated.View>
    )
}
```

<!--
When the list size changes, it skips rendering entirely and updates an Animated style.

That size changes very often while scrolling as items layout and measure and change the total size.

So it would kill performance if it had to update the outer list component with a new size every time.
-->

---
clicks: 1
---

<h1>
    Render
    <span class="relative inline-block overflow-hidden align-bottom">
        <span class="invisible">less, less often</span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? '-translate-y-full opacity-0' : 'translate-y-0 opacity-100'"
        >
            less, less often
        </span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? 'translate-y-0 opacity-100' : 'translate-y-full opacity-0'"
        >
            once
        </span>
    </span>
</h1>

<!--
LegendList is fast because it's extremely careful do less work. Rendering has a real and significant cost, so for the fastest possible apps, render less, less often.

2. Or, pushed further: render once. Use React to create the structure, then update the smallest leaf nodes directly.
-->

---
clicks: 1
---

<h1>
    Try this
    <span class="relative inline-block overflow-hidden align-bottom">
        <span class="invisible">on Monday</span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? '-translate-y-full opacity-0' : 'translate-y-0 opacity-100'"
        >
            today
        </span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? 'translate-y-0 opacity-100' : 'translate-y-full opacity-0'"
        >
            on Monday
        </span>
    </span>
</h1>

<!--
But this isn't just a list trick. This is the model I want you to take back to your apps.

So try this today. Well, maybe not today since we're at a conference.

2. So try this on Monday.

The first step is to find what's actually slow. Look at your slowest screens and interactions. Just throw logs in all of the components, and see what's rendering so much.
-->

---

<img src="/media/highlight.png" class="rounded-lg" />

<!--
Or you can use the highlight updates feature in dev tools to just watch the renders.
-->


---

# Use a fast state library

<!--
This problem can actually be fully solved with a state library, it doesn't need any framework changes.

But it's honestly tough to do this in raw React.

I obviously recommend Legend State. I'm not aware of others that do all of this, but there are some other signal style state libraries if you prefer. Or you could make your own!

The key thing is you want to decouple state creation and subscription, and push the renders down to the leaf nodes.
-->

---

<img src="/media/say-state.jpg" class="rounded-lg" />

<!--
I know this is controversial, everyone has strong opinions about state. I get a ton of pushback that teams prefer to just use the builtin useState and useContext.
-->

---

<div class="mx-auto grid w-max grid-cols-[250px_max-content] gap-1 text-lg">
  <div class="box-not-flashing-small">❌ Animated</div>
  <div class="box-not-flashing-small">✅ Reanimated</div>
  <div class="box-not-flashing-small">❌ StyleSheet</div>
  <div class="box-not-flashing-small">✅ NativeWind / Uniwind</div>
  <div class="box-not-flashing-small">❌ FlatList</div>
  <div class="box-not-flashing-small">✅ LegendList</div>
  <div class="box-not-flashing-small">❌ AsyncStorage</div>
  <div class="box-not-flashing-small">✅ MMKV / SQLite</div>
  <div class="box-not-flashing-small">❌ SafeAreaView</div>
  <div class="box-not-flashing-small">✅ react-native-safe-area-context</div>
  <div class="box-not-flashing-small">❌ Image</div>
  <div class="box-not-flashing-small">✅ expo-image</div>
  <div class="box-not-flashing-small">❌ TouchableOpacity</div>
  <div class="box-not-flashing-small">✅ Design System</div>
</div>

<!--
But you already use Reanimated.

You dropped raw StyleSheet for Unistyles or NativeWind or Uniwind

If you still have FlatLists in your app talk to me after this.

I hope you're not still using TouchableOpacity.

So why are we clinging onto the built in state and render orchestration workflow?
-->

---

<div class="mx-auto grid w-max grid-cols-[250px_max-content] gap-1 text-xl">
  <div class="box-not-flashing-small">❌ useState</div>
  <div class="box-not-flashing-small">✅ useSyncExternalStore</div>
</div>

<!--
Even if you only want to use built-in state hooks, most modern state libraries are built on useSyncExternalStore, which is a built-in hook, so you're all good.
-->

---

# No, I hate state libraries

For some reason.

<!--
But some teams just refuse to use a state library, or maybe you're a library developer and don't want to add a dependency, which makes sense. So there's also some lower level things you can do to cut out the renders.
-->

---

````md magic-move
```jsx
function Component() {
  const { width } = useWindowDimensions()

  const onClick = useCallback(() => {
    doSomethingWithWindowWidth(width)
  }, [width])

  // ...
}
```
```jsx
function Component() {
  const onClick = useCallback(() => {
    const width = Dimensions.get().width

    doSomethingWithWindowWidth(width)
  }, [])

  // ...
}
```
````

<!--
Try to use imperative APIs rather than hooks when possible, to avoid the re-rendering.

For example instead of re-rendering every time window size changes

2. You can just get screen width when you need it and make this callback stable.
-->

---

```jsx
import { Dimensions, useWindowDimensions } from 'react-native'

const windowWidth = Dimensions.get("window").width

Dimensions.addEventListener('change', ({ window }) => {
  windowWidth$.set(e.window.width))
})
```

<!--
And if you're a library author, please support an imperative API as well as a hook, like React Native's Dimensions has.

Then users can get the value when they need it, or use an event listener to hook it up to their own state, and don't need to run it through a render.
-->

---

```jsx
function ChatScreen() {
    const widthRef = useWindowWidthRef((width) => {
        console.log('width changed');
    })

    const onPress = () => {
        doSomethingWithWidth(widthRef.current);
    }

    // ...
}
```

<!--
Or another pattern I've used is to make hooks take a callback function and return a ref. Then users can use the ref when they need to get the value in a callback or pass it down a tree, or they can just listen to changes with the callback and use it how they want.

It never sets state so it doesn't need a render.
-->

---

```jsx
import useLatestCallback from 'use-latest-callback';
import { useEventCallback } from 'usehooks-ts'

function Component() {
  const { width } = useWindowDimensions()

  const onClick = useLatestCallback(() => {
    doSomethingWithWindowWidth(width)
  })

  // ...
}
```

<!--
You can change your callbacks to useLatestCallback instead of useCallback, which updates them without dependancy arrays. I've seen a few different versions of this, and I've seen a lot of teams rolled their own.

This makes callbacks perfectly stable so they don't trigger render cascades down the tree.
-->

---

```jsx
import { createContext, useContextSelector } from 'use-context-selector';

function Component() {
    const width = useContextSelector(windowContext, (window) => window.width);

    // ...
}
```

<!--
If you really want to stick with context but just not have the terrible performance problems that come with it, you could use something like useContextSelector. It still re-renders on changes, but at least lets you select only the part you care about.

But these are mostly small fixes to the symptoms.
-->

---

<img src="/media/deep-impact.jpg" class="rounded-t-3xl rounded-b-xl" />

<!--
The deepest impact we can get is from rethinking state at a higher level.
-->

---

<div class="w-[980px] text-[12px] leading-tight deep-chat-tree">
  <div class="rounded-lg border border-[#32343a] bg-[#111214] p-3">
    <div class="text-center text-xl mb-3">ChatApp</div>
    <div class="rounded-lg border border-[#32343a] bg-[#151619] p-3">
      <div class="text-center text-lg mb-3">ChatScreen</div>
      <div class="grid grid-cols-[180px_1fr_180px] gap-3 items-stretch">
        <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-3">
          <div class="text-center text-base mb-3">InboxSidebar</div>
          <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
            <div class="text-center mb-2">RoomList</div>
            <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
              <div class="text-center mb-2">RoomRow</div>
              <div class="box-flashing-small flash-delay-1">UnreadBadge</div>
            </div>
          </div>
        </div>
        <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-3">
          <div class="text-center text-base mb-3">ConversationView</div>
          <div class="grid grid-cols-[150px_1fr_150px] gap-2 items-stretch">
            <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
              <div class="text-center mb-2">RoomHeader</div>
              <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                <div class="text-center mb-2">Presence</div>
                <div class="box-flashing-small flash-delay-2">OnlineDot</div>
              </div>
            </div>
            <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
              <div class="text-center mb-2">MessageList</div>
              <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                <div class="text-center mb-2">DaySection</div>
                <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
                  <div class="text-center mb-2">MessageCluster</div>
                  <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                    <div class="text-center mb-2">MessageBubble</div>
                    <div class="flex justify-center">
                      <div class="box-flashing-small flash-delay-3">ReplyHighlight</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
              <div class="text-center mb-2">Composer</div>
              <div class="box-flashing-small flash-delay-4">ReplyPreview</div>
              <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
                <div class="text-center mb-2">InputRow</div>
                <div class="box-flashing-small flash-delay-5">SendButton</div>
              </div>
            </div>
          </div>
        </div>
        <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-3">
          <div class="text-center text-base mb-3">MembersPanel</div>
          <div class="rounded-lg border border-[#32343a] bg-[#111214] p-2">
            <div class="text-center mb-2">MemberList</div>
            <div class="rounded-lg border border-[#32343a] bg-[#17181a] p-2">
              <div class="text-center mb-2">TypingStatus</div>
              <div class="box-flashing-small flash-delay-6">TypingIndicator</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!--
High level orchestrating components should never re-render. They should own the state but not subscribe to it. Re-renders should be pushed down to the leaf nodes.

I know this is a departure from normal React patterns.
-->

---

<img src="/media/state-architecture.jpg" class="rounded-lg" />

<!--
But in my experience it really does make apps a lot faster. I think it's worth a try.

I believe state architecture is by far the biggest hidden bottleneck in most apps, so you should care about it and optimize
-->

---

<img src="/media/bananas.jpg" class="rounded-lg" />

<!--
the bananas out of it.

I care so much about this I built a whole state and sync library because it was the only way to get the best performance.
-->

---
clicks: 1
---

<h1>
    <span>R</span>
    <span class="relative inline-block overflow-hidden align-bottom">
        <span class="invisible">ender once.</span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? '-translate-y-full opacity-0' : 'translate-y-0 opacity-100'"
        >
            EALLY FAST
        </span>
        <span
            class="absolute left-0 transition-all duration-500 ease-out"
            :class="$clicks >= 1 ? 'translate-y-0 opacity-100' : 'translate-y-full opacity-0'"
        >
            ender once.
        </span>
    </span>
</h1>

<!--
This architecture in my opinion is how you get from "fast for React Native" to REALLY FAST.

2. Render once.
-->
