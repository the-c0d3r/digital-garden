---
{"dg-publish":true,"permalink":"/garden/posts/how-ai-should-be-used/","tags":["llm"],"created":"2025-05-19 21:30","updated":"2026-04-07 22:11","dg-note-properties":{"modified_date":"2026-04-07 22:11","creation_date":"2025-05-19 21:30","tags":["llm"],"aliases":["How I use AI","How to use AI"]}}
---

# How AI should be used

topic: [[02-Area/programming/AI/Large Language Model\|LLM]]

## When you want to save time

- Use it as automation tool
- if you already know how to write it, and want to skip the boilerplate, then use LLM.
- if you don't know how to approach or solve a problem, ask LLM to [[03-Resource/knowledge/Divide and conquer\|Divide and conquer]] into steps for you
- if you don't know but it is trivial to pick up then you can use LLM to automate
    - such as like generating [[10-Inbox/streamlit\|streamlit]] code for your application

## When you want to learn new things

- ask for it how to start learning about a topic
    - what are the keywords you should know about
- when you want to brainstorm for new ideas

## Note

- AI will never volunteer to optimise code
- they can only do what you ask of it, and sometimes will outright do things you specifically asked not to.
- you need a proper guidelines for AI to follow such as
	- create unit tests
	- refactor code into different files
	- simplify the code, especially after changing some stuff, they won't refactor even though it's obvious to us
- can't think of solution for you
- can't make proper judgement calls
- AI have a serious [[Garden/knowledge-base/XY problem\|XY problem]].
    - it will only help you try to solve problem, but never stopping to ask why you need to solve it in the first place.

---

## Recommended Prompt for AI chatbots

From now on, do not simply affirm my statements or assume my conclusions are correct. Your goal is to be an intellectual sparring partner, not just an agreeable assistant. Every time I present an idea, do the following:
1. Analyze my assumptions. What am I taking for granted that might not be true?
2. Provide counterpoints. What would an intelligent, well-informed skeptic say in response?
3. Test my reasoning. Does my logic hold up under scrutiny, or are there flaws or gaps I haven’t considered?
4. Offer alternative perspectives. How else might this idea be framed, interpreted, or challenged?
5. Prioritize truth over agreement. If I am wrong or my logic is weak, I need to know. Correct me clearly and explain why.”

Maintain a constructive, but rigorous, approach. Your role is not to argue for the sake of arguing, but to push me toward greater clarity, accuracy, and intellectual honesty. If I ever start slipping into confirmation bias or unchecked assumptions, call it out directly. Let’s refine not just our conclusions, but how we arrive at them.

---

## Vibe coding

Doing this in a language I know makes me feel like I lost control of the code base. So after some trial and error, I found a way to make it work.

Asking AI to write is similar to asking another person (e.g. an intern) to write code for me. When I need to change stuff, I don't want to read through those horrible code myself, so I just ask the person to change. Similar to the AI case here.

1. don't let AI write the whole project. specify the MVP first
2. I make the plan, which file should contain what, and do what. Then AI fills in the blanks.
3. Incrementally add features, and manually go through the diff and accept/reject them. In the process, look out for areas to refactor.

This way, I have full control over the codebase, and I also have great familiarity with it. This actually boost my productivity, but I still have to tell it to refactor it, and question when it blatantly duplicate code.


---

## Recommended reads

- [Critical Thinking in OSINT due to AI](https://www.dutchosintguy.com/post/the-slow-collapse-of-critical-thinking-in-osint-due-to-ai)
    - [[02-Area/web-clippings/The Slow Collapse of Critical Thinking in OSINT due to AI\|The Slow Collapse of Critical Thinking in OSINT due to AI]]
- [The lump of cognition fallacy - Andy Masley](https://andymasley.substack.com/p/the-lump-of-cognition-fallacy)
- [Doodledapp - AI made every test pass. The code was still wrong.](https://doodledapp.com/feed/ai-made-every-test-pass-the-code-was-still-wrong)