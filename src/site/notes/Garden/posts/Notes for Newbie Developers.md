---
{"dg-publish":true,"permalink":"/garden/posts/notes-for-newbie-developers/","tags":["philosophy"],"created":"2021-10-21 08:17","updated":"2026-01-22 14:00"}
---

# Notes for Newbie Developers

Firstly understand [[10-Inbox/The definition of a Software Developer\|The definition of a Software Developer]].

## Work

- [Get your work recognized: write a brag document](https://jvns.ca/blog/brag-documents/)
    - [[02-Area/work/files/Work Accomplishments\|Work Accomplishments]]

## Software development process

- before development
    - design
        - should you use the pre-built frameworks or build from scratch?
            - think about the potential future cases
            - when you are limited by the framework itself, then it might be time to start from scratch
            - if the framework is too complex, might be a good idea to start from scratch
        - consider the component diagram
            - how to separate the concers/responsibility?
            - how to make it modular?
    - detail the workflow
        - detail the main flow
        - prioritise which are main features, which are common cases, which are edge cases
- code
    - utilisation of [[02-Area/programming/tools/git\|git]]
    - utilisation of code formatters, linters
    - utilisation of [[02-Area/programming/General/pre-commit hooks\|pre-commit hooks]]
- after code
    - testing
        - unit testing, functional testing, coverage testing
    - CI/CD
- finally PR, deployment

## On presentation

- talk about impact
    - you created a web ui, but what's the *real* value?
- Address the pain points
    - compare before and after scenarios
- writing docs for users
    - make sure to put across the point what exactly the user need to do
    - get a TL;DR
- demo
    - showcase the flow and use case
    - current features, pending features
    - limitations

## System/project design

- think about the whole flow
- define inputs and outputs to your system
- what are the things that you can add in, in addition to the things you're tasked.
- work out the whole flow first, even if the result is not that great. You can swap in/out components later, important thing is the whole flow.
- what are the tradeoffs

## Client user interaction

- you have to drill down to what client/user really wants (aka, the intent) to avoid [[Garden/knowledge-base/XY problem\|XY problem]]
- not everything user wants has to be in your project
- you have the right to suggest the solution from your perspective, sometimes it will totally eliminate the requirement

## Build a [[10-Inbox/Programmer Profile\|Programmer Profile]] for yourself

- Stackoverflow
- GitHub
- personal blog

News reader such as [[10-Inbox/inoreader\|inoreader]], hacker news to keep up with the news
- important
    - keep abreast
    - know what is feasible

## Learn [[03-Resource/knowledge/Problem Solving\|Problem Solving]]

- [[Hardware and Software fundamentals\|Hardware and Software fundamentals]]
- [[02-Area/web-clippings/Back to Basics\|Back to Basics]]
- [[02-Area/web-clippings/Choose Boring Technology\|Choose Boring Technology]]
- The importance of [[Garden/posts/Note taking for software development\|Note taking for software development]].
- [[02-Area/web-clippings/Most Software Bugs Are Not From Lack of Knowledge\|Most Software Bugs Are Not From Lack of Knowledge]]
- [[02-Area/web-clippings/Modern storage is plenty fast. It is the APIs that are bad.\|Modern storage is plenty fast. It is the APIs that are bad.]]
- [[02-Area/web-clippings/Improve your debugging by asking broad questions\|Improve your debugging by asking broad questions]]
- [[02-Area/programming/General/Debugging Methodology\|Debugging Methodology]]
- [[02-Area/web-clippings/If It Doesn’t Suck, It’s Not Worth Doing\|If It Doesn’t Suck, It’s Not Worth Doing]]
- [[03-Resource/knowledge/Divide and conquer\|Divide and conquer]]

Learn why you shouldn't care too much about "what-ifs": [[02-Area/web-clippings/The mindless tyranny of what if it changes as a software design principle\|The mindless tyranny of what if it changes as a software design principle]]

Learn about [[02-Area/web-clippings/The Security Mindset\|The Security Mindset]], how it can help you write secure software or develop new career paths.

Contribute to opensource
- [[03-Resource/knowledge/Opensource First Timer\|Opensource First Timer]]

Make sure your solution works end to end, before optimising stuff.

## URLs:

- Philosophy
    - [Back to Basics – Joel on Software](https://www.joelonsoftware.com/2001/12/11/back-to-basics/)
    - [Lucas Kostka - Foundational knowledge is worth a hundred tools](https://lucaskostka.com/posts/foundational_knowledge)
    - [The Duct Tape Programmer – Joel on Software](https://www.joelonsoftware.com/2009/09/23/the-duct-tape-programmer/)
    - [Buckblog: Don't Assume It's Difficult until It Is](http://weblog.jamisbuck.org/2016/1/9/dont-assume-its-difficult.html)
    - [The Best Code is No Code At All](https://blog.codinghorror.com/the-best-code-is-no-code-at-all/)
    - [Favouring Tools is Bad Engineering](https://www.samjarman.co.nz/blog/bad-engineering)
    - [Beginner's Mindset: Key to Engineering Expertise - JabPerf Corp](https://www.jabperf.com/think-like-a-beginner-to-become-an-expert-engineer/)
    - [[02-Area/web-clippings/We have used too many levels of abstractions and now the future looks bleak\|We have used too many levels of abstractions and now the future looks bleak]]
- On selection of technology
    - [Choose Boring Technology](https://mcfunley.com/choose-boring-technology)
    - [Choose Boring Technology](https://boringtechnology.club/)
- Debugging
    - [Improve your debugging by asking broad questions](https://buttondown.email/hillelwayne/archive/improve-your-debugging-by-asking-broad-questions/)
    - [A debugging manifesto](https://jvns.ca/blog/2022/12/08/a-debugging-manifesto/)
- Problem solving
    - [Cross-Domain Thinking Drives Insights & Innovation](https://markmcneilly.substack.com/p/cross-domain-thinking-drives-insights)
- Performance optimisation
    - [Horrible Code, Clean Performance](https://johnnysswlab.com/horrible-code-clean-performance/)

## Resources
- [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)
- [Programming Paradigms (Stanford) - YouTube](https://www.youtube.com/watch?v=Ps8jOj7diA0&list=PL9D558D49CA734A02)
- [[03-Resource/knowledge/Spaced Repetition\|Spaced Repetition]]
- [[02-Area/web-clippings/Building a Second Brain The Illustrated Notes\|Building a Second Brain The Illustrated Notes]]
- Hacking
    - [[10-Inbox/pwn.college\|pwn.college]]
- networking
    - [Computer Networking Fundamentals](https://iximiuz.com/en/series/computer-networking-fundamentals/)
- [Explore | 0DE5](https://www.0de5.net/explore)

## Career

Greater impact leads to greater responsibility (bigger title)
[How to Become a 10x Dev: An Essential Guide | HackerNoon](https://hackernoon.com/how-to-become-a-10x-dev-an-essential-guide)