# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
The server opened up:
2026-06-14 13:19:11.431 Uvicorn server started on 0.0.0.0:8501

  You can now view your Streamlit app in your browser.

  Local URL: http://localhost:8501
  Network URL: http://XXX.XXX.XX.XXX:XXXX

- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

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

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
