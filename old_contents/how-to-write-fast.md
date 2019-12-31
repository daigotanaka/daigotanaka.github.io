## How to write fast

![](https://github.com/daigotanaka/essays/raw/master/images/ergodox_keyboard.jpg)

As a data scientist serving business decision makers, I write a lot of reports.
In other occasions, I write technical specifications for application
developers who implement
[the user event instrumentation for product analytics](engineers://www.linkedin.com/pulse/product-analytics-101-your-app-leaking-funnel-daigo-tanaka-ph-d-).
You also have been seeing me posting articles like this on LinkedIn every
Tuesday. I write a lot. How do I write so fast?

**There are a lot of articles focusing on how to overcome writer's block. They are helpful. But
today, I want to focus on the system I have, not the tips when you are stuck.**

My writing system is designed around one question:

### Can you make your hands work like your mouth?

You may have clicked this article because of the insane looking
keyboard in the headline image. (Go ahead and scroll up to look it again.) This
is actually my keyboard and there is only one keyboard exactly like this in the
world. I know it because I built it! It's based on an ergonomic keyboard called
Ergodox. I added custom the key caps that are made of zinc. It's not only shiny,
but it gives the just right amount of the haptic feedback for me when I type. Did you
notice the keys don't have any label? I don't need them because the keyboard
layout is fully customizable. Drawbacks? No one else can use it. It's just for me.

Why am I bragging about my keyboard that I built by soldering up more than 100
tiny diodes? **My point is that you need to remove any friction between the
thoughts in your brain and the writing.** I take this to an extreme as to build
my own keyboard.

Let's go on to find out what other tools I use.

### Stop using a word processor for drafting. Seriously!

Microsoft Word, Google Document, and anything else, a word processor
is a terrible place to dump your thought into writing. After writing a
paragraph, a sentence, or even a word, you are compelled to switch your task
to tweak the layout of the document. You think you are still working on the
document, but you are doing a very different task (styling) than putting your
thought into writing. This task switching makes you forget about what you were
going to write next.

I use a simple text editor to get the first draft done. When I do it, I do only one thing:
Just dump my thought into writing. Nothing else. I am not a native English
speaker, so I have to work a lot to correct the grammar of my writings (and it
will never be perfect). But I never care about the grammar when I draft because
that would also cause a task switch.

### Never move your hands off from the home position

How well can you use the keyboard shortcuts? Undo, redo, save, find and
replace...  I find moving my hand off the keyboard to look for the computer mouse is a
wasteful motion interrupting my brain-dump process. Invest some time to learn
keyboard shortcuts. Also, I suggest picking a text editor that let you do most
things with keystrokes without touching the mouse. If you are a coder like me, you
may know vi or vi inspired editors. I code in vi and also love to write articles
and reports in vi because not only I avoid touching the mouse, but also even
don't have to leave my hands off the home position. When you move the cursor,
you usually use a mouse or use arrow keys. Such hand motion takes your hand off
from the home position on the keyboard.
vi let you do everything with the keys my fingers can reach without moving my
wrist. I can even do spelling corrections and word wrapping just by a few
keystrokes. It helps me a lot to focus. I don't recommend vi for non-programmers
because of the high learning curve. But you get my point: Learn to do things
without moving your wrists.

### Semantics not decoration

I wrote that switching tasks for styling of the documents such as changing
fonts and colors interrupts your brain dump process. But I didn't mean to
discourage you from organizing your thoughts and writings by creating headings
and bullet points. Those are helpful in clearly re-organizing your raw brain
dumps. But again, if you use a word processor for that, your hand moves back
and forth between the keyboard and the mouse. Instead, I use markdown. Markdown
is a set of rules in writing that brings organization in the simple text
document without clutter. For example,

The headings are expressed as

```
# The biggest heading

## The 2nd biggest heading

### The 3rd biggest heading
```

You can do bullets and numbered lists:

```
- Item 1
- Item 2

1. Numbered item 1
2. Numbered item 2
```

```
This **emphasizes** words (rendered bold).
This is for *special term* and etc (rendered italicized).
```

And also on. You can even write tables and equations. Those are all expressed
with the simple and intuitive use of symbols. The markdown system visually
conveys the document organization with a simple text format, and the text can be
automatically converted to HTML if the file is opened by Chrome web browser with
[Markdown Preview Plus extension](https://atom.io/packages/markdown-preview-plus)
for example. You can copy and paste the HTML rendered page into Word or Google
Document with those structures preserved.


![](https://github.com/daigotanaka/essays/raw/master/images/markdown-preview-plus.png)
(Image courtesy of Markdown Preview Plus)

### This is just part of my system

Obviously, not every tool I introduced above is for you. But I hope you get my
point. I feel our mouth is wired directly connected to our mind. A lot of things
interrupt the signal between our brain and hands when it comes to
writing especially when we are using a computer. In my experience, anything
distracts me from dumping my thought into writing destroys the train of
thought badly. **Let your fingers simply record your thoughts just as easy as
you use your mouth to talk. That requires careful choice of the tools.**

Of course, we need a story to tell in our mind before we can start writing.
That is based on what we read, what we do, and how we reorganize the learning
from the activities. I also have a system of efficiently compiling the learning
into a story to share. But I save that for another article in the future.

Daigo Tanaka, Ph.D. is a founder of Anelen Co., LLC, a boutique consulting firm
focused on Data Science. He introduced an agile start-up methodology to a pre-Stage
A startup and helped to raise the total of $105MM. If you need a consultation on
the topic of a startup operation and data science, please don't hesitate to
[connect him on LinkedIn](https://linkedin.com/in/daigotanaka) or
[send a message](mailto:daigo@anelen.co).
