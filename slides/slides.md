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

Today I want to show you how one completely normal state decision made a very fast music app slow—and what happened when I built the same feature with six different state architectures.
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

I've spent years trying to make lists render as little as possible. It virtualizes data, recycles rows, and does a lot of weird things so React and native don't have to.

This list is very fast.
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
  <div class="placeholder-detail">Show the playback area and a circular progress indicator on the active row.</div>
</div>

<!--
This is a React Native music app I'm building.

The player shows the current track, progress, and duration. The active row also shows a circular progress indicator.

Only one little circle is moving.
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
Only these pixels need to change.

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
The native player reports one snapshot containing activeTrackId, progress, and duration.

That is good domain modeling. The values describe one moment in one playback session and arrive atomically from one native system.

So this should be one cohesive value.
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
We start state close to where it is used. This is exactly what React teaches us.

The effect legitimately synchronizes with an external native system.

Progress changes, PlaybackArea renders, and nothing else does. Perfect.
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
Green means the component needed to change its visible output.

Progress updates the playback area. The screen and list stay quiet.

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
But now the list needs activeTrackId to show the progress ring on the active row.

The state is needed by two siblings.

What do the React docs tell us to do?
-->

---

# Lift state up

<!--
Lift state up!

Move shared state to the closest common ancestor.

So that's what we'll do.
-->

---

<div class="meme-placeholder">
  <div class="placeholder-kicker">MEME PLACEHOLDER</div>
  <div class="placeholder-title">Rafiki holding Simba over Pride Rock</div>
  <div class="placeholder-detail">Label Simba “playbackState” and Rafiki “React docs”. Caption: “When one more component needs the value.”</div>
</div>

<!--
We take our tiny little state and hold it up as high as possible for the whole tree to see.

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
We move both the state and native subscription into MusicScreen.

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

Only the active indicator and playback controls changed. Everything red did work to produce the same output it already had.
-->

---

<div class="media-placeholder aspect-video">
  <div class="placeholder-kicker">VIDEO PLACEHOLDER</div>
  <div class="placeholder-title">Render-flashing app with lifted state</div>
  <div class="placeholder-detail">Show MusicScreen, LegendList, and mounted TrackRows flashing while playback advances.</div>
</div>

<!--
This is where we show it in the real app.

The circle is moving, but the whole screen is having a tiny panic attack four times a second.
-->

---

# Moving state up

# moves the render boundary up

<!--
Moving state up also moved the render boundary up.

Where a useState lives is where its updates begin.

Sharing a value and subscribing to it became the same architectural decision.
-->

---

# Let's fix it

<div class="mx-auto mt-10 grid w-max grid-cols-2 gap-4 text-xl text-left">
  <div class="box-not-flashing-small">Primitive props</div>
  <div class="box-not-flashing-small">React.memo</div>
  <div class="box-not-flashing-small">Stable renderItem</div>
  <div class="box-not-flashing-small">Stable callbacks</div>
  <div class="box-not-flashing-small">Stable track objects</div>
  <div class="box-not-flashing-small">Careful extraData</div>
</div>

<!--
We know how to optimize React.

Pass narrow primitives. Memoize TrackList and TrackRow. Stabilize renderItem and callbacks. Preserve track identities. Be careful with extraData.

Every one of these is valid. And every future change has to preserve all of them.
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
As playback information spreads through the app, prop threading gets annoying.

So we put the cohesive playback snapshot into Context.

This makes access much cleaner.
-->

---

# Context solves access to state

# not subscription granularity

```tsx
function TrackRow({ track }) {
  const playback = use(PlaybackContext)
  const isActive = playback.activeTrackId === track.id

  // Reading one field still subscribed to the whole Context
}
```

<!--
But reading one field is not subscribing to one field.

Every row reading PlaybackContext receives the new Context object on every progress update. React.memo does not block Context updates.

Context solved access. It did not solve subscription granularity.
-->

---

# Make Context performant

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
To make Context performant, we split the one value into three Contexts.

Then we split PlaybackArea into TrackDetails, Slider, CurrentTime, and TotalDuration so each component reads only the Contexts it needs.

Then we move the state and effect again into a dedicated provider so MusicScreen itself stays stable.
-->

---

# Conditional `use`

```tsx
function TrackRow({ track }) {
  const activeTrackId = use(ActiveTrackContext)
  const isActive = activeTrackId === track.id

  let position = null
  if (isActive) {
    position = {
      progress: use(ProgressContext),
      duration: use(DurationContext),
    }
  }

  return <TrackContent {...{ track, isActive, position }} />
}
```

<!--
Modern React's use API lets us consume Context conditionally.

Inactive rows read activeTrackId but do not subscribe to progress and duration. Only the active row receives position updates.

This is valid. It works. React can absolutely be made fast.
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

To make it performant, we lifted the state and effect, designed narrow props, memoized boundaries, stabilized identities, moved ownership again, split one domain value into three Contexts, split the UI, and conditionally consumed Context.

We can keep doing this. Or we can use a state model designed for subscriptions.
-->

---

# We need an external store

<div class="mt-12 text-3xl leading-relaxed">
  Shared ownership
  <span class="text-gray-500 mx-3">+</span>
  local subscriptions
  <span class="text-gray-500 mx-3">+</span>
  derived values
</div>

<!--
The requirement is simple.

State ownership should live outside the React tree. Components should subscribe locally to the smallest leaf or derived answer they display.

So we need an external state library.

But that raises the question everyone already using one is thinking.
-->

---

# So any state library?

<!--
Does any state library solve this equally well?

I could have shown a feature matrix and put green checks in the Legend State column.

That seemed slightly suspicious.
-->

---

<div class="meme-placeholder">
  <div class="placeholder-kicker">MEME PLACEHOLDER</div>
  <div class="placeholder-title">Obama awarding himself a medal</div>
  <div class="placeholder-detail">Caption: “Library author benchmarks state libraries. His library wins.”</div>
</div>

<!--
Apparently the state library I wrote is the best state library.

Source: me.

So instead of asking you to trust that, I built the same app six times.
-->

---

# I built it six times

<div class="implementation-grid mt-10">
  <div class="implementation-card react">React</div>
  <div class="implementation-card redux">Redux</div>
  <div class="implementation-card zustand">Zustand</div>
  <div class="implementation-card jotai">Jotai</div>
  <div class="implementation-card mobx">MobX</div>
  <div class="implementation-card legend">Legend State</div>
</div>

<!--
React with no library. Redux Toolkit. Zustand. Jotai. MobX. And Legend State.

They share the presentational components, LegendList setup, track data, native-player simulator, device, and update sequence.

Only the state architecture changes.
-->

---

# Same app. Same events. Same device.

<div class="benchmark-rules mt-10">
  <div>Release build</div>
  <div>Current stable versions</div>
  <div>Natural implementation</div>
  <div>Fully optimized implementation</div>
  <div>Public source</div>
  <div>Community review</div>
</div>

<!--
The benchmark has to be fair.

Same release build, same physical device, and current stable library versions.

Each library gets two implementations: the natural version someone would write after reading its docs, and the fully optimized version after profiling.

The source and raw results will be public. If I implemented your favorite library incorrectly, please fix it.
-->

---

# Three identical updates

<div class="update-sequence mt-10">
  <div class="update-card">
    <div class="update-number">1</div>
    <div class="font-bold">Progress</div>
    <div class="text-gray-400">Only active playback UI</div>
  </div>
  <div class="update-card">
    <div class="update-number">2</div>
    <div class="font-bold">Change track</div>
    <div class="text-gray-400">Old row + new row</div>
  </div>
  <div class="update-card">
    <div class="update-number">3</div>
    <div class="font-bold">Replace snapshot</div>
    <div class="text-gray-400">One atomic update</div>
  </div>
</div>

<!--
Every implementation receives the same deterministic native events.

First, progress changes. Only the playback controls and active indicator should update.

Second, the active track changes. Only the old and new rows should change.

Third, the complete native snapshot is replaced, to test batching and atomic updates.
-->

---

# State libraries solve different layers

<div class="state-architecture-groups mt-10">
  <div class="architecture-group">
    <div class="architecture-group-title">Selector stores</div>
    <div class="architecture-libraries">Redux · Zustand</div>
    <div class="architecture-caption">Compare selected output</div>
  </div>
  <div class="architecture-group">
    <div class="architecture-group-title">Atom stores</div>
    <div class="architecture-libraries">Jotai</div>
    <div class="architecture-caption">Compose dependency atoms</div>
  </div>
  <div class="architecture-group">
    <div class="architecture-group-title">Tracked observables</div>
    <div class="architecture-libraries">MobX · Legend State</div>
    <div class="architecture-caption">Track values actually read</div>
  </div>
</div>

<!--
We do not need six separate code tours. They fall into three broad architectures.

Redux and Zustand are selector stores. Jotai models state as primitive and derived atoms. MobX and Legend State automatically track observable reads.

They can all reduce React renders. But they do not necessarily avoid the same amount of work or require the same architecture.
-->

---

# Selector stores

<div class="flex code-cols gap-5 mt-8">

```tsx
const activeProgress = useAppSelector((state) =>
  state.playback.activeTrackId === track.id
    ? state.playback.progress
    : null
)
```

```tsx
const activeProgress = usePlaybackStore((state) =>
  state.activeTrackId === track.id
    ? state.progress
    : null
)
```

</div>

<div class="mt-5 grid grid-cols-2 gap-5 text-gray-400">
  <div>Redux: <code>useAppSelector</code></div>
  <div>Zustand: bound store hook</div>
</div>

<div class="work-path mt-6">
  <div class="work-node">Store update</div>
  <div class="work-arrow">→</div>
  <div class="work-node many">Selectors checked</div>
  <div class="work-arrow">→</div>
  <div class="work-node one">Changed components render</div>
</div>

<!--
Selector stores are a huge improvement over Context.

These are the normal APIs users of each library expect: a typed React-Redux selector hook and a bound Zustand store hook.

Every row selects a primitive derived output, and React renders only when that output changes.

The question for the benchmark is what happens before React: how many store subscribers wake up, how many selectors execute, and how much equality work happens for every native update?
-->

---

# Atom stores

```tsx
const activeTrackIdAtom = atom<string | undefined>(undefined)
const progressAtom = atom(0)

const activeProgressAtom = atomFamily((trackId: string) =>
  atom((get) =>
    get(activeTrackIdAtom) === trackId
      ? get(progressAtom)
      : null
  )
)

const activeProgress = useAtomValue(
  activeProgressAtom(track.id)
)
```

<!--
Atom stores can make the dependency graph precise.

We create primitive atoms, then a family of derived atoms for the answers components need. The current Jotai guidance imports atomFamily from the jotai-family package.

The row reads its derived atom with useAtomValue, which is the normal read-only React API.

The performance can be excellent. The design question is how much reactive structure we create around the original domain value to get there.
-->

---

# Automatically tracked state

<div class="flex code-cols gap-5 mt-8">

```tsx
const TrackRow = observer(({ track }) => {
  const activeProgress =
    playback.activeTrackId === track.id
    ? playback.progress
    : null

  return <Progress value={activeProgress} />
})
```

```tsx
function TrackRow({ track }) {
  const activeProgress = useValue(() =>
    playback$.activeTrackId.get() === track.id
      ? playback$.progress.get()
      : null
  )

  return <Progress value={activeProgress} />
}
```

</div>

<div class="mt-6 grid grid-cols-2 gap-5 text-gray-400">
  <div>MobX: component observation</div>
  <div>Legend: value observation</div>
</div>

<!--
MobX is the closest comparison. It already proves automatic fine-grained tracking is an excellent architecture.

MobX typically observes a component and tracks observable properties read during render.

Legend State makes the tracking boundary an explicit value. It tracks the selector's derived result and its dynamic dependencies without requiring the whole component to become an observer.

Now let's see what those differences actually cost.
-->

---

<div class="media-placeholder aspect-video">
  <div class="placeholder-kicker">VIDEO PLACEHOLDER</div>
  <div class="placeholder-title">Six-way render-flashing comparison</div>
  <div class="placeholder-detail">A 2×3 grid of identical apps receiving the same progress and track-change events.</div>
</div>

<!--
This should be the big visual payoff.

All six apps receive the same update in lockstep. Render borders and counters show which components actually render.

The recording should show both the natural and optimized variants, but keep the onstage explanation focused on the differences people can see immediately.
-->

---

# How much work happened?

<div class="comparison-results mt-8">
  <div class="comparison-row header">
    <div>State</div>
    <div>React renders</div>
    <div>Selectors / reactions</div>
    <div>Update time</div>
  </div>
  <div class="comparison-row"><div>React</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>Redux</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>Zustand</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>Jotai</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>MobX</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row legend"><div>Legend State</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
</div>

<!--
Replace every TBD with measured results from the reproducible app.

Render count explains visible React work. Selector and reaction executions expose work that render counts hide. Update time measures the complete state path before React commit.

If another library wins a metric, show it. The comparison becomes more credible when it is not a wall of Legend State victories.
-->

---

# Fast by default?

<div class="comparison-results compact mt-8">
  <div class="comparison-row header">
    <div>State</div>
    <div>Natural</div>
    <div>Optimized</div>
    <div>Code added</div>
  </div>
  <div class="comparison-row"><div>React</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>Redux</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>Zustand</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>Jotai</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row"><div>MobX</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
  <div class="comparison-row legend"><div>Legend State</div><div>TBD</div><div>TBD</div><div>TBD</div></div>
</div>

<!--
Performance ceiling is only half the story.

Compare the first natural implementation with the fully optimized version. Then show how much state-specific code and how many concepts were added to reach that ceiling.

That is what Fast by Default means: both the result and the path to it.
-->

---

# Performance vs. architecture

<div class="media-placeholder metric-placeholder mt-8">
  <div class="placeholder-kicker">CHART PLACEHOLDER</div>
  <div class="placeholder-title">Scatter plot: update time × state-specific code</div>
  <div class="placeholder-detail">Place each natural and optimized implementation from measured results. Desired position: fast and simple.</div>
</div>

<!--
This should be the summary visualization.

The vertical axis is update time or CPU. The horizontal axis is state-specific code and architecture required.

The ideal library is at the bottom-left: fast without requiring the application to be reorganized around the library.
-->

---

# The state library I want

<div class="flex flex-col gap-5 text-2xl text-left w-[760px] mx-auto mt-8">
  <div><span class="text-blue-300 font-bold">1.</span> State follows the domain</div>
  <div><span class="text-blue-300 font-bold">2.</span> Subscribe to any leaf</div>
  <div><span class="text-blue-300 font-bold">3.</span> Subscribe to any derived answer</div>
  <div><span class="text-blue-300 font-bold">4.</span> Track dynamic dependencies</div>
  <div><span class="text-blue-300 font-bold">5.</span> Persist and sync the same state</div>
</div>

<!--
So what do I actually want from a state library?

State should follow the domain, not the component tree. Any leaf or derived answer should become a subscription. Dependencies should change dynamically based on what was actually read. And the same state should extend into effects, persistence, and synchronization.

That is the design target for Legend State.
-->

---

# Legend State

<div class="text-3xl mt-8 text-gray-300">Fine-grained state for React</div>

<!--
This is why I built Legend State.

Not because I wanted another spelling for setState. I wanted state ownership and subscription to be separate decisions, without forcing the domain into an atom graph or the component into an observer wrapper.

And I wanted the fast path to be the obvious path.
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
We return to the original domain model.

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

They can live wherever the UI structure makes sense. We do not need providers and matching component topology just to create those boundaries.
-->

---

# Subscribe to the answer

```tsx
function TrackRow({ track }) {
  const activeProgress = useValue(() =>
    playback$.activeTrackId.get() === track.id
      ? playback$.progress.get()
      : null
  )

  return (
    <TrackContent
      track={track}
      activeProgress={activeProgress}
    />
  )
}
```

<!--
The row subscribes to the answer it actually needs.

Every row tracks whether it is active. Only the active row reads progress, so only that row subscribes to progress updates.

When activeTrackId changes, the old and new selected results change. Every other row still resolves to null, so React does not render them.
-->

---

# Notify the value that changed

<div class="node-notification mt-12">
  <div class="node-object">
    <div class="node-row quiet">activeTrackId</div>
    <div class="node-row active">progress <span>● listener</span></div>
    <div class="node-row quiet">duration</div>
  </div>
  <div class="work-arrow">→</div>
  <div class="render-node expected">Active progress UI</div>
</div>

<!--
This is the architectural performance advantage.

Legend State keeps listeners at nodes within the observable tree. A progress write notifies progress listeners rather than broadcasting the whole store.

It creates path metadata lazily, does not modify the underlying data, and tracks only values read by each useValue computation.

That is why it can do less work before React as well as inside React.
-->

---

# Prove it

<div class="media-placeholder metric-placeholder mt-8">
  <div class="placeholder-kicker">BENCHMARK PLACEHOLDER</div>
  <div class="placeholder-title">Current versions · source · raw results · QR code</div>
  <div class="placeholder-detail">Show the most relevant measured win and link to the complete reproducible repository.</div>
</div>

<!--
Put the strongest honest result here, with a QR code to every implementation and the raw measurements.

The talk should not ask people to trust a chart made by the winning library author. Let them inspect it, run it, and improve competing implementations.

If the measurements disagree with the thesis, we change the thesis—not the measurements.
-->

---

# Rendering is only the beginning

```tsx
syncObservable(playback$, {
  persist: {
    name: 'playback',
    retrySync: true,
  },
})
```

<div class="mt-8 text-2xl text-gray-300">Render · Persist · Offline · Sync</div>

<!--
Most state libraries stop once React renders correctly.

Production state also has to survive a restart, work offline, and often synchronize remotely.

Legend State uses the same observable graph for local persistence, optimistic changes, retrying pending writes after restart, and remote sync. The UI still just reads and writes the same state.
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
    <div class="text-green-300 mt-5">Legend State</div>
  </div>
</div>

<!--
This is why LegendList belongs in the story.

Virtualization controls how many rows exist. State invalidation controls which mounted rows update.

A fast list minimizes mounted work. Fine-grained state minimizes update work inside that window.
-->

---

# Try one hot path

<div class="mt-12 grid grid-cols-2 gap-10 w-[850px] mx-auto">
  <div class="choice-card">
    <div class="text-2xl font-bold">No state library?</div>
    <div class="text-gray-300 mt-5">Build the next shared feature with Legend State</div>
  </div>
  <div class="choice-card">
    <div class="text-2xl font-bold">Already using one?</div>
    <div class="text-gray-300 mt-5">Move one hot screen and compare it</div>
  </div>
</div>

<!--
You do not need to rewrite your whole app on Monday.

If you do not use a state library, build your next shared feature with Legend State instead of lifting volatile state through the tree.

If you already use one, move one hot screen. Compare renders, state work, CPU, and the amount of architecture required.
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

# Fast by Default

<div class="mt-10 text-3xl text-gray-300">Use a state library. Make it Legend State.</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div>@jmeistrich</div>
</div>

<div class="absolute bottom-12 right-16 text-gray-500">legendapp.com</div>

<!--
React is fast at rendering what we ask it to render. Broad state asks it to render too much.

A state library is necessary for the fastest React architecture. Fine-grained state is the fastest category. And Legend State combines precise leaves, derived subscriptions, cohesive objects, and production sync in one system.

Use a state library. Make it Legend State.

Thank you!
-->
