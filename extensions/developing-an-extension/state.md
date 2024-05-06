---
description: >-
  An explanation of the three types of state in an extension, and when to use
  each.
---

# 📦 State

There are three types of state that are persisted to the database for an extension: Message State, Chat State, and Initialization State. In your metadata file you may define schemas for them if you wish, but a schema isn't needed to use and persist state.

### Initialization State

Initialization state is for anything that an extension creates _once and only once_ the first time it is instantiated in a chat. If your extension generates a map or setting dynamically with different routes in each chat, but does not need to alter the map, it is a part of initialization state and should be returned in `initState` at the end of your extension's `load` function. On subsequent loads of the extension in the chat (when someone leaves and comes back), the extension will be passed its old initialization state again.

### Message State

Message State is the current state of the extension for a given message in the chat, and is the type of state most used. It is returned `beforePrompt` and `afterResponse`. As an example, for showing character emotions, message state would have a map from each character to their current emotion. Importantly, some things that in a linear chat would belong in something like 'chat state', such as the path traversed to get to the current point, also belong here.

### Chat State

Chat State is an uncommonly-used place for state that applies to the entire chat, _**even across all branches**_. This is unique to the way we handle conversation history as a graph and _**has no analogous concept in any other UI**_. User or character health does not belong here. Position does not belong here. Paths traveled do not belong here. If you think something belongs here, it probably doesn't. It probably belongs in message state. Fog of war belongs here. Bizarre meta-commentary where you chide the user for swiping too much belongs here. It is returned `beforePrompt` ,  `afterResponse`, and on `load`.

## Example: Maze Extension

The maze extension has all three state types. Here's how they're used:

```
type InitStateType = {
    // The maze is generated algorithmically
    //  at the beginning of a chat,
    //  with a configurable size,
    //  and is never the same maze twice.
    // This grid stores where the walls are.
    maze: MazeGrid
};

// The maze's message state is just the player's location,
//   and a generated image being displayed currently, if any.
type MessageStateType = {userLocation: { posX: number, posY: number }, image: string | null };

// The maze's chat state keeps track of what tiles
//   (plus a small radius around them) the user has visited,
//    in order to keep the fog of war on those tiles cleared,
//    even if the user backtracks and goes in a different direction.
type ChatStateType = {
    visited: {[key: number]: Set<number>}
}

// Since the maze is square, we don't have to do anything fancy with nodes,
//   just an array of arrays. Clever no, readable yes.
type MazeGrid = MazeCell[][];

interface MazeCell {
    walls: { [key in MazeWall]: boolean };
    colNum: number;
    rowNum: number;
    // This is used for something else internally,
    //    unrelated to the chat state 'visited'.
    visited: boolean;
}

enum MazeWall {
    down = 'down',
    right = 'right',
    up = 'up',
    left = 'left',
}
```
