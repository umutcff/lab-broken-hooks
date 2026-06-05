# My Lab Notes

Fill this file in as you work through the lab. Be honest and specific. This file is part of what you hand in.

## What I think is wrong

Write your first impressions here, before asking anyone or any AI for help. Describe what you saw in the browser, in the Console, and in the Network tab. Write down your theory about what is causing each problem.

Problem 1: When clicking on a user to open their details, the Network tab shows an endless stream of requests to the posts API. My theory is that the `useEffect` hook that fetches the posts has `posts` in its dependency array, which triggers a re-fetch every time `setPosts` updates the state with the newly fetched data array.

Problem 2: Clicking the favorite button on a user card does not change the UI. The state is being mutated directly instead of returning a new object reference, causing React to not detect a state change and bail out of re-rendering.

## What did I ask the AI

I did not use an AI assistant for this lab.

## What was the solution

For each problem, explain what the actual cause was, which file and which lines you changed, and why your change fixes it.

Problem 1:
The actual cause was `posts` being in the dependency array of the `useEffect` hook in `app/components/UserDetail.js`. Because `setPosts` creates a new array reference every time the fetch succeeds, the hook thought a dependency changed and ran again, causing an endless loop.
I changed `app/components/UserDetail.js`, removing `posts` from the dependency array. This fixes the issue because the hook now only runs when `userId` changes.

Problem 2:
The actual cause was mutating the existing `users` array and passing the same reference to `setUsers` in `app/components/UsersExplorer.js`. React compares the old state reference with the new one and seeing they are identical, doesn't trigger a re-render.
I changed `handleToggleFavorite` in `app/components/UsersExplorer.js` to return a completely new array, mapping over the previous users and only spreading and updating the specific user object that changed. This gives React a new array reference, triggering the re-render.
