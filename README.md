# CtF_v1
Capture the flag activity - Level 1

https://rhoded-uwp.github.io/CtF_v1/



# CTF Challenge - Build Process & Tools

A single-file, browser-based Capture the Flag activity for a high school Cybersecurity workshop. Students use their browser's developer tools to find 12 hidden flags, submit them for points, and race a timer. The entire thing is one `ctf-challenge.html` file, ready to drop on GitHub Pages or embed in Canvas via iframe.

## Tools Used

- **Claude (Anthropic)** as the development partner, writing and iterating on the code through conversation
- **Single-file HTML/CSS/JavaScript**, no build step, no dependencies, no server
- **Web Crypto API** (`crypto.subtle.digest`) for client-side SHA-256 flag hashing
- **Google Fonts** (JetBrains Mono + Outfit) for the hacker-terminal aesthetic
- Intended deployment: GitHub Pages, embedded in Canvas via iframe

## How It Was Built

The page was built entirely through an iterative back-and-forth with Claude. Each prompt added or refined one piece, and Claude implemented it, then flagged tradeoffs and asked what to do next. The sequence:

1. **Brainstorm hiding spots.** Started by asking Claude for ideas on where to hide flags that teach real inspect-element skills. Claude returned a ranked list spanning HTML comments, attributes, CSS, the console, the network tab, storage, and more, sequenced from easy to hard.

2. **Build the page.** Asked Claude to turn the ideas into a working page with a start button, a timer, a dark hacker theme, and a completion checklist. Claude generated the full single-file artifact with 10 flags, each teaching a different DevTools panel, plus an auto-detecting checklist and progress bar.

3. **Add harder hidden-text flags.** Requested two more same-color-text flags, but tougher, blended into panel backgrounds rather than the page background. Claude added them and bumped the total to 12.

4. **Rename checklist items.** Asked Claude to simplify several flag titles. Claude renamed them and pointed out one item that didn't fit the requested category, leaving it unchanged rather than guessing.

5. **Add scoring.** Specified a formula: 35 points per flag, minus 5 for each full minute elapsed, floored at 5. Claude implemented it, then verified the math against the provided examples before delivering. Also added a floating "+points" animation and a score bump.

6. **Hide the hints behind buttons.** Asked Claude to collapse the overly-helpful hint text under each flag and reveal it via a "Hint" button with an animation. Claude built the reveal with a slide-and-glow effect.

7. **Obfuscate the answer key.** Asked whether the flag array could be hidden from the Sources tab. Claude explained the honest limits (anything the browser reads, the student can read) and recommended SHA-256 hashing as the realistic best fit. Implemented hashing so only digests appear in the source.

8. **Add a hint penalty.** Decided hints should cost 5 points each. Claude added the deduction, a red downward-floating "-5" animation, and a floor so the score never goes negative.

9. **Fix hint text rendering.** When specific hints needed tag names like `<head>` and `<style>` to show, Claude caught that those were being parsed as real HTML and disappearing, and escaped them so they display correctly.

10. **Swamp Ctrl+F.** Asked Claude to add 40+ clearly-labeled fake flags so searching for `flag{` returns a haystack instead of a shortcut. Claude generated 43 decoys, scattered them across the HTML, CSS, and JS, and verified none of them validate as real flags.

## The Role of Claude

Claude acted as the implementer and a second set of eyes, not just a code generator:

- **Translated plain-language requests into working code** without requiring any hand-holding on syntax or structure.
- **Surfaced tradeoffs proactively.** When asked to hide the answer key, Claude was upfront that client-side code can never be truly hidden and framed the obfuscation itself as a teachable cybersecurity lesson rather than overpromising.
- **Caught bugs the prompt didn't mention.** It noticed that tag names in hint text would render as invisible HTML, and that decoys needed checking against the real flag hashes to avoid accidental collisions.
- **Verified its own work.** For the scoring formula, the hashes, and the decoy collisions, Claude ran checks before claiming the feature was done, rather than asserting correctness.
- **Respected explicit constraints.** When a requested rename didn't fit a flag's actual category, Claude left it alone and said so instead of forcing the change.

## Educational Notes

- **The flags are SHA-256 hashed**, so the Sources tab shows only digests, not answers. This defeats casual copy-the-array cheating. It is not cryptographically bulletproof (short flags could be brute-forced or reverse-looked-up), which is itself a useful discussion point about salting and why real systems do more.
- **The scoring logic is still visible** in the source, since only flag values are hashed. That was an intentional scope decision.
- **The 43 decoys** turn Ctrl+F from a shortcut into a lesson: students learn that searching alone is not enough and they have to understand where each flag type actually lives.

## Flag Categories (12 total)

The flags span: HTML comment, JavaScript source, data attribute, meta tag, CSS comment, broken image source, hidden element, same-color text (x3, varying difficulty), console message, and CSS pseudo-element content. Together they push students through the Elements, Sources, Console, and Application panels of DevTools.

---

Built for the CS Cybersecurity Workshop. Stay curious, stay ethical.
