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

# React Native is not

# the compromise anymore

</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div>@jmeistrich</div>
  <div class="text-gray-400">Bravely, Legend, Margelo</div>
</div>

<!--
I'm here to talk about performance. But I'm not going to talk about how to do profiling or little optimization tricks. I want to talk about making apps that are REALLY FAST.
-->

---

# REALLY FAST

<!--
I want to talk about making apps that are REALLY FAST.
-->

---

<SlidevVideo src="/media/v0.mp4" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[520px]" />

<!--
I worked with Fernando on animations and list performance for the v0 iOS app. It's a beautiful AI chat app that feels incredible. When it was announced people couldn't believe it was React Native.
-->

---

<SlidevVideo src="/media/md-open.mov" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[520px]" />

<!--
Here's a React Native macOS app I'm working on. It opens a 10MB markdown file in 20 milliseconds.
-->

---

<div class="flex items-center justify-center gap-2">
  <img src="/media/md-1-start.png" class="rounded-lg max-h-[260px]" />
  <img src="/media/md-2-loading.png" class="rounded-lg max-h-[260px]" />
  <img src="/media/md-3-loaded.png" class="rounded-lg max-h-[260px]" />
</div>

<!--
Here's every single frame of the video from open to being fully ready.
-->

---

<SlidevVideo src="/media/md-zed.mov" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[520px]" />

<!--
For comparison, here's Zed loading the same file. Most other apps I tried froze until they crashed.
-->

---

<SlidevVideo src="/media/visaurora.mov" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[520px]" />

<!--
Here's another one. It's an mp3 player app.
-->

---

<div class="flex items-center justify-center gap-8">
  <img src="/media/musiccpu.png" class="rounded-lg max-h-[340px]" />
  <img src="/media/musicmemory.png" class="rounded-lg max-h-[340px]" />
</div>

<!--
It uses less CPU and memory than every other music app.
-->

---

<SlidevVideo src="/media/photos.mov" autoreset="slide" autoplay mute loop class="rounded-lg max-h-[520px]" />


<!--
And here's a photo app I prototyped. It opens my photos library instantly and flies through fullscreen jpgs. It turns the photos into basically a movie.
-->

---

# React Native macOS is 🔥 {.inline-block.view-transition-title}

<!--
These are all React Native apps and they're faster than every other app on my computer.
-->

---

# React Native in 2026 is 🔥 {.inline-block.view-transition-title}

---

# React Native in 2026 is 🔥 {.inline-block.view-transition-title}

<div class="mx-auto w-[160px]">

New Architecture

Hermes

Nitro Modules

Lists

Redraw

Dev Tools

</div>

<!--
In 2026 the game has totally changed.

New architecture unlocks big possibilities. Hermes keeps getting better. Nitro Modules are crazy fast. Lists are no longer terribly slow. Redraw makes incredible graphics easy and fast. The new dev tooling is awesome.
-->

---

# React Native is a compromise?

<!--
But still, React Native is seen as a compromise.

I don't think that's true. I've seen and worked on React Native apps that are fully best in class.

React Native is not slow. The way we use it is.
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

Render once.

Use React to create the structure, then update the smallest leaf nodes directly.
-->

---

# Why is rendering too much a problem?

<!--
But first, why is rendering too much a problem? Everyone talks about reducing renders, but what's the actual impact?
-->

---

# render = Component function

```tsx
function Component(props) {
    const [state, setState] = useState(false);

    return (
        <View>
            <Text>Hi</Text>
        </View>
    )
}
```

<!--
And by render, I mean your function component. It gets called over and over to render UI, run hooks, etcetera.
-->

---

# Render more vs. render less

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
    .slidev-code-wrapper {
        width: 310px !important;
    }
}
</style>

</div>

<!--
We know that for best performance we should render less often, but it's also important to render less stuff. But how much?

I did a little benchmark test in my music app to compare re-rendering at the app root vs. updating a single label.
-->

---

# CPU

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
Even I was surprised.

First and obviously, reducing the update frequency reduced the CPU usage.

But then dropping that update target from the top level down to the tiniest leaf node made a huge difference, reducing the CPU usage by X%.

So the best thing to do is render less
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
We use render to coordinate the whole app:

- local state to pass down the tree
- update useContext consumers
- trigger effects
- update query properties
- external properties
- update subscriptions
- and actually render UI

That's a lot of responsibility for "render". And that's where so many normal React patterns become a problem.
-->

---

# React encourages re-rendering

<!--
We know that big renders are bad for performance, but React's design encourages more rendering
-->

---

<img src="/media/chat-without-reply.png" class="rounded-lg" />

<!--
For example let's say we have a chat message with a reply feature which changes the background color. That's cool, just re-render the message to update the color.
-->

---

<img src="/media/chat-with-reply.png" class="rounded-lg" />

<!--
And then you want to also show in the composer that you're replying. But oh no, now we need to access the same state in sibling components. So what do you do? Say it with me!
-->

---

<img src="/media/lifting-state-up.png" class="rounded-lg" />

<!--
Lift state up!

Nobody did it, that's okay.

You do what the React docs tell you. You lift state up. And it's one of the most common things you'll do writing React code.
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

It re-renders the chat screen. It re-renders the composer with all of its dialogs and buttons and hooks. It re-renders everything down to the list. And then it re-renders every chat message. So that one message can change a background color.

That kills performance.
-->

---

# React Compiler!

<!--
And React Compiler helps a lot, but it can't save you from legitimate changes. The `replyId` changed. Props changed. Wherever a prop is changed, it has to render. To update a style in one message we have to cascade state changes through the whole screen.
-->

---

<img src="/media/not-need-useeffect.webp" class="rounded-lg" />


<!--
Now let's talk about effects. We all know the "You might not need useEffect" meme, but it is still a core tool for orchestrating apps that you can't really avoid.
-->

---

```tsx
const [isOpen, setIsOpen] = useState(false)

useEffect(() => {
    if (isOpen) {
        dialogRef.current?.showModal()
    }
}, [isOpen])

return (
  <Pressable onPress={() => setIsOpen(true)} />
)
```

<!--
Here's an example from the React docs: controlling a modal dialog. To open a dialog on button click, we could just open the dialog. But the React pattern is set state which triggers a render, then use an effect to open the dialog.

It's not actually changing what renders. But it re-renders anyway.
-->

---

```tsx
const [isPaused, setIsPaused] = useState(false)

const { data } = useQuery({
    queryKey,
    refetchInterval: isPaused ? false : 1000,
})

const pauseQuery = () => {
  setIsPaused(true)
}
```

<!--
Or maybe I want to pause query polling temporarily. I have to set a state to call the useQuery hook again with different parameters.

Setting isPaused doesn't change what renders. But we have to re-render to change it.
-->

---

```tsx
const isFocused = useIsFocused()

useEffect(() => {
  if (isFocused) {
    markConversationRead(conversationId)
  }
}, [isFocused, conversationId])
```

<!--
Or I want to mark this chat as read when the user is focused on the chat. Nothing visible changed here. I just need to run an imperative action when the screen becomes focused. And that requires going through render for the side effect, and now this screen re-renders every time focus changes..
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
    <MyBigComponent onPress={onPress} />
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
    <MyBigComponent onPress={onPress} />
  )
}
```
````

<!--
Now let's talk about callbacks. Shout out if you see the problem here.

** React to crowd => Nobdoy did it, oh wow

The deps array needs to have value in it or this callback will become stale when value changes.

So we add value to the deps and it's fixed! But now the identity of onPress changes whenever value changes, so we have re-render the whole MyBigComponent. It doesn't actually change what's rendered, but we have to re-render to do update the callbacks.
-->

---

# deps array ???

```tsx
const onPress = useCallback(() => {
    // ...
}, [value, thingRef, otherValue, otherRef])
```
<!--
As a side note, dependency arrays are just the most maddening thing.

It's a puzzle I have to solve every time I look at it. Are all deps stable? Maybe one can change sometimes? But when does it change? What render cascade will we see when one of the deps changes?
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

That sounds normal because we're used to it, but it's actually a big limitation.

The owner of the state HAS to re-render when it changes. And then it's responsible for passing it down.

Ownership and subscription are tied together.
-->

---

```tsx
const { stuff } = useContext(Provider)
```

<!--
Context can be even worse.

useContext doesn't subscribe to the field you need. It subscribes to the whole context value. Whenever anything in the context value changes, ALL subscribers re-render.
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
Say you want to get `fontScale` in your text element. This will re-render whenever fontScale changes, but that doesn't happen too often, right?

TRICK!

You actually subscribed to window width and height too. So now when an iPad user resizes the window, every single text element re-renders and your app freezes. It doesn't actually need the width, but useContext subscribed to it. So now you have a huge performance problem for no reason.
-->

---

# Rendering too much, too often

<!--
So we know that rendering too much, too often is a major cause of performance problems. But render is the orchestrator of everything, and React encourages re-rendering often, and spreading those renders deep and wide.
-->

---

Everything everwhere all at once video

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
Currently, render coordinates everything and producers push state down, through props and deps arrays.

Instead, let's let consumers update themselves as needed. All we need to do to change this paradigm is to re-think state.
-->

---

# Stable state objects

<br />

```tsx
const replyId$ = useObservable('')

const replyId = useValue(replyId$)
```

<!--
And that means: stable state objects that you can subscribe to

Some people call this signals. Some people call it observables.

The key thing here is that useObservable does not subscribe to the value, it just creates it. Any other component can useValue it to subscribe to the value.

I just use a $ suffix to clearly mark observables, there's no compilation or anything that needs that.
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
    <Text>Replying to {replyId}</Text>
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
So whereas normally the whole app re-renders
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
An observe effect can just re-run itself when the state it cares about changes. It doesn't need a render to update it. It automatically subscribes to any state it gets. So we can open a modal without any render coordination.
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
And this is not just a wild theory. I build all of my apps and libraries like this. If you use my state library Legend State, you probably realized a while ago that I've been describing how it works.
-->

---

<div class="flex flex-col gap-y-8">
    <img src="/media/solid.png" class="rounded-lg max-w-[340px]" />
    <img src="/media/svelte-horizontal.png" class="rounded-lg max-w-[300px]" />
    <img src="/media/preact.png" class="rounded-lg max-w-[300px]" />
</div>

<!--
And this is how some other frameworks like SolidJS, Svelte, and Preact work.
-->

---

<img src="/media/reanimated.png" class="rounded-lg" />

<!--
But it's actually already in React Native. This is how Reanimated works. A SharedValue is a stable stage object. When you get a SharedValue within an observing hook, it subscribes and updates itself automatically. You can set it anywhere in your code, and it will automatically update the UI without a re-render.

So we're already doing this in order to reach the best performance with animations.
-->

---

<img src="/media/list-cpu.png" class="rounded-lg" />

<!--
And if you use LegendList, you already have this pattern in your app. This is how LegendList is the fastest list library on both web and mobile.
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
                <div class="box border-none absolute inset-x-8 top-0 h-[290px] bg-red-500 font-medium">
                    Container 0
                </div>
                <div class="box border-none absolute inset-x-8 top-[300px] h-[290px] bg-blue-500 font-medium">
                    Container 1
                </div>
                <div class="box border-none absolute inset-x-8 top-[600px] h-[290px] bg-green-500 font-medium">
                    Container 2
                </div>
                <div class="box border-none absolute inset-x-8 top-[900px] h-[290px] bg-purple-500 font-medium">
                    Container 0
                </div>
                <div class="box border-none absolute inset-x-8 top-[1200px] h-[290px] bg-teal-500 font-medium">
                    Container 1
                </div>
                <div class="box border-none absolute inset-x-8 top-[1500px] h-[290px] bg-pink-500 font-medium">
                    Container 2
                </div>
                <div class="box border-none absolute inset-x-8 top-[1800px] h-[290px] bg-red-500 font-medium">
                    Container 0
                </div>
                <div class="box border-none absolute inset-x-8 top-[2100px] h-[290px] bg-blue-500 font-medium">
                    Container 1
                </div>
            </div>
        </div>
    </div>
</div>

<!--
LegendList mounts a pool of absolutely positioned containers, and never re-renders the array of containers again. It signals individual containers to re-render themselves as needed.

So while you're scrolling down it's signaling individual containers to re-render at a new position with a new item. It results in the same thing on screen, but it skips a bunch of work in the middle.
-->

---

# PositionView

```tsx
function PositionViewState({ id, ...rest }) {
    const [position] = useValue(`containerPosition${id}`);

    return <View style={{ top: position }]} {...rest} />;
})
```

<!--
When an item's size changes, a tiny PositionView component re-renders itself with a style change.
-->

---

```tsx
function ContainersSizer({ children }) {
    const animSize: Animated.Value = useValue$("totalSize");
    const style={
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
When the list size changes, it skips React entirely and updates an Animated style. That size changes very often while scrolling as items layout and measure. So it would kill performance if it had to update the outer list component with a new size every time.
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

Or, pushed further: render once. Use React to create the structure, then update the smallest leaf nodes directly.
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

So try this on Monday.

Look at your slowest screens. Find the components doing the most work, and trace how often they re-render.

Find one state change that causes a big render cascade and clean it up.
-->

---

# Use a state library

<!--
Use a state library. It's honestly tough to do this in raw React. I obviously recommend Legend State. Or there's some other signal style state libraries if you prefer. The key thing is you want to decouple state creation and subscription, and push the renders down to the leaf nodes.
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
  <div class="box-not-flashing-small">✅ Nativewind / Uniwind</div>
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
But you use Reanimated instead of Animated, Nativewind or Uniwind instead of StyleSheet, LegendList instead of FlatList, MMKV instead of AsyncStorage, Safe area context, expo image, a design system - I hope you're not using TouchableOpacity. So why are we clinging onto the built in state and render orchestration workflow?
-->

---

<img src="/media/state-architecture.jpg" class="rounded-lg" />

<!--
IMHO state architecture is by far the biggest hidden bottleneck in most apps, so that's what you should care about and optimize the most. I care so much about this I built a whole state and sync library because that was the only way to get the best performance.
-->

---

# REALLY FAST

<!--
That's how you get from "fast for React Native" to REALLY FAST.
-->

---

# Render once

<!--
Render once.
-->
