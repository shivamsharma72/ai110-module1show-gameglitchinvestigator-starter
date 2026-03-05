# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

**What did the game look like the first time you ran it?**

It looked like a web UI in Streamlit asking me to guess a number, with three difficulty levels in the sidebar and a text input for guesses.

**List at least two concrete bugs you noticed at the start.**

The hints were backwards: when I guessed lower than the target it said go lower, and when I guessed higher it said go higher. When I clicked New Game, the score did not reset and history was not cleared. On even attempts you could never win because the secret was compared as a string so the guess never matched. The UI always said 1 to 100 even on Easy or Hard, and New Game always picked from 1 to 100 instead of the current difficulty. Hard used 1 to 50 while Normal used 1 to 100, so Hard was actually easier.

---

## 2. How did you use AI as a teammate?

**Which AI tools did you use on this project?**

I used Cursor with Claude to understand the app flow, list the bugs, and implement fixes.

**Give one example of an AI suggestion that was correct.**

The AI suggested running tests from the project root so that logic_utils could be imported. I ran the command from the folder that has both logic_utils and the tests folder, and all three tests passed.

**Give one example of an AI suggestion that was incorrect or misleading.**

When I ran pytest from inside the tests folder, the command failed with file or directory not found because from there the path to the test file is different. The fix was to run from the project root so the test path and imports work.

---

## 3. Debugging and testing your fixes

**How did you decide whether a bug was really fixed?**

I checked by playing the app, clicking New Game, changing difficulty, and confirming hints and score, and also by running pytest for the guess logic. I only considered a fix done when the UI behaved right and the tests passed.

**Describe at least one test you ran and what it showed you about your code.**

I tested manually first: when the secret was 67 and I guessed 55, the app said go lower instead of go higher, which confirmed the hint logic was wrong. I also saw that history and score were not cleared on New Game. Running pytest showed that check_guess() returns two values, so the tests had to be updated to use the first one for the outcome.

**Did AI help you design or understand any tests? How?**

The AI pointed out that the tests expected one return value but check_guess() returns two, and suggested unpacking in the tests. After that change, the tests passed and matched how the app uses the function.

---

## 4. What did you learn about Streamlit and state?

**In your own words, explain why the secret number kept changing in the original app.**

The script runs from top to bottom on every click or input. If the secret were a normal variable it would be recomputed every time and the number would change. Keeping it in session state and only setting it when it is missing or when starting a new game makes it persist so the same secret is used until the player starts over.

**How would you explain Streamlit reruns and session state to a friend who has never used Streamlit?**

Every time you click a button or change a widget, Streamlit reruns the whole script from the top. Normal variables are lost between reruns. Session state is a place that survives reruns for that browser tab, so you store the secret number, attempts, and score there. The script reads and updates session state so the game remembers between clicks.

**What change did you make that finally gave the game a stable secret number?**

The secret was already in session state. The real problem was that on even attempts the code passed the secret as a string into the comparison, so the guess could never match and you could not win on those turns. The fix was to always pass the secret as an integer so the comparison is consistent and the player can win every time.

---

## 5. Looking ahead: your developer habits

**What is one habit or strategy from this project that you want to reuse in future labs or projects?**

I will run tests from the project root and use the Debug expander to see live state while testing the UI. I will also keep game logic in a separate module so it can be tested with pytest without running Streamlit.

**What is one thing you would do differently next time you work with AI on a coding task?**

I would double check run instructions so I do not waste time on import or file not found errors. I would also ask the AI to list all suspected bugs and fixes in one place before changing code so I have a clear checklist.

**In one or two sentences, describe how this project changed the way you think about AI generated code.**

AI generated code can look fine but have subtle bugs like wrong hints, type mismatches, or state not resetting. Treating it like a code review and testing both manually and with unit tests makes it easier to find and fix those issues instead of assuming the first draft is correct.
