## Product analytics 101: Your app as a leaking funnel

![leaking bucket](https://github.com/daigotanaka/essays/raw/master/images/leaking-bucket.png)

As a product manager (PM) of web or mobile application, you want to ensure the
application design offers the user experience (UX) that is not confusing and
pleasant to use. A successful application helps your company's key performance
indicators to go up. But how do you know if it is working in the field?

In the design phase, you may rely on UX designer's knowledge and skills. During
the internal test, you can count on your trained eye as a product person and
your coworkers as test users. But the real test always takes place after the app
is released.

**Let's be honest and ask ourselves. Are we following up to measure the
post-release success objectively?**

If not, I suggest you to seriously think about taking action now.

The key is to identify:

- Business goals
- Product goals
- User goals

And record key user events It's not too difficult. Let's take an example of a
travel booking app.

### Travel booking example

I sometimes use Kayak to book flights. One of their appealing feature
is to forecast the ticket prices. A notification is sent to the user to suggest
when to buy. (I am making up the details of how it works from here. It is
probably different from the reality)

Suppose the alert comes in as a notification on my phone if I had installed
the app beforehand. By tapping the notification, the app opens and I will
go through a few screens like this before I buy the ticket:

![kayak](https://github.com/daigotanaka/essays/raw/master/images/kayak.png)

Did you notice that on the first screen from the left, it says, "Our advice
Buy" with an icon to indicate that the price will go up. Let's pretent that you
are the PM and wants to test if such advice will drive the most important
**business goal**: the sales.

In fact, you even ran an
[A/B test](https://www.linkedin.com/pulse/tale-two-universes-experiments-business-daigo-tanaka-ph-d-)
to see if "Our advice Buy" increases the number of sale closed, which is your
**product goal**. The test came in positive, but the increase of sales was not
as much as you had expected. What is going on?

Assuming there are more users persuaded to buy the ticket by the recommendation
than the actual sales, you imagine several scenarios happening among users who
entered the booking process in the app.

The user may have:

1. Been just playing with your app without a buying intention
2. Changed the mind
3. Got interrupted by something outside the app
4. Got confused by the user interface
5. Got frustrated and given up the process
...

Certainly there is a work for your team to improve the UX if the 4th and 5th
are the big reasons. But you don't have a good visibility here. The distance is
great in the observation between what you controlled (Whether the user is
presented with "Our advice Buy") and the outcome (sales closed or not). There
are many steps involved in between and you are not observing them.

**Imagine the fear and frustration of driving a car while you have to keep eyes
closed for 5 seconds for every 5 seconds! That is your situation.**

### What is instrumentation?

To solve this problem, your application needs to *"instrument"* user events.
In computer programming, "instrumentation refers to an ability to monitor or
measure the level of a product's performance, to diagnose errors and to write
trace information.", according to [Wikipedia](http://bit.ly/2u1KFXk).

In this context, you want to log how the user interacts with the application.
Where she tapped in the screen, on what page she closed the app, and etc. By
carefully analyzing how the user interacted with your app, you may be able to
infer the confusion and frustration that prevented user from achieving the
**user goal**: Booking the flight.

### Don't record everything!

In the era of cheap storage and faster internet, you may be attracted by the
idea of recording every interaction. Literally every single tap and swipe.
**Don't do that.** The reasons are:

1. **Unclear definition:** You may end up hundreds of user event whose
   definition is not documented at all and you will never know if you are using
   the information correctly.
2. **Doesn't answer the question:** If an instrumentation is not driven by a
   product question, it is very likely you need to redo the instrumentation
   once you have the actual question. The meaning of the event could change
   dramatically just by moving the logging code before or after a logical block.
   It is most likely do not match exactly what you want once you come up with
   a specific question.
3. **Cost**: If you are using a service like [Mixpanel](https://mixpanel.com),
   it will dramatically increase the bill while you are not looking back 99% of
   the data. It's a waste of money.

You need to select the user events wisely.

### How to select key user events?

The easiest way to select the key user events is to create a "funnel" towards
achieving of the **user goal**. In flight booking app example, it would look
like this:

![leaking funnel](https://github.com/daigotanaka/essays/raw/master/images/user-goal-funnel.png)

At the beginning of the booking process, the number of users interacting with
the app are much higher than the number of users completing the user goal of
booking flight (or sales). So the top is wider than the bottom, therefore the
funnel analogy. Just notice that the funnel is a leaking funnel. For every
page in the app, latency, and expected user input, there is a risk of user
falling out of the process.

Draw the funnel of your app and identify:

1. The user inputs that carry the user to the next level in the funnel.
2. The user inputs such as cancel button and timeout that indicate why
   the user is falling out of the funnel.

Those will become the minimal set of user events for you to start understanding
what is going on.

### Adding attributes to the events

After you have the user event logs from many different users, you will need to
tie the one step to the next in the funnel for each unique transaction. Your
user event comes into the server like this:

|**Time**|**Event name**|**Distinct ID**|**Experiment Group**|**...**|
|:----|:----|:----|:----|:----|
| 2017-07-07 17:34:05 0001 | List displayed | 1234 | Treatment | ... |
| 2017-07-07 17:34:05 0011 | Booked flight | 2235 | Control | ... |
| 2017-07-07 17:34:06 0100 | Select flight | 1234 | Treatment | ... |
| 2017-07-07 17:34:05 0011 | List displayed | 2155 | Treatment | ... |
| 2017-07-07 17:34:07 0020 | Enter traveler | 1234 | Treatment | ... |

Here, you need to pick the Distinct ID of 1234 and thread them in order to
identify where the user is dropping off from the funnel or completing the
user goal. It would also be helpful if you include additional attributes
such as Experiment Group in the example to group the user of the interest
to see there is a difference in the performance.

### Get the insight

In the flight booking app example, you may find the following things by
analyzing the user event:

1. The number of the users who start typing traveller information is higher
   among the treatment group in which they are presented with "Our advice Buy".
   The effect seems to be much bigger than what you first observed in the
   final number of sales during the original A/B test.
2. Most users actually finish typing in the traveller, payment information
   and click Book. But they are falling off at the confirmation step. They seems
   to be forgetting to click "Book" as the final step of the sales.

By fixing the presentation issue of the confirmation page, you fix the issue
of users forgetting to click "Book" button. In other scenario, there may be
a bug in the fallback logic when the network is interrupted during the
transaction call.

After resolving the issue, the user presented with "Our advice Buy" started
to perform much better in terms of the number of sales. The team is celebrating
and you are glad that **you now have a good visibility of the user interaction
that gives you the actionable insights.**

Instrumentation of user event is a powerful product analytics tool. There are
many practical issues and techniques involved to implement effectively. If you
need an expert help, please start feeling good by
[connecting me on LinkedIn](https://linkedin.com/in/daigotanaka) or
[sending a message](mailto:daigo@anelen.co).

Daigo Tanaka is founder and principal data scientist at Anelen Co., LLC.
