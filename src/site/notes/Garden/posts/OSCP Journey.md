---
{"dg-publish":true,"permalink":"/garden/posts/oscp-journey/","created":"2026-01-05T22:08:16+08:00","updated":"2026-01-05 22:08"}
---

# OSCP Journey - 2020
tags: #hacking


![OSCP Journey-1.png](/img/user/03-Resource/Attachments/OSCP%20Journey-1.png)

Just like those who have done the OSCP exam before me, I felt the urge to write my own account of how I came to acquire the certification.

For those of you who don't know what OSCP is, it stands for Offensive Security Certified Professional, and this is an entry-level certification for penetration testers. I have had a great opportunity to acquire this certification from my company.

To get this certification, you have to go through a course called Penetration Testing With Kali Linux (PWK) course, practice through the PWK labs and go for the 48-hour online examination when you feel ready.

I would like to talk about how I went about preparing for the OSCP and how I managed to pass it on the first try, and may this help you in your journey as well.

## Background

I have been doing cybersecurity-related things + programming since I was around 14, 15 years old (which is about 10 years back). Since then, I have learned things like web penetration testing, for instance, how to do SQL injection, RFI, LFI, XSS, the works. I also learned C, Python, Java, JavaScript, and PHP along the way.

But my concentrated effort in cybersecurity didn't begin until 4, 5 years ago, when I first started university. This does not mean that my university degree is in Cyber Security or Information Security. It's just that I have more free time on my hand to learn more about these things on my own.

I have been a member of HackTheBox (HTB) lab since 2017, and I have compromised around 40 machines, practicing here and there. And I have always been keeping abreast with the things happening in the cybersecurity world.

About two months before starting the PWK course, I purchased VIP on HTB and TryHackMe (THM) labs and started practicing. THM has an OSCP path, where they listed a set of machines with progressional difficulties.

I was hesitant to start the PWK course, as I do not feel quite ready to take up this challenge. This is due to me being unable to compare with someone else and get a baseline of my skills. I have seen too many horror stories of people failing the OSCP exam on the first attempt or fail a few times.

Finally, I decided to start the course and registered for it.

## PWK course

I registered for the PWK course with 30 days of lab access. I am determined to compromise all the PWK lab machines, 66 of them in total during these 30 days.

I was also working full time when I was going through the labs, so I only have the time in the evenings, when I get home from work until 12, or 1, 2 AM, as much as I can muster without falling asleep. And weekends are the days that I would spend the majority of the time going through the labs.
As soon as I got the PWK course materials (consists of videos and an 850+ pages PDF), I glanced through briefly, for about a day or two. Then I dived right into the lab.

I was aware that if I worked through the exercises and write a report, I could get 5 bonus points, which could save me on the exam. But I thought if I needed a mere 5 points to pass the exam, then I am not good enough to pass the exam. At that time, it feels like lab time is more important than working through the exercises since the lab time starts as soon as I received the course materials.

In hindsight, working through the PDF, videos, and exercises could have saved me a lot of frustrations and taught me a lot of things if I had gone through them first.

So I was compromising a few machines every weekday, but sometimes I could only compromise one machine, sometimes I could get up to 3 machines. Then during the weekends, I work in the lab for more than 8 hours a day and usually compromise about 5 or 6 machines per day.

While working through the lab, I also discussed with other people on the [InfoSec Prep discord server](https://discord.com/invite/mEtEFhp)

The community has been super helpful whenever I was stuck on a machine. Of course, they will not tell you the solutions, they will only tell you the hints, and whether you are heading in the right direction or not.

When the lab time is about to expire (less than a few days left), I had about 50+ machines compromised in the lab. I still had ~16 machines to go for, and not enough time. Then I decided to extend the lab for another 30 days to ensure I have done all the necessary preparations for the exam.

So I used the 30 days extension to complete what's left of the lab machines and spent about 2 weeks working through the exercises and compiled my lab report. The lab report is about 220+ pages long. The exercises are quite tedious, and I would rather choose to re-compromise all the lab machines than doing the exercises again.

I managed to secure an exam date around my now extended lab end time. The time selection was tough, if I get too close to the end date of the lab, I feel I might not be too ready, and if I get it too far from the end date, then I thought I might lose the momentum, like where else am I going to get the chance to maintain the pace of hacking a few machines every day. The exam date I got was about two weeks after my lab ends.

After I have done all the lab machines and completed my report, I started working on TryHackMe labs. Initially, before starting PWK, I have gone through the THM OSCP path. So this time, I am more focusing on two lab machines in THM, namely [Buffer Overflow Prep](https://tryhackme.com/room/bufferoverflowprep) and [Windows Privesc](https://tryhackme.com/room/windowsprivescarena)

I am very familiar with the Linux privilege escalation, as I have been using Linux since I started learning cybersecurity and programming, but Windows privilege escalation, on the other hand, feels quite foreign to me. So I also registered for Tib3rius's [Windows privilege escalation course](https://www.udemy.com/course/windows-privilege-escalation/) and created all the cheatsheets from that course.

I started reading all the OSCP exam experience I can find on the internet, by people who have passed the exam.  When going through the buffer overflow prep THM room, I created a buffer overflow python program to help me save time in the exam, as well as for general testing usage. It is available at [the-c0d3r/buffer-overflow](https://github.com/the-c0d3r/buffer-overflow)

Using the buffer overflow program, I was able to spawn a reverse shell to my Kali Linux machine within 10 minutes for most of the buffer overflow challenges from the THM room.

## Exam Preparations

- Food: quick and easy meals, coffee, red bulls, snacks
- Tmux sessions: I created 5 tmux sessions, 5 folders for each exam machine, and the notes template. I usually just use markdown type note, since I can easily edit it in terminal using vim.

- Things that I used in the exam, that I feel have helped me
    - [Autorecon](https://github.com/Tib3rius/AutoRecon): the very famous must-have tool for the exam
    - tmux: this helped me boost up my productivity while working in multiple terminal sessions, tabs, windows, etc.
    - zsh: zsh shell is bash on steroids. I used [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) framework.
    - fzf: [fuzzy finder](https://github.com/junegunn/fzf) (zsh plugin version) for searching through the terminal history. This saved me from having to memorize a bunch of commands, or the need to have cheatsheets. I just type the command correctly for one time, and all the subsequent times, I can just "Ctrl+R" and search for the keyword and press enter. This one is *extremely* helpful.
    - neovim: this is vim but better. Knowing how to navigate in vim, would help save time. Most of the time when you are hacking, you would be working in a terminal, dealing with texts. Using tmux and vim, you never have to leave your terminal to write down notes.
    - exam template: I used [whoisflynn's template](https://github.com/whoisflynn/OSCP-Exam-Report-Template)

<br />

![OSCP Journey-2.png](/img/user/03-Resource/Attachments/OSCP%20Journey-2.png)
<p align=center>figure 1: neovim in action</p>

![OSCP Journey-3.png](/img/user/03-Resource/Attachments/OSCP%20Journey-3.png)
<p align=center>figure 2: fzf in action</p>

## Exam
The pre-exam verification steps took me like an hour. I started 30 mins before (8:30 AM) the exam as advised by Offsec, and only completed verification and exam connection pack around 1 hour (9:30 AM).

### Buffer overflow + 10 points
I started AutoRecon scanning for the 10 point machine, and get started with the buffer overflow machine. Using the script I created beforehand, I was able to spawn a shell on my windows testing machine within 10 minutes, and I spent another 10 minutes creating the PoC and taking screenshots.

So at this time, the 10 point machine scanning is almost done. I stopped it and started the scan for 25 point machine. And I managed to root 10 point machine in about 10 minutes, as it was very very straightforward.

### 25 points
Then I spent another roughly 4 hours on the 25 point machine rooting it. This machine was not straight forward at all, it requires so many trial and errors. But I did not face the rabbit holes that many people always complained about. I just keep trying different things and little by little, I managed to slip through and widen the crack big enough to execute whatever I want. As I was working on the 25 point machine, I also started scanning for the remaining two 20 point machine.

### 20 points
I had to take a break during the 25 points to have a quick 30-minute lunch. Then I started working on the first 20 point machine. This one is also quite straightforward for a foothold, but the privilege escalation took slightly longer, as I went the wrong way. I assumed things based on the experiences I had in the lab. And that was dead wrong. Fortunately, I was able to figure out a way to privilege escalate. The whole process took me about 2 hours.

### Current points
By now, I have enough points to pass the exam.
25 (buffer overflow) + 10 + 25 + 20 = 80 points
And I went through the trouble of completing the lab report, which will add up to a total of 85 points. As I went through each machine, I quickly documented down all the steps I took and made sure that I have screenshots for the steps I took to reach root access.

At this point (~7-8 hours into the exam), I could have just go to bed and take my sweet time with the report. But I knew better than to just be "good enough". I took turns doing the last 20 point machine and compiling the exam report. But even after putting the remaining 12 hours+ into the 20 point machine, the machine didn't budge at all. It feels like all hope is lost, I was devastated. I was beginning to feel quite uncomfortable now that I have spent 24 hours straight hacking. I looked outside my window, the dawn is approaching, I am running out of time and ideas, I decided to accept the fact that I couldn't get the last one.

So I double-triple checked all my screenshots and reports, and submitted my exam report, and asked my proctor to end the exam about half an hour earlier.


## PWK Tips
- Do not just try to complete the machines as fast as possible. Don't be too focused on the number of machines you compromised. I have known some people who only did ~10+ machines and still passed on the first try. Completing all 66 machines is not a guarantee to pass the exam, it only increases the likelihood.
- Look at what each machine is trying to teach you.
- Take rigorous notes, and learn to take screenshots from the start.
- Extract all the techniques you used, enumerations you did, into a methodology. A mindmap would help in this case. Focus on building the methodology, to know what to do in each case.
- Go through the PDF and Videos
- Don't look at hints, unless you have been stuck for a few days on the same machine. I had to rely on a lot of hints to complete the machines as fast as possible, due to my limited lab time. But that was a very bad idea.

## Exam Tips
- Create a schedule beforehand
- Practice Buffer overflow to the point when it becomes your muscle memory. This is the easiest 25 points.
- Do lots of enumeration with tools such as Autorecon.
- Take screenshots and notes, as soon as you compromised one machine, double back and make sure you got all screenshots for each step.
- Take a nap from time to time. Prepare ready-made meals, coffee, redbulls.
- If there's something that is running, think "maybe because it has a purpose".
- Prepare scans of your ID just in case your webcam cannot focus.
- Create one tmux session for each machine with unlimited scrollback history.
- Use one drive to write your exam report. One drive has free online Microsoft word editor
- Read through the exam guide, and prepare a checklist for each step of report submission.
- Email offsec and clarify about anything (such as Metasploit usage) before the exam, rather than waiting for their reply during the exam.


Good luck to all the OSCP students, and remember to *try harder*.
