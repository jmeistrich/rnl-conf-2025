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

# Legend List 2.0

# The Fastest List for React Native & React

</div>

<div class="absolute bottom-16 gap-y-2 flex flex-col">
  <div>Jay Meistrich</div>
  <div class="flex gap-x-5">
    <div>𝕏 @jmeistrich</div>
    <div>🦋 @jayz.us</div>
  </div>
  <div class="text-gray-400">React Universe - Sept 4, 2025</div>
</div>

<!--
Hi I'm Jay!

It's super cool to be up here. This was my first ever conference in 2018 and it's an honor to finally be speaking here.

I'm here to talk about LegendList, which is now the fastest list for both React Native and React.

First a quick question - raise your hand if you've ever had performance problems with long lists in React or React Native.
-->

---

<img src="/media/list-cards-text.png" class="max-h-[380px] rounded-lg" />

<!--
React Native is amazing in almost every way, but this seemingly simple thing, just having items in a list, was suprisingly slow. So for some reason I decided to try to solve it, and I thought it would be easy.
-->

---

<div>
    <img src="/media/meme-hard.jpg" class="border-primary rounded-xl">
</div>

<!--
It was not easy. But here we are! And it's going pretty well! And I have two annoucements today.
-->

---

<div class="text-5xl font-bold">

🎉 &nbsp;&nbsp; 2.0 &nbsp;&nbsp; 🎉

</div>

<!--
LegendList version 2 is now stable and released!
-->


---

<div class="text-5xl font-bold">

🎉 &nbsp;&nbsp; Web beta &nbsp;&nbsp; 🎉

</div>

<br /><br />

### React DOM + React Native Web

<br />

<div class="text-gray-300">

### 2.1.0-beta.0

</div>

<!--
And web support is now in beta. It's worked in React Native Web for a while, but now it works in React DOM too.

This has been months of work to completely rewrite the core without breaking anything, and to try to make it faster.
-->

---

# Today's Plan

- Advanced performance debugging secrets
- Why V2 is faster and more accurate
- Making it fast on web too

<!--
So today I want to tell you about how the changes in V2 work, some new things I've learned about optimizing, and some differences in optimizing for web.

First I'll show you all the advanced tools I use for performance debugging.
-->

---

<div class="transition-transform duration-300" :class="$clicks > 0 ? 'translate-y-[0]' : 'translate-y-[4rem]'">

# console.log

</div>

<div v-click :class="$clicks > 0 ? 'opacity-100' : 'opacity-0'" class="transition-opacity delay-100 duration-500">

```tsx
const start = performance.now()

// Do the things

console.log('[calculate time]:', performance.now() - start);
// [calculate time]: 2137
```

</div>

<!--
console.log

2. Seriously. I just use console logs to see what's actually happening, in what order, and how long it takes.
-->

---

<div>
    <img src="/media/hirbod.png" />
</div>

<!--
And my friend Hirbod gave me a really slow phone when I started this project. Because optimizing is mostly about how it looks on slow devices.

I run a script to scroll at a consistent speed, using this very slow phone. And I cannot emphasize enough how slow this phone is.

And I just watch how it behaves. Watching the FPS monitor is a big help, but it's also important to feel it with a low framerate, because phones gonna be slow.
-->

---

# Render less, less often

<!--
My optimization philosophy is just three words: render less, less often.
-->

---

<div class="flex items-center">
    <div class="box-not-flashing-small">LegendList</div>
    <div class="box-flashing-small">Containers</div>
    <div class="flex flex-col gap-4">
        <div class="box-flashing-small">Container 0</div>
        <div class="box-flashing-small">Container 1</div>
        <div class="box-flashing-small">Container 2</div>
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
The normal way to update items in view would be to create a new array of items, re-render the Containers with the new array, and let React do all the diffing and reconciliation all the way down.

But list scrolling is a huge bottleneck, so to maximally optimize it we need to get a little ***nuts*** and do it differently.
-->

---

<div class="flex items-center">
    <div class="box-not-flashing-small">LegendList</div>
    <div class="box-not-flashing-small">Containers</div>
    <div class="flex flex-col gap-4">
        <div class="box-container container-flashing-1">Container 0</div>
        <div class="box-container container-flashing-2">Container 1</div>
        <div class="box-container container-flashing-3">Container 2</div>
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
So Legend List skips all of that, it never re-renders the Containers array. So while you're scrolling down and a new item comes on screen it recycles an old container and signals it to re-render itself at a new position with a new item.

It results in the same thing on screen, but it skips a bunch of work in the middle, and re-renders only the one container that changed.

But that's how Legend List already worked, so let's get to the one reason why version 2 is so much better.
-->

---

# maintainVisibleContentPosition

<!--
maintainVisibleContentPosition. It's finally correct.

It seems like just one feature but it's actually the key to everything.

It's also surprisingly hard to say so I'm gonna call it MVCP from here on.
-->


---

<div class="h-[100px] relative -mt-8 w-80 px-4 relative">
    <div class="mvcp-item-above w-[280px] bg-purple-500 rounded flex items-center justify-center text-white font-medium absolute bottom-0 transform">
        Item Above
    </div>
</div>
<div class="text-sm mt-4 mb-2 text-center text-gray-400">Viewport</div>
<div class="w-80 border-2 border-dashed border-gray-500 overflow-hidden p-4 flex flex-col gap-2">
    <div class="h-12 bg-blue-500 rounded flex items-center justify-center text-white font-medium">
        Visible Item 1
    </div>
    <div class="h-20 bg-green-500 rounded flex items-center justify-center text-white font-medium">
        Visible Item 2
    </div>
    <div class="h-16 bg-pink-500 rounded flex items-center justify-center text-white font-medium">
        Visible Item 3
    </div>
</div>

<!--
MVCP makes it so that when items change size above the viewport, the items you're looking at in the viewport don't move.

This is very important when items are dynamic sizes because we will first render an item at an estimated size, and then adjust it after it renders and we measure the actual size.

Without it, everything would be jumping around while scrolling up or scolling to index because positions keep changing.
 -->

---

<div>
    <img src="/media/issue-reopen.png" class="border-primary rounded-xl">
</div>

<!--
The way we did MVCP in V1 sometimes had a delay of one frame, or affected momentum scrolling, or caused the underlying ScrollView to scroll itself.

But with a few small changes I finally fixed it for good.

About 5 times. People kept finding new ways to break it. I was on an emotional rollercoaster of finally fixing it at night and waking up to messages that it's still broken.
-->

---

<div class="phone-container scale-[1.1]">
    <img src="/media/phone.png">
    <SlidevVideo src="/media/mvcp.mov" autoreset="slide" autoplay mute loop class="phone-screen" />
</div>

<!--
So I went back to basics and found a new and better MVCP algorithm, which is instant and doesn't affect scroll momentum at all.

You can see in this video I'm scrolling up infinitely and continually adding items of random sizes above the viewport, and it's perfectly smooth.
-->

---

<img class="img-border p-2 bg-black" src="/media/gitdiff.png" />

<!--
MVCP working correctly also let me remove hundreds of lines of hacks and workarounds that were previously needed to make it work reliably.

And it just made everything easier.
-->

---

<div class="phone-container scale-[1.1]">
    <img src="/media/phone.png">
    <SlidevVideo src="/media/scroll-to-index.mp4" autoreset="slide" autoplay mute loop class="phone-screen" />
</div>

<!--
Most importantly, it enabled perfectly accurate scrollToIndex.

The problem is with dynamically sized items, we don't know the actual target position when starting a scroll because we don't know the actual size of everything. So we have to guess based on estimates and adjust positions while scrolling. In version 1 it was working okay, but it could sometimes miss the target. We had a bunch of ***silly*** workarounds like just scrolling multiple times to try to get it right.

With the new MVCP, when you scroll to an estimated position, the target element moves itself into that estimated position, so the estimate becomes the correct position.

And it's perfectly accurate every time.
-->

---

<div class="phone-container scale-[1.1]">
    <img src="/media/phone.png">
    <SlidevVideo src="/media/sticky.mov" autoreset="slide" autoplay mute loop class="phone-screen" />
</div>

<!--
It also made it easy to add stickyIndices, which was a surprisingly highly requested feature although I'd never used it myself, and a blocker for a lot of apps.

We'd attempted it unsuccessfully a few times before, with big PRs and long discussions, but after v2 I was able to do it in an hour on a train ride.
-->

---

<div class="flex items-center">
    <div class="box-not-flashing-small">Containers</div>
    <div class="flex flex-col gap-4">
        <div class="box-container">Container 0</div>
        <div class="box-container">Container 1</div>
        <div class="box-container">Container 2</div>
    </div>
    <div class="flex flex-col gap-4">
        <div class="box-container container-flashing-mvcp-1">PositionView 0</div>
        <div class="box-container container-flashing-mvcp-2">PositionView 1</div>
        <div class="box-container container-flashing-mvcp-3">PositionView 2</div>
    </div>
    <div class="phone-container">
        <img src="/media/phone.png">
        <div class="absolute inset-x-6 rounded-[32px] top-[22.5px] bottom-[21px] overflow-hidden bg-white pt-2">
             <div class="box border-none m-2 bg-purple-500 mvcp-item-above-2">
                Item Above
            </div>
            <div class="box border-none m-2 h-24 bg-red-500 font-medium">
                Container 0
            </div>
            <div class="box border-none m-2 h-32 bg-blue-500 font-medium">
                Container 1
            </div>
            <div class="box border-none m-2 h-48 bg-green-500 font-medium">
                Container 2
            </div>
        </div>
    </div>
</div>

<!--
It also allowed an optimization I had attempted multiple times. Positions need to update very often, after the initial mount and any time an item size changes. So it's a shame to have to render the whole container just for a style change.
-->

---

```jsx
function PositionView ({ id, style, ...rest }) {
    const position$ = useValue$(`containerPosition${id}`);

    return (
        <Animated.View
            style={[
                style,
                { transform: [{ translateY: position$ }] }
            ]}
            {...rest}
        />
    );
}
```
<!--
So the part that sets position style is now its own tiny subcomponent that only updates when position changes.

On old architecture it actually sets an Animated position so it doesn't even render at all.
-->

---

<div class="border-primary rounded-lg">
    <div class="code-title mb-0">All in one frame</div>
    <div class="flex items-center p-4">
        <div class="box-not-flashing-small whitespace-pre">
            Render
        </div>
        =>
        <div class="box-not-flashing-small whitespace-pre">
            useLayoutEffect
        </div>
        =>
        <div class="box-not-flashing-small whitespace-pre">
            measure
        </div>
        =>
        <div class="box-not-flashing-small whitespace-pre">
            updatePositions
        </div>
        =>
        <div class="box-not-flashing-small whitespace-pre">
            Re-render
        </div>
    </div>
</div>

<!--
In new architecture the optimization is different because measuring layout is synchronous, and a cool feature of React is that setting state in useLayoutEffect renders again before painting to screen.

So we can update an element, measure its new size, update positions in the list, calculate what's in view, and re-render the new positions, all in one frame.

So although we have to render twice, it only takes one frame visually, so it feels very fast.
-->

---

# 🎉 &nbsp;&nbsp; Web &nbsp;&nbsp; 🎉

<!--
And that brings me to the next big thing that the new MVCP opened up. It made it much easier to build a web version.
-->

---

<div class="flex items-center">
    <div class="phone-container">
        <img src="/media/phone.png">
        <div class="absolute inset-x-6 rounded-[32px] top-[22.5px] bottom-[21px] overflow-hidden bg-white">
            <div class="box border-none absolute inset-x-8 top-[10px] h-[60px] bg-red-500 font-medium flex-col">
                <div>Item 2</div>
            </div>
            <div class="box border-none absolute inset-x-8 top-[80px] h-[60px] bg-blue-500 font-medium flex-col">
                <div>Item 1</div>
            </div>
            <div class="box border-none absolute inset-x-8 top-[150px] h-[60px] bg-green-500 font-medium flex-col">
                <div>Item 3</div>
            </div>
            <div class="box border-none absolute inset-x-8 top-[220px] h-[60px] bg-purple-500 font-medium flex-col">
                <div>Item 7</div>
            </div>
            <div class="box border-none absolute inset-x-8 top-[290px] h-[60px] bg-teal-500 font-medium flex-col">
                <div>Item 6</div>
            </div>
            <div class="box border-none absolute inset-x-8 top-[360px] h-[60px] bg-pink-500 font-medium flex-col">
                <div>Item 4</div>
            </div>
            <div class="box border-none absolute inset-x-8 top-[430px] h-[60px] bg-orange-500 font-medium flex-col">
                <div>Item 0</div>
            </div>
        </div>
    </div>
</div>


<!--
Going into it I thought the main difference would be that web needs items to be in order for contiguous text selection. In React Native the order doesn't really matter, so Legend List's container recycling does not care about order at all, so items end up in random positions.
-->

---

<SlidevVideo src="/media/order-problem.mov" autoreset="slide" autoplay mute loop class="video-rounded" />

<!--
But on web, you need to be able to highlight across the page. And it needs to highlight everything you highlighted. But if elements are in a random order, highlighting looks ***insane***.

So I might have to completely change the algorithm for web to maintain order.

But first let's see the variety of techniques in other list libraries.
 -->


---

<SlidevVideo src="/media/virtuoso.mov" autoreset="slide" autoplay mute loop  />

<!--
Let's start with Virtuoso. It renders the items in view with normal block layout, setting a paddingTop on the scroller to take the place of the items above the viewport.

You can see that as I scroll the highlighted element is moving up and down the DOM.

Using normal layout keeps the elements in order, but it also makes the browser layout the whole list whenever an item is added or removed. Plus, padding is a layout property so setting that will make the inner list layout.
-->

---

<SlidevVideo src="/media/tanstack.mov" autoreset="slide" autoplay mute loop  />

<!--
Now let's look at an example I saw in the TanStack Virtual docs. It works similarly, rendering the items in view normally. But it handles the top offset by setting a transform. That seems like a good optimization right? Because changing transform is always faster than a layout property? But the items array inside is changing so it's laying out anyway, so it's not really preventing a layout.

And in this case transforming actually has a huge negative effect because it's on such a big element. In a browser when you set a transform on an element, it creates a new layer on the GPU. So if you have a list that's 100,000 pixels tall, the GPU has to tile it into multiple 16k chunks, transform all of them, stitch them back together, and composite it onto the page.

So while it might avoid a small layout, it hits you hard in graphics memory and compositing time.
-->

---

<SlidevVideo src="/media/virtua.mov" autoreset="slide" autoplay mute loop  />

<!--
Now let's look at Virtua. It works quite differently, absolutely positioning each item. This takes it out of the layout flow, so changing an absolutely positioned element does not reflow the outer list. And it can skip offsetting the top since they're positioned correctly.

So that's great, it should be avoiding a lot of layouts.
-->

---

# Render less, less often

<!--
But the thing they all have in common is that when one item comes into view, it re-renders the whole list.

That does a bunch of work in React. And then some DOM elements get destroyed or some get created.

So I wondered how performance would be impacted by updating only one item at a time.
 -->

---

<SlidevVideo src="/media/legendlistweb.mov" autoreset="slide" autoplay mute loop  />

<!--
If we just port over the algorithm from React Native, LegendList also positions absolutely to avoid those layouts, but when a new item comes into view, it signals a single container to re-render itself.

That just does a lot less work. It skips all of the work in React, of diffing and reconciling the list. And it updates fewer DOM elements.

But...
 -->

---

<img class="img-border" src="/media/out-of-order.webp" />

<!--
now we have a big problem, list items are out of order.
-->

---

# CSS order

<div class="relative">
    <img class="img-border max-h-[440px]" src="/media/order.png" />
    <div v-click class="absolute inset-0 flex flex-col items-center justify-center" >
        <img src="/media/red-x.png" />
    </div>
</div>

<!--
I asked Claude for ideas and it suggested using the CSS order property. It can effectively display elements in a different order than they are in the DOM. Pretty cool right?

2. NOPE. It didn't work at all. It's only visual, it has no effect on selection or accessibility.
 -->

---

<div class="relative size-[440px] flex flex-col items-center justify-center">

# ✨ Web Workers ✨

<div v-click class="absolute inset-0 flex flex-col items-center justify-center" >
    <img src="/media/red-x.png" class="size-[440px]" />
</div>

</div>

<!--
So I asked Claude again and it said web workers are good for performance. But what does that have to do with anything?

2. That was useless. So I had to figure something else out.
 -->

---

# Reorder the DOM manually

<br />

<div class="flex items-center">
    <div class="box-not-flashing-small">LegendList</div>
    <div class="box-not-flashing-small">Containers</div>
    <div class="flex flex-col gap-4">
        <div class="box-container container-flashing-1">Container 0</div>
        <div class="box-container container-flashing-2">Container 1</div>
        <div class="box-container container-flashing-3">Container 2</div>
    </div>
</div>

<!--
It turns out that we can just move DOM elements around.

Normally adding or removing elements that React is controlling can crash your app, because when React re-renders it would try to update DOM elements that don't exist anymore.

But as I showed before, our Containers component never re-renders, only the elements inside do. So we can actually safely rearrange the inner DOM elements manually, and React can still update the list items totally fine.
 -->

---

<SlidevVideo src="/media/legendlistweb-reorder.mov" autoreset="slide" autoplay mute loop  />

<!--
So Claude did end up helping me to make an optimal reordering algorithm which does the smallest possible number of updates to the DOM to put it in order.

But being out of order won't really be a problem while scrolling quickly, which is when performance is the tightest, so we can just debounce it until scroll slows down.

I put an extra long delay on it for this video so you can see it happen, but it should be much sooner. And this can probably be a prop to control its behavior.
 -->

---

<SlidevVideo src="/media/comparison.mov" autoreset="slide" autoplay mute loop />

<!--
So then doing a comparison of the libraries, I added some slowdown to the list items to see what it looks like with heavy components on a slow device.

In this test, Virtuoso and TanStack Virtual have some trouble. while Virtua does pretty well. And LegendList is already faster.

But LegendList's web support is so new it's mostly just the same logic from mobile. So I think there's still a lot more room for web-specific optimizations, and we can make it even faster.

So why is it faster than the others?
-->


---

# 1. Render less, less often

- Less work in react
- Less browser layout
- Less browser compositing

<!--
It's just doing smaller renders, only the one item that changed rather than the whole list. So it does both less work in React and less work in the browser.
 -->

---

# 2. Recycling

- React components
- DOM elements

<!--
And recycling, which we're used to on mobile but isn't really a thing on web.

Rather than destroying and re-creating elements when they go in and out of view, we can reuse them.

So if only an image source or some text is different, updating those tiny components and DOM elements is much less work than constructing a full component from scratch.
 -->


---

# MVCP on Web

<!--
The last big challenge on web is how to do MVCP. In React Native it uses a feature built into ScrollView, so for web I had to implement it myself.

That turned out to be pretty easy though.
-->

---

```ts
// scrollBy doesn't break momentum scroll
const maintainVisibleContentPosition = (deltaY) => {
    scrollRef.current.scrollBy(0, deltaY)
}
```
<!--
We can just use scrollBy to adjust the scroll, and that doesn't affect momentum scrolling at all. This seems to work well on all browsers except mobile Safari. So I still need to figure that one out.
-->

---

<SlidevVideo src="/media/web-mvcp.mov" autoreset="slide" autoplay mute loop class="rounded-md" />

<!--
From what I can tell, none of the other libraries can do infinite scroll up or initialScrollIndex very well.

But with MVCP working perfectly, we can start at an initial scroll index and scroll up with no jank. And we could scroll up infinitely if we wanted, just like we can on mobile.
-->

---

# Web + Mobile

<!--
So then the next challenge is an architecture one. How do I make one library that works on both web and mobile with the same API?
-->

---

# New Architecture closer to Web

<div class="flex gap-x-4 code-cols pt-8">
    <div>
        <div class="code-title">React Native</div>

```tsx
// React Native
useLayoutEffect(() => {
    let rect: LayoutRectangle;
    view.measure((x, y, width, height) => {
        rect = { height, width, x, y };
    });
    updatePositions(); // Re-render in same frame
}, []);
```

</div>
<div>
    <div class="code-title">React</div>

```tsx
// React
useLayoutEffect(() => {
    const rect = element.getBoundingClientRect();
    updatePositions(); // Re-render in same frame
}, []);
```

</div>
</div>

<!--
LegendList already splits behavior in many places between old and new architecture. The key difference on new architecture is that we can measure layout synchronously, and we can update positions immediately within useLayoutEffect.

On web we have getBoundingClientRect which is also synchronous, so the web version can take the new architecture path. And the general logic can be mostly the same on web as it is for the new architecture.
 -->

---

# Passive

<div class="flex gap-x-4 code-cols pt-8">
<div>
    <div class="code-title">React Native</div>

```tsx
// Passive handler
return <View onScroll={onScroll} />
```

</div>
<div>
    <div class="code-title">React</div>

```tsx
// Passive handler
element.addEventListener("scroll", handleScroll, { passive: true });

// Active handler
element.addEventListener("scroll", handleScroll);
```

</div>

</div>

<!--
On web there's two kinds of event handlers, active and passive. Active handlers are synchronous and block everything until they're finished, whereas passive handlers are asynchronous and don't block.

In React Native the onScroll event is always passive, because it's the safest and highest performance version.

There's a tradeoff. With a passive listener, if rendering is slow you'll see blank spaces. But with an active listener if rendering is slow you'll slow down the scroll itself.

So since LegendList is already built for passive listeners I've started with doing the same on web. But it's possible that the active version could be better, I still need to experiment with it, so we'll see how it goes.
 -->

---

<img src="/media/usain.webp" class="max-h-[440px] rounded-md" />

<!--
So with all that, LegendList V2 is even faster in React Native and is now the fastest list on web too.

And it's very powerful with all these features now working very reliably.
-->

---

# What's Next

1. 2.0 released today
2. Web beta: 2.1.0-beta.0
3. Make it perfect

<!--
So what's next?

2.0 is release today. Give it a try, it's much better than version 1.

Web support is in beta today. It barely works, but I'll be improving it over the next few weeks.

I'm excited to see what you all can build with this. And I need your help!
-->

---

<SlidevVideo src="/media/comments.mp4" autoreset="slide" autoplay mute loop class="video-rounded h-[480px]" />

<!--
So please report any problems you see and send PRs. Everybody uses lists in a different way so it's very possible that I never even thought to test for a bug you found.
-->

---

<div class="bg-white absolute inset-0 flex items-center justify-center">
    <img src="/media/endpage.001.png"></img>
</div>

<!--
And if you want your apps to be really fast, I also make Legend State which is the fastest state library for React, as well as a full local first sync engine.

If you see me around, let's chat about performance. Or talk to me on the various internets and let me know if I can help you with anything.

Thank you!
-->