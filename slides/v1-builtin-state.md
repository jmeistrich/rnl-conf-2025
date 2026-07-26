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

# Fast by Default:

# Performance Starts with State ⚡

</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div>@jmeistrich</div>
  <div class="text-gray-400">Chain React 2026</div>
</div>

<!--
Hi everyone! I'm Jay.

I build Legend State and Legend List, and I spend a slightly unhealthy amount of time thinking about performance.

Today I want to walk through one completely normal state decision in a music app. We're going to follow the recommended React patterns, make every reasonable optimization, and see where we end up.
-->

---

# I make lists fast

<div class="media-placeholder aspect-video mt-8">
  <div class="placeholder-kicker">VIDEO PLACEHOLDER</div>
  <div class="placeholder-title">LegendList scrolling smoothly in the music app</div>
  <div class="placeholder-detail">Show a long, image-heavy track list on a slower physical device.</div>
</div>

<!--
Most of you probably know me from LegendList.

I've spent years trying to make lists render as little as possible. It virtualizes the data, recycles rows, and does a lot of weird things so React and native don't have to.

And the list in this app is very fast.
-->

---

# Then I made it slow

<!--
And then I made it slow with three values.

Not ten thousand tracks. Not giant images. Not a bad native module.

Three values.
-->

---

<div class="media-placeholder aspect-video">
  <div class="placeholder-kicker">VIDEO PLACEHOLDER</div>
  <div class="placeholder-title">Music app playing a track</div>
  <div class="placeholder-detail">Show the playback area updating and a circular progress indicator on the active row.</div>
</div>

<!--
This is a music app I'm building with React Native.

The player at the bottom shows the current track, progress, and duration. The active row also has a circular progress indicator on the right.

It looks like a tiny feature. One circle is moving.
-->

---

# One circle is moving

<div class="mt-12 flex items-center justify-center gap-14">
  <div class="mock-track-list">
    <div class="mock-track-row"><span>Track One</span><span>○</span></div>
    <div class="mock-track-row mock-track-active"><span>Track Two</span><span class="mock-spinner">◔</span></div>
    <div class="mock-track-row"><span>Track Three</span><span>○</span></div>
    <div class="mock-track-row"><span>Track Four</span><span>○</span></div>
  </div>
  <div class="text-6xl">🤏</div>
</div>

<!--
Only this one little circle needs to update.

So let's build it the normal way.
-->

---

# One cohesive value

```ts
type PlaybackState = {
  activeTrackId?: string
  progress: number
  duration: number
}
```

<!--
The native player reports one playback snapshot: activeTrackId, progress, and duration.

That is good domain modeling. These values describe one moment in one playback session. They come from one native system, together, and they should stay consistent with each other.

So this is one cohesive value.
-->

---

# Start state where it's used

```tsx {1-8|4-6|10-14}
function PlaybackArea() {
  const [playback, setPlayback] = useState(initialPlayback)

  useEffect(() => {
    return NativePlayer.subscribe(setPlayback)
  }, [])

  return (
    <PlaybackControls
      progress={playback.progress}
      duration={playback.duration}
    />
  )
}
```

<!--
We start state close to where it's used. This is exactly what React teaches us.

The effect is also completely legitimate. A native player is an external system, so the playback area subscribes and copies each snapshot into React state.

And this is fast. Progress changes, PlaybackArea renders, the slider and text update. Perfect.
-->

---

<div class="render-tree">
  <div class="render-node quiet">MusicScreen</div>
  <div class="render-children">
    <div class="render-node quiet">LegendList</div>
    <div class="render-node expected">PlaybackArea</div>
  </div>
</div>

<div class="mt-10 flex justify-center gap-8 text-base">
  <div><span class="legend-dot expected-dot"></span> Needed render</div>
  <div><span class="legend-dot quiet-dot"></span> No render</div>
</div>

<!--
When progress changes, the playback area renders. The screen and the list do not.

Green means something visibly changed. No border means React left it alone.

This is the ideal update.
-->

---

# The list needs `activeTrackId`

<div class="mt-12 text-left mx-auto w-[660px]">
  <div class="render-node quiet">MusicScreen</div>
  <div class="render-children">
    <div class="render-node wanted">TrackList <span class="text-gray-400">needs activeTrackId</span></div>
    <div class="render-node wanted">PlaybackArea <span class="text-gray-400">needs everything</span></div>
  </div>
</div>

<!--
But now the list needs activeTrackId so it can show that progress indicator on the active row.

The state is needed by two sibling components.

What are we supposed to do?
-->

---

# Lift state up

<!--
Lift state up!

This is one of the most common patterns in React. The docs say to move shared state to the closest common ancestor.

So that's what we'll do.
-->

---

<div class="meme-placeholder">
  <div class="placeholder-kicker">MEME PLACEHOLDER</div>
  <div class="placeholder-title">Rafiki holding Simba over Pride Rock</div>
  <div class="placeholder-detail">Label Simba “playbackState” and Rafiki “React docs”. Caption: “When one more component needs the value.”</div>
</div>

<!--
We take our tiny little state and hold it up as high as possible for the entire component tree to see.

What could go wrong?
-->

---

````md magic-move
```tsx
function PlaybackArea() {
  const [playback, setPlayback] = useState(initialPlayback)

  useEffect(() => NativePlayer.subscribe(setPlayback), [])

  return <PlaybackControls playback={playback} />
}
```
```tsx
function MusicScreen() {
  const [playback, setPlayback] = useState(initialPlayback)

  useEffect(() => NativePlayer.subscribe(setPlayback), [])

  return (
    <>
      <TrackList playback={playback} />
      <PlaybackArea playback={playback} />
    </>
  )
}
```
````

<!--
We move both the state and its native subscription into MusicScreen.

Now both children can access it. The ownership problem is solved.

But every progress callback now calls setState at the screen.
-->

---

<div class="render-tree wide-tree">
  <div class="render-node waste">MusicScreen</div>
  <div class="render-children">
    <div>
      <div class="render-node waste">LegendList</div>
      <div class="render-row-grid">
        <div class="render-node waste small">TrackRow</div>
        <div class="render-node waste small">TrackRow</div>
        <div class="render-node expected small">Active TrackRow</div>
        <div class="render-node waste small">TrackRow</div>
      </div>
    </div>
    <div class="render-node expected">PlaybackArea</div>
  </div>
</div>

<div class="mt-8 flex justify-center gap-8 text-base">
  <div><span class="legend-dot expected-dot"></span> Visible change</div>
  <div><span class="legend-dot waste-dot"></span> Same output</div>
</div>

<!--
Progress changes four times a second.

MusicScreen renders. The list receives a new playback object. The mounted rows are reconsidered. The playback area renders.

Only the active row indicator and playback controls changed. Everything red did work to produce the same output it already had.

Virtualization limited this to the mounted rows, but it cannot make the update itself precise.
-->

---

<div class="media-placeholder aspect-video">
  <div class="placeholder-kicker">VIDEO PLACEHOLDER</div>
  <div class="placeholder-title">Render-flashing music app before optimization</div>
  <div class="placeholder-detail">Alternate red borders or counters on MusicScreen, LegendList, and every mounted TrackRow while progress advances.</div>
</div>

<!--
This is where we show it in the real app.

The circle is moving, but the whole screen is having a tiny panic attack four times a second.

Use a release or profiling build and keep the render visualization cheap. The point is to make the propagation visible, then back it up with measurements later.
-->

---

# Moving state up

# moves the render boundary up

<!--
Moving state up also moved the render boundary up.

This is the first key idea: where a useState lives is where its updates begin.

Sharing the value and subscribing to the value became the same architectural decision.
-->

---

# Let's fix it

<!--
But this is React. We know how to optimize React.

Let's fix it.
-->

---

````md magic-move
```tsx
<TrackList playback={playback} />
```
```tsx
<TrackList
  activeTrackId={playback.activeTrackId}
  progress={playback.progress}
  duration={playback.duration}
/>
```
```tsx
const TrackList = memo(function TrackList({
  activeTrackId,
  progress,
  duration,
}) {
  // ...
})
```
````

<!--
First, never pass a big object when we can pass narrow primitives.

Then memoize TrackList so unrelated changes can stop at that boundary.

This is a real improvement. If a child receives only activeTrackId, progress no longer needs to reach it.

But our active row also needs progress and duration for the indicator, so that volatile state still has to enter the list somewhere.
-->

---

# And then...

<div class="mx-auto mt-10 grid w-max grid-cols-2 gap-4 text-xl text-left">
  <div class="box-not-flashing-small">React.memo</div>
  <div class="box-not-flashing-small">Primitive props</div>
  <div class="box-not-flashing-small">Stable renderItem</div>
  <div class="box-not-flashing-small">Stable callbacks</div>
  <div class="box-not-flashing-small">Stable track objects</div>
  <div class="box-not-flashing-small">Careful extraData</div>
</div>

<!--
Then we memoize the row. Stabilize renderItem. Stabilize callbacks. Preserve track object identity. Decide which values belong in extraData.

Every one of these is a valid optimization.

And every future change has to preserve all of them.
-->

---

<div class="meme-placeholder">
  <div class="placeholder-kicker">MEME PLACEHOLDER</div>
  <div class="placeholder-title">Charlie Day conspiracy board</div>
  <div class="placeholder-detail">Cover the board with “memo”, “useCallback”, “extraData”, “stable object”, and arrows pointing to TrackRow.</div>
</div>

<!--
At this point the performance architecture is mostly photos of props connected with red string.

And we still have to get playback state into parts of the tree that need it.
-->

---

# Context!

```tsx
<PlaybackContext value={playback}>
  <TrackList />
  <PlaybackArea />
</PlaybackContext>
```

<!--
So let's stop prop drilling and use Context.

The state stays cohesive, the provider owns it, and any component can access it.

This code is much cleaner.
-->

---

```tsx {2|3-4|all}
function TrackRow({ track }) {
  const playback = use(PlaybackContext)
  const isActive = playback.activeTrackId === track.id

  return (
    <TrackContent
      track={track}
      isActive={isActive}
      progress={playback.progress}
      duration={playback.duration}
    />
  )
}
```

<!--
The row reads the playback Context and uses the fields it needs.

But reading one field is not subscribing to one field. The row subscribed to the Context value.

Every progress update gives the provider a new object, so every mounted row that read this Context renders. React.memo does not stop Context notifications.
-->

---

# Context solves access to state

# not subscription granularity

<!--
Context solved access to the state.

It did not solve subscription granularity.

But we can optimize Context too.
-->

---

# Split the Context

```tsx
const ActiveTrackContext = createContext<string | undefined>(undefined)
const ProgressContext = createContext(0)
const DurationContext = createContext(0)
```

<!--
We split our one playback value into three Contexts.

Now a progress change does not have to notify a consumer that only reads duration or activeTrackId.

But Context subscription granularity stops at the component boundary, so splitting the state is only half of the optimization.
-->

---

# Split the UI too

<div class="subscription-layout mt-10">
  <div class="subscription-card">
    <div class="text-gray-400">ActiveTrackDetails</div>
    <div class="subscription-pill active">ActiveTrackContext</div>
  </div>
  <div class="subscription-card">
    <div class="text-gray-400">PlaybackSlider</div>
    <div class="subscription-pill progress">ProgressContext</div>
    <div class="subscription-pill duration">DurationContext</div>
  </div>
  <div class="subscription-card">
    <div class="text-gray-400">CurrentTime</div>
    <div class="subscription-pill progress">ProgressContext</div>
  </div>
  <div class="subscription-card">
    <div class="text-gray-400">TotalDuration</div>
    <div class="subscription-pill duration">DurationContext</div>
  </div>
</div>

<!--
If PlaybackArea reads all three Contexts, the entire playback area still renders whenever any one changes.

So we split the UI into subscription-specific leaves too.

Track details read activeTrackId. The slider reads progress and duration. CurrentTime reads progress. TotalDuration reads duration.

Now the duration text does not render four times a second just because progress changed.
-->

---

# The provider moved too

```tsx
function PlaybackProvider({ children }) {
  const [playback, setPlayback] = useState(initialPlayback)

  useEffect(() => NativePlayer.subscribe(setPlayback), [])

  return (
    <ActiveTrackContext value={playback.activeTrackId}>
      <ProgressContext value={playback.progress}>
        <DurationContext value={playback.duration}>
          {children}
        </DurationContext>
      </ProgressContext>
    </ActiveTrackContext>
  )
}
```

<!--
And the state cannot remain in MusicScreen, because MusicScreen would still render on every native callback.

So we move it again into a dedicated provider whose children can remain stable while Context updates its specific consumers.

Our state has now moved twice, and the one provider has become three.
-->

---

# Conditional `use`

```tsx {2-3|5-11|all}
function TrackRow({ track }) {
  const activeTrackId = use(ActiveTrackContext)
  const isActive = activeTrackId === track.id

  let progress = 0
  let duration = 0

  if (isActive) {
    progress = use(ProgressContext)
    duration = use(DurationContext)
  }

  return (
    <TrackContent
      {...{ track, isActive, progress, duration }}
    />
  )
}
```

<!--
Modern React's use API can read Context conditionally.

So inactive rows can read activeTrackId without subscribing to progress and duration. Only the active row receives the position updates.

This is valid. It works. React can absolutely be made fast.
-->

---

# It works! 🎉

<div class="render-tree wide-tree mt-10">
  <div class="render-node quiet">MusicScreen</div>
  <div class="render-children">
    <div>
      <div class="render-node quiet">LegendList</div>
      <div class="render-row-grid">
        <div class="render-node quiet small">TrackRow</div>
        <div class="render-node quiet small">TrackRow</div>
        <div class="render-node expected small">Active TrackRow</div>
        <div class="render-node quiet small">TrackRow</div>
      </div>
    </div>
    <div class="render-node expected">CurrentTime</div>
  </div>
</div>

<!--
And now progress updates only the active row and the current-time parts of the playback UI.

We did it.

So what am I complaining about?
-->

---

# Look what we had to do

<div class="mx-auto mt-8 grid grid-cols-2 gap-x-12 gap-y-2 text-left text-[21px] w-[820px]">
  <div>Lift state and its native effect</div>
  <div>Design narrow primitive props</div>
  <div>Add memo boundaries</div>
  <div>Stabilize callbacks and objects</div>
  <div>Move state into a provider</div>
  <div>Split one value into 3 Contexts</div>
  <div>Split the UI into consumers</div>
  <div>Conditionally consume Context</div>
</div>

<!--
We started with three values and one component.

To make it performant, we lifted the state and effect, designed narrow props, memoized boundaries, stabilized identities, moved ownership again, split one domain value into three Contexts, split the UI into subscription components, and conditionally consumed Context in the row.

And all of this needs to stay correct as the app grows.
-->

---

<div class="meme-placeholder">
  <div class="placeholder-kicker">MEME PLACEHOLDER</div>
  <div class="placeholder-title">“This is fine” dog surrounded by Context providers</div>
  <div class="placeholder-detail">Replace the flames with nested ActiveTrackContext, ProgressContext, DurationContext, memo, and extraData boxes.</div>
</div>

<!--
This is fine.

It's technically very optimized.
-->

---

# We started with one cohesive value

```ts
{
  activeTrackId,
  progress,
  duration,
}
```

<!--
Remember where we started.

This is one cohesive value because it represents one playback session and arrives as one native snapshot.
-->

---

# We reorganized the app around updates

<div class="architecture-compare mt-10">
  <div class="architecture-column">
    <div class="architecture-label">Domain</div>
    <div class="cohesive-object">
      <div>activeTrackId</div>
      <div>progress</div>
      <div>duration</div>
    </div>
  </div>
  <div class="text-5xl text-gray-500">→</div>
  <div class="architecture-column">
    <div class="architecture-label">React optimization</div>
    <div class="split-object active">ActiveTrackContext</div>
    <div class="split-object progress">ProgressContext</div>
    <div class="split-object duration">DurationContext</div>
    <div class="text-sm text-gray-400 mt-3">+ matching component boundaries</div>
  </div>
</div>

<!--
The domain says these values belong together.

But the rendering model pushed us to reorganize them by update frequency, and then reorganize the component tree to match.

We started by organizing components around the UI. To make Context performant, we reorganized them around notifications.
-->

---

# React made us choose

<div class="mt-12 grid grid-cols-2 gap-10 w-[840px] mx-auto">
  <div class="choice-card">
    <div class="text-2xl font-bold">Cohesive state</div>
    <div class="text-red-300 mt-5">Broad subscriptions</div>
  </div>
  <div class="choice-card">
    <div class="text-2xl font-bold">Precise subscriptions</div>
    <div class="text-yellow-200 mt-5">Fragmented architecture</div>
  </div>
</div>

<!--
With built-in shared state, we ended up choosing between keeping the state cohesive and making the subscriptions precise.

That is the core problem.

Because the row never actually wanted these three Contexts.
-->

---

# What does the row actually need?

```tsx
if (activeTrackId !== track.id) {
  return null
}

return {
  progress,
  duration,
}
```

<!--
The row wants one derived answer.

Am I the active track? If not, I don't care about progress or duration at all. If I am active, give me the position.

We need to subscribe to the answer, not merely to the source fields.
-->

---

# We need an external store

<div class="mt-12 text-3xl leading-relaxed">
  Shared ownership
  <span class="text-gray-500 mx-3">+</span>
  local subscriptions
  <span class="text-gray-500 mx-3">+</span>
  selectors
</div>

<!--
This requires a different state model.

The state needs shared ownership outside the component tree, while every component subscribes locally to its own leaf or derived result.

React gives libraries useSyncExternalStore to integrate this kind of state safely.
-->

---

# It's only 15 lines!

```tsx
let playback = initialPlayback
const listeners = new Set<() => void>()

function setPlayback(next) {
  playback = next
  listeners.forEach(listener => listener())
}

function subscribe(listener) {
  listeners.add(listener)
  return () => listeners.delete(listener)
}

function usePlayback(selector) {
  return useSyncExternalStore(
    subscribe,
    () => selector(playback),
  )
}
```

<!--
And an external store is easy! It's only about fifteen lines.

We have a value, a set of listeners, and a hook that selects a snapshot.

This solves today's demo.
-->

---

# And then...

<div class="mx-auto mt-6 grid grid-cols-3 gap-3 text-base w-[900px]">
  <div class="box-not-flashing-small">Selector equality</div>
  <div class="box-not-flashing-small">Cached snapshots</div>
  <div class="box-not-flashing-small">Dynamic dependencies</div>
  <div class="box-not-flashing-small">Batching</div>
  <div class="box-not-flashing-small">Atomic updates</div>
  <div class="box-not-flashing-small">Derived state</div>
  <div class="box-not-flashing-small">Concurrent rendering</div>
  <div class="box-not-flashing-small">Subscription cleanup</div>
  <div class="box-not-flashing-small">Native synchronization</div>
  <div class="box-not-flashing-small">Persistence</div>
  <div class="box-not-flashing-small">Remote sync</div>
  <div class="box-not-flashing-small">Debugging</div>
</div>

<!--
Then the real app arrives.

We need selector equality, stable snapshots, dynamic dependencies, batching, atomic updates, derived state, concurrent rendering correctness, cleanup, native synchronization, persistence, remote sync, and debugging.

Creating an external store is easy. Making it the correct foundation for an application is not.
-->

---

<div class="meme-placeholder">
  <div class="placeholder-kicker">MEME PLACEHOLDER</div>
  <div class="placeholder-title">Gru presentation plan meme</div>
  <div class="placeholder-detail">Panels: “Optimize one circle” → “Build useSyncExternalStore wrapper” → “Maintain a state library forever” → Gru looking back at the last panel.</div>
</div>

<!--
Congratulations. We fixed one circle and started maintaining a state library.
-->

---

# Or...

<!--
Or we could use one.
-->

---

# Legend State

<div class="text-3xl mt-8 text-gray-300">Fine-grained state for React</div>

<!--
This is why I built Legend State.

Not because I wanted another syntax for setting a value. I wanted state ownership and state subscription to be separate decisions.

And I wanted the fast architecture to be the default architecture.
-->

---

# Keep the cohesive value

```tsx
const playback$ = observable({
  activeTrackId: undefined as string | undefined,
  progress: 0,
  duration: 0,
})

NativePlayer.subscribe((state) => playback$.set(state))
```

<!--
We go back to the original domain model.

One native player. One playback snapshot. One cohesive observable.

The native callback updates external state instead of forcing every event through parent React state.
-->

---

# Subscribe to any leaf

<div class="flex code-cols gap-5 mt-8">

```tsx
function CurrentTime() {
  const progress = useValue(
    playback$.progress,
  )

  return <Text>{formatTime(progress)}</Text>
}
```

```tsx
function TotalDuration() {
  const duration = useValue(
    playback$.duration,
  )

  return <Text>{formatTime(duration)}</Text>
}
```

</div>

<!--
CurrentTime subscribes to progress. TotalDuration subscribes to duration.

They can live wherever the UI structure makes sense. We do not have to build a Context and component topology just to create those subscription boundaries.
-->

---

# Subscribe to the answer

```tsx
function TrackRow({ track }) {
  const activePlayback = useValue(() => {
    if (playback$.activeTrackId.get() !== track.id) {
      return null
    }

    return {
      progress: playback$.progress.get(),
      duration: playback$.duration.get(),
    }
  })

  return (
    <TrackContent
      track={track}
      activePlayback={activePlayback}
    />
  )
}
```

<!--
And the row subscribes to the answer it actually needs.

Every row tracks whether it is active. Only the active row reads progress and duration, so only that row subscribes to position updates.

When progress changes, the active row updates. When activeTrackId changes, the old and new derived answers change. Every other row still resolves to null, so React does not render them.
-->

---

<div class="render-tree wide-tree">
  <div class="render-node quiet">MusicScreen</div>
  <div class="render-children">
    <div>
      <div class="render-node quiet">LegendList</div>
      <div class="render-row-grid">
        <div class="render-node quiet small">TrackRow</div>
        <div class="render-node quiet small">TrackRow</div>
        <div class="render-node expected small">Active TrackRow</div>
        <div class="render-node quiet small">TrackRow</div>
      </div>
    </div>
    <div class="render-node expected">CurrentTime</div>
  </div>
</div>

<div class="mt-8 text-2xl">Progress changed</div>

<!--
Now progress changes exactly the pixels that depend on progress.

The screen does not render. LegendList does not render. Unrelated rows do not render. Duration does not render.

The fast result follows directly from what each component reads.
-->

---

<div class="media-placeholder aspect-video">
  <div class="placeholder-kicker">VIDEO PLACEHOLDER</div>
  <div class="placeholder-title">Before and after render-flashing comparison</div>
  <div class="placeholder-detail">Same device, dataset, and playback cadence. Left: lifted state/Context. Right: Legend State leaf subscriptions.</div>
</div>

<!--
Run the exact same interaction again.

On the left, broad state propagation lights up the screen and rows. On the right, only the playback UI and active indicator flash.

Do not change the dataset, update frequency, or device between recordings.
-->

---

# Same app. Same state. Less work.

<div class="media-placeholder metric-placeholder mt-8">
  <div class="placeholder-kicker">MEASUREMENT PLACEHOLDER</div>
  <div class="placeholder-title">Before / after profiler results</div>
  <div class="placeholder-detail">Include renders per progress event, JS or commit time, and CPU as supporting evidence. Use real release-build measurements.</div>
</div>

<!--
Put the measured results here once the real demo is instrumented.

The render count is the clearest causal metric. Commit time and CPU show the user impact.

The numbers matter, but the architecture explains why those numbers changed.
-->

---

# LegendList solves half the problem

<div class="mt-12 grid grid-cols-2 gap-10 w-[850px] mx-auto">
  <div class="choice-card">
    <div class="text-2xl font-bold">Virtualization</div>
    <div class="text-gray-300 mt-5">How many rows exist?</div>
    <div class="text-blue-300 mt-5">LegendList</div>
  </div>
  <div class="choice-card">
    <div class="text-2xl font-bold">Invalidation</div>
    <div class="text-gray-300 mt-5">Which rows update?</div>
    <div class="text-green-300 mt-5">State</div>
  </div>
</div>

<!--
This is why LegendList belongs in this story.

Virtualization controls how many rows exist. State invalidation controls which mounted rows update.

A fast list minimizes mounted work. Fine-grained state minimizes update work inside that mounted window.

You need both.
-->

---

# Keep state cohesive

# Make subscriptions precise

<!--
This is the idea I want you to remember.

Keep state cohesive according to the domain. Make subscriptions precise according to the UI.

Those should not be competing goals.
-->

---

<div class="state-table">
  <div class="state-table-row header">
    <div></div>
    <div>State</div>
    <div>Subscriptions</div>
  </div>
  <div class="state-table-row">
    <div>One Context</div>
    <div class="good">Cohesive</div>
    <div class="bad">Broad</div>
  </div>
  <div class="state-table-row">
    <div>Split Contexts</div>
    <div class="warn">Fragmented</div>
    <div class="warn">More precise</div>
  </div>
  <div class="state-table-row emphasis">
    <div>Fine-grained state</div>
    <div class="good">Cohesive</div>
    <div class="good">Precise</div>
  </div>
</div>

<!--
One Context keeps state cohesive but broadcasts updates.

Split Contexts make subscriptions more precise by fragmenting the state and component architecture.

Fine-grained state gives us both: cohesive state and precise leaf or derived subscriptions.
-->

---

# React isn't slow

# Broad state is expensive

<!--
React is fast at rendering what we ask it to render.

The problem is that broad state subscriptions ask it to render far too much.

React can be manually optimized. Fine-grained state makes that optimized architecture the natural way to write the feature in the first place.
-->

---

<div class="flex flex-col gap-8 text-3xl text-left w-[760px] mx-auto">
  <div><span class="text-blue-300 font-bold">1.</span> State architecture is render architecture</div>
  <div><span class="text-blue-300 font-bold">2.</span> Ownership and subscription are different concerns</div>
  <div><span class="text-blue-300 font-bold">3.</span> Subscribe where the pixels change</div>
</div>

<!--
Three takeaways.

State architecture is render architecture.

Ownership and subscription are different concerns. State can be shared without rendering the owner.

And put the subscription where the pixels change. Subscribe to the smallest leaf or derived answer the UI needs.
-->

---

# Fast by Default

<div class="mt-10 text-3xl text-gray-300">Keep state cohesive. Make subscriptions precise.</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div>@jmeistrich</div>
</div>

<div class="absolute bottom-12 right-16 text-gray-500">legendapp.com</div>

<!--
Fast by default does not mean React is incapable of being fast.

It means you should not have to redesign your state and component tree every time you discover that one value changes more often than another.

Keep state cohesive. Make subscriptions precise.

Thank you!
-->
