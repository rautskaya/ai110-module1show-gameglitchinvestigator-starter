# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
I needed to guess a number between 1 and 100. Attempts left: 7
The game showed developer debugger info with attempts history and hints.

- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").
  1) After I played one time, I pressed New Game button and it was not responsive. I needed to refresh the page to be able to start a new game.
  2) Mismatch of the count of attempts allowed when starting a new game. On the left panel the text indicated "Attempts allowed: 8" but the text under "Make a guess" showed "Guess a number between 1 and 100. Attempts left: 7".

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
| | | | |
| | | | |
| | | | |

Issue #1 The hint asks to go lower with low input and higher with high input
Input: 30 Secret: 47
Expected Behavior: The hint should say go higher
Actual Behavior: The hint says go lower
Console Output: Game outputs: "Go lower"
#FIX: Refactored logic into logic_utils.py using agent mode and added a test case:  
            if guess < secret:
            return "Too Low", "📈 Go HIGHER!"

Input: 90 Secret: 63
Expected Behavior: The hint should say go lower
Actual Behavior: The hint should say go higher
Console Output: Game outputs: "Go higher"
#FIX: Refactored logic into logic_utils.py using agent mode and added a test case: 
            if guess > secret:
            return "Too High", "📉 Go LOWER!"

Issue #2 Score increases after a wrong (too high) guess on even attempt
Input: Guess 90, Secret 34 (second guess of the game)
Expected Behavior: Score should decrease by 5 for a wrong guess
Actual Behavior: Score increases by 5 
Console Output: Score goes up instead of down after "Too High"
#FIX: Removed in update_score so a "Too High" guess always subtracts:
            if outcome == "Too High":
            return current_score - 5

Issue #3 On pressing the New Game button, the history is not cleared
Input: press on New Game button
Expected Behavior: Should clear history of old game
Actual Behavior: Old inputs are displayed
---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
Claude Code
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
I only noticed one wrong hint (secret 50, guess 70 said "Go lower"), but Claude Code pointed out the comparison logic was reversed both ways. It suggested making `guess < secret` return "Go HIGHER" and `guess > secret` return "Go LOWER". I verified by manually testing a guess below and above the secret and both showed the correct hint.
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
On Hard mode the prompt still said "Guess a number between 1 and 100," and Claude Code suggested just fixing the message text. But the real bug was that New Game used `random.randint(1, 100)` and ignored the difficulty range. I verified by playing several Hard games and seeing secrets above 50 in the debug panel.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
I tested manually and added pytest tests. For each bug I first reproduced it in the game, then made the fix, and replayed the same case to confirm the behavior changed. I also added tests in `test_game_logic.py` (like `test_guess_too_high`, `test_guess_too_low`, and `test_too_high_on_even_attempt_loses_points`) so the fix was checked in both directions and would stay fixed. 
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
I ran `test_too_high_on_even_attempt_loses_points`, which checks that a "Too High" guess subtracts 5 points. It failed before my fix and passed after, which showed me the scoring now works for wrong guesses on every attempt. I also ran `test_guess_too_high` and `test_guess_too_low`, which confirmed the hints point the right way in both directions.
- Did AI help you design or understand any tests? How?
Yes. Claude Code helped me write the test cases in `test_game_logic.py` and explained that I should test both the too-high and too-low directions, not just one. 
Test results:
collected 4 items

tests/test_game_logic.py ....                                            [100%]

============================== 4 passed in 0.01s ===============================


---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
Every time you click a button or type something, Streamlit re-runs the whole script from top to bottom, like reloading the page. That means normal variables get reset each time, so the game would forget your score and secret number. Session state remembers values between reruns, so things like the score, attempts, and secret stay the same until you change them. In this game, that's why the secret number and history are stored in session state instead of regular variables.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
Reproducing a bug first, then writing a test for it before fixing, so I can prove the fix actually works.
- What is one thing you would do differently next time you work with AI on a coding task?
I would test the AI's suggestions more carefully instead of trusting them right away, since a fix that looked right (like just changing the message text) sometimes missed the real bug.
- In one or two sentences, describe how this project changed the way you think about AI generated code.
I learned that AI-generated code can look correct and still hide real bugs, so I can't just trust it. Now I see AI as a helpful teammate whose work I still need to test and verify myself.
