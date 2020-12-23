# Packing Game

This is a 2D geometric bin-packing game that can be played in the browser.
This game is currently a minimal prototype.

## How to play

Drag-and-drop rectangles from the left side (arena)
to the minimum number of bins on the right side.

## How to contribute

* Help with design decisions. See the issues labeled 'design'.
We want you to come up with precise descriptions of what the UI should look like
and how would the user interact with it.

* Try out the game and suggest improvements.
Open an issue for your suggestion after ensuring that
there isn't already an issue for it.

* Help in implementing stuff. See the issues for what needs to be done.
Before making a major change, preferably talk about the change to ensure
that you're implementing the right thing.

## Guidelines for adding a new feature

The game is written in a layered manner (see `script.js`),
and additions to code must adhere to this structure.

Consider the example of picking an item and placing it into a bin (already implemented).
First I wrote code to do this via the developer console:
```js
i = globalGame.items[0];
b = globalGame.bins[0];
i.attach(b, 0, 0);
```
This places item `i` at the top left corner of the first bin.
This allowed me to only focus on the packing logic and not worry about mouse events.
I could even test/debug my code before implementing event handlers.
Later when implementing the event handlers, I just call `attach` at the appropriate place.

Similarly, for loading the game, functions like `loadGameFromGen`, `loadGameFromUrl`
and `loadGameFromUpload` are available.
The game currently lacks a UI (perhaps a toolbar) to call these functions,
so they can alternatively be called from the developer console.

## Using the querystring

The game begins by choosing a 'level'.
A level is a specification of the dimensions of the bins and items (and other info).
We can either (randomly) generate a level or load a pre-defined level from a file.
This level-loading information is obtained from the
[querystring](https://en.wikipedia.org/wiki/Query_string).

Examples:

1. Randomly generate a level (using generator `bp1`)
containing 40 items and bins of dimensions 30 by 30:
<https://sharmaeklavya2.github.io/packing-game/?srctype=gen&src=bp1&n=40&xLen=30&yLen=30>
2. Load a level from the file `levels/bp/1.json`:
<https://sharmaeklavya2.github.io/packing-game/?srctype=url&src=levels/bp/1.json>

If no querystring is provided, the default is `?srctype=gen&src=bp1`.

### Format

The parameter `scaleFactor` gives the size of 1 unit in pixels.

`srctype` decides the type of source to load the game from. Currently supported values are
`gen` (loading from a generator) and `url` (loading from a url).

When `srctype` is `url`, the parameter `src` must be provided and should be a URL
(either a relative URL or an absolute URL).
The contents of this URL will be fetched and interpreted as the JSON representation of a game.

When `srctype` is `gen`, the parameter `src` must be provided and should be the name of
a generator. The global JavaScript object `levelGenerators` maps generator names to generators,
so to find out which generators are available, look at the keys of `levelGenerators`
(using the developer console).
For each value `v` of `levelGenerators`, `v.info` gives a brief summary
of what the generator does.

The output of generators can often be tuned by giving additional parameters in the querystring.
For each value `v` of `levelGenerators`, see `v.defaultValues` to know what parameters
are available and what their default values are.
