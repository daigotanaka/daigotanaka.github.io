## Inside or outside the black box: Talking to broader audience

At FiveStars, we have biweekly all-hands town hall meetings. There, Victor Ho,
the CEO, recently proposed to do a fun activity: 10 people from the company are
randomly picked at each meeting, and they have to briefly explain what they
are working on now to everyone. I understand some people's talk very well while
I don't on the talks of others. What makes one's explanation more comprehensive
than others?

*I will take examples from software engineers' talk in the post, but the big
picture is applicable to any expert-to-non-expert talk.*

It seems the challenge is harder for engineers. The audiences just clap hands,
whistling, basically saying, "Well done, speaking something.", or just paint
a big question mark on their faces. Is it because engineers are socially
awkward? Is it because what they do is too complex for general audience to
understand?

I don't think those are the case. In essence, this is an issue of missed focus
by the speaker. Let me explain.

![black box](https://farm9.staticflickr.com/8643/16116215093_f038197aee_c.jpg)

The objective of software engineering is business automation. We put the tasks,
which are done repeatedly and manually otherwise, into a magic black box so we
can forget about it and get the benefits at scale. When we talk about this
black box, most audience wants to know **what it does** (outside the black box).
But engineers, who created it or about to create, want to shift everyone's
focus immediately on **how it does** (inside the black box).

Somehow we forget that not everyone is informed upfront of what it does. So,
we tend to skip the discussion to how it does. It is impossible to follow a
discussion on a process without knowing its purpose.[^benefits]

Of course, it is often good also to discuss how the black box actually works.
It's wrong to assume non-experts aren't interested in knowing the process.
However, staying at an appropriate level of details seems to be another
challenge.

Engineers build things bottom up, piece by piece. In software, we always want
to the cutting-edge technologies that always come with an exotic name. [^names]
Result?

> "...so, we cache this info at Redis, and...oh, Redis is what we use for
key-value cache store, and..."

> "What do you mean by cash and key value store?"

> ...

We get stuck in describing low level items in detail while the listener don't even
remember why we are talking about this anymore. It's as if we are lost in the
forest while climbing to the summit.

Human brain seems to use symbols to pack a big amount of information such as
complex process. Once the symbol is registered in our brain, we can focus on
high level items to think faster. Think of the case that we often say, "google
it". When everyone uses Google, that makes sense, but we tend to forget that
something familiar to us may be completely new and strange thing to others.

In the above example, we could have just told the audience that it takes time
to calculate the result, so we make computer remember the result at the first
calculation in case we need the same information again in the future. Only
after audience understands that, we can put a short-hand "Redis" on top it to
revisit the idea efficiently in the rest of the discussion.

To summarize, here are my tips:

1. Talk about the purpose (what is it?) and benefits (so what?) first.
2. Keep the description of process (how does it do?) at high level.
3. Convert symbols and shortcuts (i.e. technical jargons) into plain English.

After all, it boils down to the golden rule of public speaking: Know your
audience. I'd add to this golden rule by saying, "Know thyself" (or know your
tendency as an expert). We all should take a moment to think about this rule,
and make our own actionable practices. That way, an expert can also be an
effective communicator, be it at a small group discussion or at a conference
scale talk.

*Report grammatical error [here](https://github.com/daigotanaka/essays/pull/7/files?diff=unified)*

[^benefits]: For audience to pay attention, describing the purpose is often not enough. We need to sell the idea by talking about benefits in the form that the audiences can relate to themselves.
[^names]: I know. Sometimes the names aren't exotic. Just celery and rabbit. It's just they aren't what normal people think.
