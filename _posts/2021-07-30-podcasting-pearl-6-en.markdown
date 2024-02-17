---
title: 'Podcasting Pearl #6: A Conversation with Ronny Kohavi.'
date: 2021-07-30 02:02:36 Z
categories:
- podcast
layout: post
lang: en
ref: pearl6
---

Online Experimentation: on trust, best practices and moving the needle.
<iframe src="https://open.spotify.com/embed/episode/4d3Prnlc0VIf6dQSdWdGmb" width="100%" style="max-width:660px" height="152" frameBorder="0" allowtransparency="true" allow="encrypted-media"></iframe>

<iframe src="https://embed.podcasts.apple.com/us/podcast/conversation-ronny-kohavi-ex-airbnb-microsoft-amazon/id1574474152?i=1000527341014&amp;itsct=podcast_box_player&amp;itscg=30200&amp;ls=1&amp;theme=light" height="175px" frameborder="0" sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="autoplay *; encrypted-media *;" style="width: 100%; max-width: 660px; overflow: hidden; border-radius: 10px; background: transparent;"></iframe>

**Key points:** 
- Most idea fails
- Trust is key

**Best practices:**
- Don’t build the feature: tweak small stuff.
- Don't waste time on hard implementation details. Focus on the part easy to implement, not edge cases. 
- OEC: overal evaluation criterion: agree in advance on what to optimize. Optimize a short-term metric that will give results on the long term.  
Eg: Microsoft wants to have a better market share of search engines. They don't **maximize** the number of queries: a bad search engine easily augment the  number of queries. They optimise the number of sessions. They **minimize** the queries per sessions.
- Ramp ups. Integrate experimentation in the development process: every deploymenyt is measured. 
- When getting mature, invest in a platform that will automate the analysis. (slice and dice for discoveries, etc...)
- Use variance reduction techniques to improve statistical power.  
More sensitivity with less users --> more testing power.
- Test apps: include sleeping tests in release, activate them gradually.
- Check the countervailing metric. Look at the "contrary" of what is optimized.  
Eg: transaction rate --> cancellation/refund rate.  
Eg: newsletter subscription. If it’s too easy to subscribe, population is less qualified.
- Don't analyse before checking for [SRM](https://exp-platform.com/Documents/2019_KDDFabijanGupchupFuptaOmhoverVermeerDmitriev.pdf): sample ratio mismatch.  
When you have an SRM, you probably lose a population with a unique aspect. This means the test will be biased. Results will not be trustworthy. 

**Data challenges**: 
- Detect bot, scrapers and frauds.   
Eg: High stakes ads: some companies intentionally click on competitors ads to exhaust their money.
- Remove 5% of points that have low confidence: this will reduce variance.
- Measuring real valued metrics is hard. Variance can be high.  
Eg: average cart of 30$, but sometimes cart of 3000$. Cap values: `max(revenue, CAP_VALUE)`.
- Being able to run a lot of experiments is key: this is the iterative capital.  


**Quotes**:
- "It's easy to get data, it's hard to get data you can trust."
- "The product team provides ideas. The experimentation team provides testing power."
- "It’s not about shipping it’s about moving the metric."
Nobody wants a feature removed 2 years after its release.
- "Any figures that looks interesting is usually the result of an error" - [Twyman's law](https://en.wikipedia.org/wiki/Twyman%27s_law) 

**State of the field**:
- analysis and maths: well understood
- data collection: still very difficult  

**About innovation**:  
No dichotomy of optimizations / innovation.
Small, iterative optimizations can lead to breakthroughs.  
Metaphor of water: you make it cool gradually, measuring every degrees: it's only at 0°C that something very different happens.  
Example of Amazon: was trying to optimize the purchase flow --> invented the one click purchase.

Example of Google:
[40 shades of blue](https://www.theguardian.com/technology/2014/feb/05/why-google-engineers-designers) is sometimes considered pushing AB testing too far.    
Ronny does not agree. Some guys tweaking colors realised it was important.
As engineers, they then put massive test resources to understand and learn what color was the best for Google's business.  
Small changes: independent color optimization. Big change: massive color testing, and change for all websites. Learning about color neuroscience. 

**Learning from tests:**  
When a test fail, the major question is: *do you pass and move on, or do you iterate ?*.  
Retrospective and meta-learning is important in experimentation.
Focus on what happened and why. 

**Resources:**
Kdd 2019 reasons for srm microsoft
