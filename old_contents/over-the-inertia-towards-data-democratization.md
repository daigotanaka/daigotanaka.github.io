## How to overcome organizational inertia towards data democratization

![](https://raw.githubusercontent.com/daigotanaka/essays/master/images/iStock-518597818.jpg)

**Silicon Valley companies are all about data-driven decision making...right?**

It is shocking to me every time my friends at startups show their concerns that
their bosses are not using the insights from their own data to make strategic
decisions.

I am quite happy that whole company, the executives and individual contributors
alike, are increasingly utilizing data every day in order to make decisions at
FiveStars.

How did we get here? What lessons can we share to other companies yet to
overcome the organizational inertia?

*What I am going to write may sound too obvious or not even relevant to those
who already are experts on this topic or working for an elite data company. But
If you happened to experience the same pain my friend shared with me, please
go ahead and read the rest.*

I joined FiveStars as one of the founding engineers over five and half years
ago. FiveStars is now the leader of customer relationship management for small
and mid-size retail stores in the USA. The company raised 105 million dollars
and serving over 10,000 merchants and their 30 million customers.

We now have a **Data Lake** where our entire data, varying from Salesforce to
user events, is **extracted** and poured in. The data in the lake is curated,
cleaned, and **transformed** before **loaded** to the **Data Warehouse**, where
the rest of the employees consumes the clean and well-defined data for daily
analytics with their favorite business intelligence tools.

**But its path to today's data democratization is nothing but a story of an elite
data-centric company.**

Why?

It's partly because there weren't many hosted service six years ago. The
open-source solutions cost too much engineering time to set up.

And here is the irony: Our initial products were selling so well that we
didn't have to worry about defining a clear key performance indicator for our
products.

**We kept postponing the data investment.**

In the summer of 2014, two and half years after I joined, I decided to become a
data scientist at FiveStars. After all, the reason I moved from the world from
geophysics to the small startup was the excitement of analyzing consumer
behavioral data we would be getting through our products. When I joined, we
didn't have a truly scalable product, not to mention the data with a meaningful
volume.

But finally, I could leave the engineering tasks to the true engineers and
go back to my own strength as a Ph.D. researcher who dealt with the variety
of data from medical imaging to reflection seismology!

Since I was a full-stack engineer in the product team, I started to apply
data science to one of the new products I was heavily involved. The concept
was very simple: run an A/B test between the legacy and new tablet product
deployed at our merchant's store front.

Despite the analytical simplicity, the execution was very difficult. You could
think of third-party mobile A/B testing service today. However, because
our experiment was beyond the software and it was involved the tablet
supply-chain, I had to run the random assignment manually.

The data collection process was messy too. We did not have a data lake or data
warehouse. We just had a replica of the production database. I had to join
the tables across the other data sources manually.

**The process was ugly but it paid off!**

It helped our leaders to clearly understand the risk and impact of the new
product. We released it and did so with the greatest confidence we ever had.

My effort inspired who were not very aware of the power of analytics and fueled
the passion to those who always want it to happen. CTO and I started a Slack
channel called #datatrends. The culture of data quickly infected everyone in
the company.

After this project and some more data hackathons with our data pioneers, the
company accelerated the investment to the talent and data infrastructure.
We officially hired an analytics manager who also instrumented the today's
internal analytics ecosystem. I proposed CTO to create a data science position
in the company and I successfully transitioned from the engineering to the
first data scientist.

And thanks to a veteran employee who went the extra mile to teach anybody in
the company how to use SQL, the infrastructure investment is fully utilized
by all functions at all levels.

**This is our version of the birth of a data democracy!**

Without an infrastructure, it is much harder to load the data into one place to
run analysis to demonstrate the power of data. But without demonstrating the
value, it is not possible to persuade your CEO of the investment towards
data democracy.

*From the experience I wrote above, I truly empathize the pain you might have.
And I would like to help by designing and executing a data science project that
helps to demonstrate the value and overcome the inertia towards a data
democratization in your company. I always welcome and appreciate if you
[connect me on LinkedIn](https://linkedin.com/in/daigotanaka) or
[send a message](mailto:daigo@anelen.co).*
