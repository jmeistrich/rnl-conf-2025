---
theme: default
title: Fast by Default - Performance Starts with State
highlighter: shiki
transition: none
comark: true
mdc: true
duration: 20min
defaults:
  layout: center
  transition: view-transition
---

<div class="pb-12">

# Fast by Default: Legend State

</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div>@jmeistrich</div>
  <div>Legend, Margelo</div>
</div>

<!--
Hi everyone! I'm Jay. This is my third time at Chain React and I'm really excited to finally be doing a talk here!

You might know me from LegendList. And I work at Margelo helping companies performance optimize their apps. And let me tell you, I've seen some things.
-->

---

LegendList video

<!--
I spent years building the fastest list library to make apps faster, but a slow list is actually not the biggest performance problem in most apps.
-->

---

MEME?

# Top 10 Performance Problems

<div class="text-3xl text-center pt-8">

<span>1. State</span>

</div>

<!--
It's state.

Most of us don't even think about it. We use the built in useState and useContext.
-->

---

<div class="text-3xl pt-8">

1. React Compiler
2. useCallback
3. useMemo

</div>

<!--
Then just throw in useCallback and useMemo and enable the compiler, and assume that's good enough.
-->

---

# React State is slow by default

<!--
But you just fundamentally can't get the best performance with the built in state.
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
        <div class="absolute inset-x-6 rounded-[16px] top-[22.5px] bottom-[21px] overflow-hidden bg-white">
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
The main reason that LegendList is fast is that it skips as much React work as possible.

It never re-renders the outer list, or the array of containers. It signals individual items to re-render themselves as they come onto screen.
-->

---

# Render less, less often

<!--
The key to building the fastest apps is to minimize the amount of work that React and React Native do.

That means having smaller renders and doing it less often.

useState and useContext are fundamentally incompatible with that.
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

function ChatMessage({ id, replyId }) {
  const isReply = id === replyId

  // ...
}
```

<!--
useState both creates and subscribes to state. So whenever the state changes, it re-renders where it was created. If you have state at a high level, it has to pass it all the way down.
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
  </div>
</div>

<!--
And it re-renders every component all the way down.

Compiler and memoizing can't save you here. The value is actually changing, so React has to do a ton of work rendering and reconciling everything.

That kills performance.
-->

---

```tsx
function ChatScreen() {
  const [replyId, setReplyId] = useState('')

  return (
    <ReplyContextProvider value={{ replyId, setReplyId }}>
      <View>
        <ChatMessages />
        <ChatComposer />
      </View>
    </ReplyContextProvider>
  )
}

function ChatMessage({ id, replyId }) {
  const isReply = id === replyId

  // ...
}
```

<!--
Context can be better in some cases, it doesn't have to pass down the tree.
-->

---

<div class="flex flex-col gap-2 pt-8">
  <div class="flex justify-center">
    <div class="box-flashing ml-10">ChatScreen</div>
  </div>
  <div class="flex justify-center items-start">
      <div>
    <div class="box-not-flashing">ChatMessages</div>
    <div class="flex flex-col flex-1 items-center">
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
      <div class="box-flashing">Message</div>
    </div>
      </div>
      <div>
        <div class="box-not-flashing">ChatComposer</div>
      <div class="flex flex-col flex-1 items-center">
        <div class="box-not-flashing">ReplyRow</div>
        <div class="box-not-flashing">InputRow</div>
      </div>
      </div>
  </div>
</div>

<!--
But instead of going deep it goes wide. Every time the value changes it re-renders every instance of useContext.
-->

---

```tsx
function MyText({ text }) {
  const { fontScale, /*width, height*/ } = useWindowDimensions()
  const numberOfLines = fontScale >= 1.3 ? 2 : 1

  return (
    <Text numberOfLines={numberOfLines}>
      {text}
    </Text>
  )
}
```

<!--
I've seen this absolutely destroy performance. If any value in a context changes, it re-renders everything, even if it isn't used.

For example, useWindowDimensions re-renders constantly while resizing a window, even if you don't use the window size.

So you could split that out into one concern per provider.
-->

---

```tsx
function App() {
  return (
    <WidthProvider>
      <HeightProvider>
        <FontScaleProvider>
          <ScaleProvider>
            <ReplyProvider>
              <AttachmentProvider>
                <PhotoProvider>
                  <UploadProvider>
                    <MyActualApp />
                  </UploadProvider>
                </PhotoProvider>
              </AttachmentProvider>
            </ReplyProvider>
          </ScaleProvider>
        </FontScaleProvider>
      </HeightProvider>
    </WidthProvider>
  )
}
```

<!--
But then you end up with these massive provider trees. And they still re-render too much anyway.
-->

---

<img src="/media/noonecares.webp" class="rounded-lg" />

<!--
But how much does this matter? How big of a deal is some extra renders? The React docs say to do this, so it must be fine.

Right?
-->

---

<img src="/media/legend-state.png" class="rounded-lg" />

<!--
Well, we need a good benchmark to know that.

Legend State beats all other state libraries in popular benchmarks, but I wanted a more realistic way to test it.
-->

---

# Benchmark

<br />
<div class="flex three-column-code gap-4">

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

    code {
        font-size: 12px !important;
        line-height: 18px !important;
    }
}
</style>

</div>

<!--
I did a little benchmark based on a music app I'm making, which updates the timestamp twice a second based on state coming from a native module. In the first one it passes state down from the root. Or it owns the state in a medium size component in the middle of the tree. Or just a single text element leaf node updates itself.

It's a small difference, right?
-->

---

# CPU Usage

<div class="flex gap-2 mt-10 text-left">
  <div class="box-not-flashing flex-1 flex-col !items-start">
    <div class="text-xl text-gray-400">App</div>
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

Re-rendering at the root is 10 times slower.

In my adventures consulting on optimizing apps and list performance I see this problem over and over. Huge trees of components re-render for no real reason. That makes apps feel slow.

This is actually very easy to fix.
-->

---

# Use a State Library

<!--
Use a state library. That's it.
-->

---

<img src="/media/state-meme.jpeg" class="rounded-lg" />

<!--
But I've worked with many companies who say the built-in state is fine, and they don't want to add a dependency.
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
But you already use Reanimated instead of the builtin Animated because it's better.

You dropped raw StyleSheet for Unistyles or NativeWind or Uniwind

If you still have FlatLists in your app, please talk to me.

I hope you're not still using TouchableOpacity.

So why are you clinging onto the built in state?
-->

---

# Use a State Library

<!--
So seriously, please use a state library. They exist because they're better for performance, or they have a better developer experience, or they scale better.

Just about any popular state library will be a huge win over useState and useContext.

But for the absolute best possible performance you want...
-->

---

# Fine-grained reactivity

<div class="flex two-column-code gap-4 pt-8">

```tsx
function ChatMessage({ id }) {
  // Every message re-renders

  const replyId = useReplyId()
  const isReply = id === replyId

  return (
    <View>
      {isReply && <ReplyRow />}
      <MessageContent />
    </View>
  )
}
```

````md magic-move
```tsx
function ChatMessage({ id }) {
  // 1 message re-renders

  const isReply = useValue(() => id === replyId$.get())

  return (
    <View>
      {isReply && <ReplyRow />}
      <MessageContent />
    </View>
  )
}
```
```tsx
function ChatMessage({ id }) {
  // No re-renders

  return (
    <View>
      <Memo>
        {() => id === replyId$.get() && <ReplyRow />}
      </Memo>
      <MessageContent />
    </View>
  )
}
```
````

</div>

<style>
.two-column-code {
    align-items: flex-start;

    .slidev-code-wrapper::before {
        display: block;
        margin-bottom: 8px;
        color: #9ca3af;
        font-size: 16px;
        font-weight: 500;
    }

    .slidev-code-wrapper:nth-of-type(1) {
        width: 400px !important;
    }

    .slidev-code-wrapper:nth-of-type(2) {
        width: 520px !important;
    }

    .slidev-code-wrapper:nth-of-type(1)::before {
        content: "Lame";
    }

    .slidev-code-wrapper:nth-of-type(2)::before {
        content: "Fine-grained";
    }

    code {
        font-size: 12px !important;
        line-height: 18px !important;
    }
}
</style>

<!--
Fine grained reactivity

That means re-renders are targeted at the smallest leaf nodes to keep re-renders small and rare.

In the lame way on the left, every message re-renders when the replyId changes. With fine-grained selectors only one message re-renders.

But even this isn't ideal, the whole ChatMessage shouldn't need to re-render, it's only changing whether one component displays or not.

2. So we can use this LegendState feature, the Memo component, which runs a function in a tiny wrapper component. So now only the ReplyRow toggles, without re-rendering the whole message component.
-->

---

<img src="/media/state-meme.webp" class="rounded-lg" />

<!--
Before I even started on LegendList, Legend State was my first love. To try to get the best performance in my app Legend, I was trying to optimize re-renders, and found that it's caused directly by state. I tried every state library and they couldn't support the right architecture, so I tried to figure out how to build the fastest possible state system.
-->

---

<img src="/media/jackie-chan.webp" class="rounded-lg" />


<!--
I blacked out for a week. I have no memory of that week but git shows that I tried 18 different approaches. I tried dozens of state libraries, studied their source, and ran a bunch of benchmarks, trying to find the fastest possible solution with the best developer experience.

And this was before AI, so this was real typing code.
-->

---
clicks: 1
---

<div class="flex flex-col gap-6 pt-8">
    <div class="text-4xl">
        render() => render UI
    </div>
    <div class="text-4xl">
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-25' : 'opacity-100'"
        >render() => </span>
        <span>effects re-run</span>
        <span
            class="transition-opacity duration-500"
            :class="$clicks >= 1 ? 'opacity-100' : 'opacity-0'"
        > themselves</span>
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
What I ended up with was a fundamental architecture change. React's model depends on render to run effects, update dependency arrays, re-run hooks, and pass down state.

So if we're going to reduce renders we need to change the whole model.

Currently, render coordinates everything.

2. Instead, let's let consumers update themselves as needed.
-->

---

# Stable state objects

<br />

```tsx
// Global
const replyId$ = observable('')
const replyId = replyId$.get()

function Component() {
  // Local
  const replyId$ = useObservable('')
  const replyId = useValue(replyId$)
}
```

<!--
To do that we just need stable state objects that we can subscribe to.

Some people call these signals. I call them observables.

The key thing here is that useObservable does not subscribe to the state, it just creates it. Any other component can useValue it to subscribe to it.
-->

---

# Primitives or stores

```tsx
const store$ = observable({
  settings: {
    theme: 'light',
    fontSize: 14
  },
  messages: [],
  users: new Map<string, User>()
})

store$.settings.theme.get()
store$.users.get('userId')
store$.messages[4].messageData.userName.get()
```

<!--
Observables can be primitives or they can be large objects or stores. It's infinitely nested, so you can access any value anywhere inside any object.
-->

---

# Observe

```tsx
const store$ = observable({
  settings: {
    theme: 'light',
  }
})

observe(() => {
  const isLight = store$.settings.theme.get() === 'light'
})

function Component() {
  const isLight = useValue(() => store$.settings.theme.get() === 'light')
}
```

<style>
.slidev-code-wrapper {
    width: 720px !important;
}
</style>

<!--
Observing contexts auto track dependencies, so just getting a value is all you need to do to set up subscription.
-->

---

# Set observables

```tsx
const store$ = observable({
  settings: {
    theme: 'light',
  }
})

store$.settings.theme.set('dark')

store$.settings.assign({ theme: 'dark' })

store$.settings.theme.set((prev) =>
  prev === 'light' ? 'dark' : 'light
)
```

<!--
Setting a value is easy, you can set or assign any value. It even has that familiar callback version like React.
-->

---

# No deps


```tsx
const onPressReply = useCallback(() => {
    sendReply(replyToId$.get())
}, [])

useObserveEffect(() => {
    if (isOpen$.get()) {
        dialogRef.current?.showModal()
    } else {
        dialogRef.current?.close()
    }
})
```

<!--
Then callbacks can be stable because they don't depend on any local state. Effects can re-run automatically whenever an observable changes. We don't need to coordinate it through render.
-->

---

## Two-way bind to forms

```tsx
function App() {
    // Never re-renders
    const state$ = useObservable({ user: { name: '', email: '' }})

    const onPressSubmit = () => {
        const user = state$.user.get()
        submit(user)
    }

    return (
        <Form>
            <Label>Name</Label>
            <$TextInput $value={user$.name} />
            <Label>Email</Label>
            <$TextInput $value={user$.email} />
            <Button onPress={onPressSubmit}>
        </Form>
    );
}
```

<!--
Legend State has components that can two-way bind to observables without going through a render. This can be a big speed improvement for forms, which don't have to set state anywhere or re-render.

Just bind inputs to observables and they automatically update while typing.

This has great performance because the whole form never re-renders, just the inputs themselves. But it's also just less cumbersome and less code.
-->

---

## Two-way bind to device APIs

```tsx
const brightness$ = observable(
  synced({
    get: () => Brightness.getBrightnessAsync(),
    set: ({ value }) => Brightness.setBrightnessAsync(value),
    subscribe: ({ update }) => {
      Brightness.addBrightnessListener(({ brightness }) => {
        update(brightness)
      })
    },
  }),
);

function BrightnessSettings() {
  // Never re-renders
  return (
    <Slider $value={brightness$} />
  );
}
```

<!--
We can two-way bind to any external data too. So an observable defines its link to an external API and then your components don't need to know anything about it. Just bind your UI to the brightness and it will sync itself.
-->

---

# Render Once

```tsx
function ThisComponentRendersOnce() {
  const text$ = useObservable('')

  return (
    <>
      <$TextInput $value={text$} />

      <Text>
        <Memo>{() => text$.get()}</Memo>
      </Text>

      <$Text
          $style={() => ({ backgroundColor: text$.get() ? 'black' : 'red' })}
      >
        {() => text$.get()}
      </$Text>
    </>
  )
}
```

<style>
.slidev-code-wrapper {
    width: 720px !important;
}
</style>

<!--
And so if we change the model to not rely on render, we can make components that only render once.

So Legend State has components that can two-way bind to observables without going through a render. We can Memo to extract a re-render to a tiny component that just returns a string.

We can have reactive props that update themselves.

The component never has to re-render.
-->

---

<img src="/media/noonecares.webp" class="rounded-lg" />

<!--
This is cool and all, but it's not magic, it's just a state library.

This is possible to get close to in React, but it's hard. It's possible in other state libraries but takes more thinking and boilerplate.

We want fast by default. So let's look at how to do it in React and popular state libraries.
-->

---

# React useState

```tsx
function MusicApp() {
  const [playback, setPlayback] = useState(initialPlayback)

  return (
    <TrackList activeTrackId={playback.activeTrackId} />
  );
}

function TrackList({ activeTrackId }) {
  return tracks.map((track) => (
    <TrackRowWrapper key={track.id} track={track} activeTrackId={activeTrackId} />
  ));
}

function TrackRowWrapper({ track, activeTrackId }) {
  const isActive = activeTrackId === track.id

  return <TrackRowView track={track} isActive={isActive} />
}
```

<style>
.slidev-code-wrapper {
    width: 720px;
}
</style>

<!--
We'll look at an example of a music app changing the active song. All we need to do is change the background color on 2 tracks, the previous track and the new one.

With useState we can't really optimize it much. We can make a wrapper component that checks if it's active and passes that down. But the active track id has to pass down the track list and all items. But then we do achieve avoiding a full track render except for the two that changed with a memoized track row view.
-->

---

# React Context

```tsx
const PlaybackContext = createContext(initialPlayback)

function PlaybackStateProvider({ children }) {
  const [playback, setPlayback] = useState(initialPlayback)

  return (
    <PlaybackContext value={playback}>
      {children}
    </PlaybackContext>
  )
}

function TrackRowWrapper({ track }) {
  const playback = useContext(PlaybackContext)
  const isActive = playback.activeTrackId === track.id

  return <TrackRowView track={track} isActive={isActive} />
}
```

<!--
With Context it still needs the wrapper component to check isActive. It doesn't pass the state all the way down, but it still has to render every wrapper to check if it's active. So as with state, it's still going to render every track, but at least it's not the whole tree between the state and the track.
-->

---

# useSyncExternalStore

<div class="flex gap-4">

```tsx
let playback = initialPlayback
const listeners = new Set()

function subscribe(listener) {
    listeners.add(listener)
    return () => listeners.delete(listener)
}
function setPlayback(nextPlayback) {
    if (!Object.is(playback, nextPlayback)) {
        playback = nextPlayback
        listeners.forEach((listener) => listener())
    }
}
function usePlaybackSelector(selector) {
    return useSyncExternalStore(
        subscribe,
        () => selector(playback),
    )
}
```

```tsx
function TrackRow({ track }) {
    const isActive = usePlaybackSelector(
        (state) => state.activeTrackId === track.id,
    )

    return (
        <View style={[styles.row, isActive && styles.activeRow]}>
            <Text>{track.title}</Text>
        </View>
    )
}
```

</div>

<style>
.slidev-code-wrapper {
    width: 480px;
}
</style>

<!--
Some teams are so resistant to a state library that they build their own stores on top of useSyncExternalStore.

Don't do this. It's a ton of boiler plate. You'll end up building your own state library.

But this does finally achieve what we want. We don't need a TrackRowWrapper anymore. TrackRow only re-renders when its active state changes, so only two tracks re-render.

Now let's look at some popular state libraries.
-->

---

# Redux Toolkit

<div class="flex gap-4">

```tsx
const playbackSlice = createSlice({
    name: 'playback',
    initialState: initialPlayback,
    reducers: {
        playbackReceived:
            (_, action) => action.payload,
    },
})
const store = configureStore({
    reducer: {
        playback: playbackSlice.reducer,
    },
})
function PlaybackStateProvider({ children }) {
    return (
        <Provider store={store}>
            {children}
        </Provider>
    )
}
```
```tsx
function TrackRow({ track }) {
  const isActive = useSelector(
    (state) =>
      state.playback.activeTrackId === track.id
  )

  return (
    <View style={[styles.row, isActive && styles.activeRow]}>
      <Text>{track.title}</Text>
    </View>
  )
}
```

</div>

<style>
.slidev-code-wrapper {
    width: 480px;
}
</style>

<!--
The redux version gets a bit long but its useSelector makes only the affected rows re-render.
-->

---

# Zustand

```tsx
const usePlaybackStore = create((set) => ({
    playback: initialPlayback,
    setPlayback: (playback) => {
        set({ playback })
    },
}))

function TrackRow({ track }) {
    const isActive = usePlaybackStore(
        (state) => state.playback.activeTrackId === track.id,
    )

    return (
        <View style={[styles.row, isActive && styles.activeRow]}>
            <Text>{track.title}</Text>
        </View>
    )
}
```

<!--
Zustand gets shorter and also re-renders only the affected rows.
-->

---

# Jotai

```tsx
const playbackAtom = atom(initialPlayback)

const isActiveAtom = atomFamily((trackId) =>
    atom(
        (get) => get(playbackAtom).activeTrackId === trackId,
    ),
)

function TrackRow({ track }) {
    const isActive = useAtomValue(isActiveAtom(track.id))

    return (
        <View style={[styles.row, isActive && styles.activeRow]}>
            <Text>{track.title}</Text>
        </View>
    )
}
```

<!--
With Jotai we can stack atoms and atom families to make a selector to re-render only the affected rows
-->

---

# MobX

```tsx
const playback = observable({ ...initialPlayback })

const TrackRow = observer(function TrackRow({ track }) {
    const isActive = computed(
        () => playback.activeTrackId === track.id
    ).get()

    return (
        <View style={[styles.row, isActive && styles.activeRow]}>
            <Text>{track.title}</Text>
        </View>
    )
})
```

<!--
With MobX we can use a computed to make an isActive selector. Unfortunately the observer wrapper makes it incompatible with React Compiler though.
-->

---

# Legend State

```tsx
const playback$ = observable({ ...initialPlayback })

function TrackRow({ track }) {
    const isActive = useValue(
        () => playback$.activeTrackId.get() === track.id,
    )

    return (
        <View style={[styles.row, isActive && styles.activeRow]}>
            <Text>{track.title}</Text>
        </View>
    )
}
```

<!--
And with Legend State we can just pass a selector function to useValue. This is what I mean by "fast by default".

The efficient way of doing it is the most obvious and easiest way.
-->

---

# Legend State Fine Grained

```tsx
function TrackRow({ track }) {
    return (
        <$View
            $style={() =>
                playback$.activeTrackId.get() === track.id ?
                    styles.activeRow :
                    styles.row
            }
        />
    )
}
```

<!--
Legend State lets us go even more fine grained though, in a way that isn't possible in other libraries.

Using a reactive style prop, the TrackRow never re-renders. Only a tiny wrapper around the View updates a style when the activeTrackId changes.

It's possible to do this with regular React state hooks or other state libraries. But it's complex to create special child style wrapper components.

Legend State is all about making it fast by default, and very easy to make it even faster.
-->

---

# Cause => Render => Effect

<div class="flex items-center gap-4 text-2xl">
    <div class="box-not-flashing">state change</div>
    =>
    <div class="box-not-flashing">render</div>
    =>
    <div class="box-not-flashing">effects</div>
</div>

<div class="flex items-center gap-4 opacity-0 text-2xl">
    <div class="box-not-flashing">state change</div>
    =>
    <div class="box-not-flashing">effects</div>
</div>

<!--
It's also easier to reason about.

Normally in React the cause effect relationship is not clear. You change some state and then some time later the render happens and some time later the render commits and runs the effects.

Coordinating everything through render is really confusing.
-->

---

# Cause => Effect

<div class="flex items-center gap-4 text-2xl">
    <div class="box-not-flashing">state change</div>
    =>
    <div class="box-not-flashing">render</div>
</div>

<div class="flex items-center gap-4 text-2xl">
    <div class="box-not-flashing">state change</div>
    =>
    <div class="box-not-flashing">effects</div>
</div>

<!--
So instead we just run the effects when state changes. Don't need to go through the render.
-->

---

<div class="flex flex-col gap-6 pt-8">
    <div class="text-4xl">
        UI &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = &nbsp;fn(props, state)
    </div>
    <div class="text-4xl">
        effects &nbsp;= &nbsp;fn(props, state)
    </div>
    <div class="text-4xl">
        hooks &nbsp;&nbsp;= &nbsp;fn(props, state)
    </div>
</div>

<!--
Everything is just a function of state. And it feels a lot easier to understand.
-->

---

# Remove hooks

<div class="mx-auto grid w-max grid-cols-[250px_max-content] gap-1 text-lg pt-8">
  <div class="box-not-flashing-small">❌ &nbsp;&nbsp;useState</div>
  <div class="box-not-flashing-small">✅ &nbsp;&nbsp;useObservable</div>
  <div class="box-not-flashing-small">❌ &nbsp;&nbsp;useEffect</div>
  <div class="box-not-flashing-small">✅ &nbsp;&nbsp;useObserveEffect</div>
  <div class="box-not-flashing-small">❌ &nbsp;&nbsp;deps arrays</div>
  <div class="box-not-flashing-small">✅ &nbsp;&nbsp;[]</div>
</div>

<!--
We can remove the confusing hooks. We've replaced useState with useObservable. so now we don't need useEffect, useObserveEffect can observe them without a render. And most importantly, we remove the deps arrays so we don't make mistakes and so that callbacks are stable.
-->

---

# onChange

```tsx
const user$ = observable({ name: 'Annyong' });

user$.onChange(({ value, changes }) => {
    changes.forEach((change) => {
        const { path, valueAtPath, prevAtPath } = change;
        // ...
    });
});
```

<!--
But that was only the second of the three goals of Legend State. The third comes from an interesting property of having deeply nested changes.

onChange gets more information than just that it changed. We know the path of the child within the hierarchy and the details of that change.

So it can notify only the nodes that actually changed with the exact details of the change. And we can use that to setup automatic persistence.
-->

---

# Persistence

```tsx
const store$ = observable({
  settings: {
    theme: 'light',
    fontSize: 14
  },
  messages: [],
  users: new Map<string, User>()
})

syncObservable(store$, {
    persist: {
        name: "store",
        plugin: ObservablePersistMMKV
    },
})
```

<!--
First we can track changes in the observable and save to persistence whenever anything changes. That's easy.

But what's really cool is that it knows exactly what child changed and how. So we can build a full sync engine.
-->

---

# Sync engine

````md magic-move
```tsx
const profile$ = observable(syncedCrud({
    get: getProfile,
    create: createProfile,
    update: updateProfile,
    delete: deleteProfile,
}))

profile$.get() // Triggers sync

profile$.name.set('Annyong') // Saves to server
```
```tsx
const profile$ = observable(syncedCrud({
    get: getProfile,
    create: createProfile,
    update: updateProfile,
    delete: deleteProfile,
    persist: {
        plugin: ObservablePersistMMKV, // Set the persistence plugin
        name: 'profile', // Set the name of this object in persistence
        retrySync: true, // Persist pending changes to retry
    },
    retry: {
        infinite: true, // Keep retrying until it saves
    },
    changesSince: 'last-sync', // Sync only diffs
    fieldUpdatedAt: 'updatedAt' // Required for syncing only diffs
}))
```
````

<style>
.slidev-code-wrapper {
    width: 720px !important;
}
</style>

<!--
We define in the state how it should sync itself.

We don't have to set up any queries or mutations, or optimistic updates. We just get the value and it downloads, and set it and it saves itself.

2. And even cooler, it can cache saved changes offline and it will sync them whenever it can. It can do partial syncs if you have an updated timestamp to save on bandwidth.
-->

---

# Just set state

```tsx
profile$.name.set('Annyong')
```

<div class="flex flex-col gap-6 pt-8 justify-self-center">
<div class="flex items-center gap-4 text-2xl">
    state => render
</div>

<div class="flex items-center gap-4 text-2xl">
    state => effects
</div>

<div class="flex items-center gap-4 text-2xl">
    state => sync
</div>
</div>

<style>
.slidev-code-wrapper {
    width: 280px !important;
}
</style>

<!--
So all you have to do is set state. That triggers rendering, side effects, and sync. It doesn't have to run through render to do any of that.

Skipping render makes it fast. It skips a ton of work and does only what's needed.
-->

---

# Fast by Default

<!--
And that's why Legend State is Fast by Default.
-->

---

# www.legendstate.com


<!--
Check it out at Legend State .com

I had planned to release 3.0 right now, but my plane from London didn't have wifi. Use 3.0 beta for all the latest features and 3.0 should come next week.

Thanks!
-->
