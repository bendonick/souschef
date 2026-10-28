# What went wrong

The three incidents I learned the most from, and the small traps that each cost an afternoon.

## Eleven days of deploys that failed, and nobody noticed

In late June the API refused to start in production. A safety check I had written required one environment variable, and it was missing on the server. The hosting platform kept serving the last good build, so the app looked fine. For eleven days every merged change was not live.

Now the health endpoint reports which commit is running, and a workflow checks it after every push until the new commit answers. If it does not within fifteen minutes, I get an email.

The lesson: a guard that refuses to start is only safe if it also tells someone. "Nothing visibly broke" is not the same as "it worked".

## The matcher merged the wrong foods

The first pantry matcher guessed whether two names were the same food from a similarity score. It was good at "chicken breasts" and "Chicken-Breast". It was also happy to merge two foods that were not the same, and the numbers on a product I trusted changed without any sign.

I removed the guessing and replaced it with a lookup in a fixed list of known foods. A set of 43 test cases now runs before any change to that list.

The lesson: pick the failure you can see. A duplicate is visible. A wrong merge is not.

## Green tests, eight bugs

By September every automated check passed, including thirty scripted flows that drive the real app in the iOS Simulator. Then I spent three evenings using the app by hand and found eight problems. The worst: from the second receipt in a session, the review screen showed the first receipt again.

The lesson: a scripted flow only checks what it was written to check. Every flow started fresh, so a second receipt was never in any of them. Hands-on use found it in an evening.

## Small things that cost an afternoon each

- macOS ships an old bash where an empty array counts as unset under strict mode.
- A Simulator test run that finds no device prints neither pass nor fail, and looks green.
- The sign-in endpoint allows ten attempts a minute. From the app, the eleventh looks exactly like a wrong password.
- A flow's own environment block silently overrides the one passed on the command line, so one run tested the wrong branch and passed.
