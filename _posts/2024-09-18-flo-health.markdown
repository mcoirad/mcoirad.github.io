---
title: "Flo Health: How to Badly (and Unethically) Use the Facebook SDK"
layout: post
date: 2024-09-18 02:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- tech ethics
- research
- startups
star: true
category: blog
projects: false
author: dario
description: "A data ethics analysis of Flo Health, the Facebook SDK, reproductive health privacy, cloud infrastructure, and developer responsibility."
---

*Here is an article originally written for my Medium blog, but posted here as part of my explorations into technology ethics.*

## Flo Health: How to Badly (and Unethically) Use the Facebook SDK

The cloud computing revolution has made it easier than ever for developers to abstract the code and software they create from the physical hardware used to store and process data. As the specifics of this functionality are usually not explicitly exposed to application users, users must rely on the terms of service and policies they agree to when they begin use of an app. Flo Health, a period and ovulation tracking app for iOS and Android smartphones, came to national attention in 2019 as a result of a Wall Street Journal (WSJ) investigation that revealed it was sending health related data to Facebook.[^1] This was in contravention to its stated privacy policy that such data would never be sent to "third party vendors."[^2] The WSJ investigation highlighted that a Facebook software development kit (SDK), originally created to allow developers to track user actions in mobile applications, was used to send Facebook data "when a user was having her period" or had "an intention to get pregnant."[^3] Once the data entered Facebook's platform it was subject to Facebook's terms that allowed for its aggregation and use by Facebook to personalize ads and content, among other general uses.

### Tools of the trade
To develop its application, Flo Health used a variety of tools and services such as databases and compute capacity, some of which are typically run on server infrastructure owned and operated by third party cloud providers such as AWS.[^4] While this is a commonplace practice among technology companies, Flo Health would have violated its own privacy policy, as stated in 2018, if it had used this third-party infrastructure to store any data related to "marked cycles, pregnancy, symptoms."[^5] Despite this potential breach of the agreed terms, the data of users was still likely to be stored in secure systems. Flo Health appears to have utilized Amazon Web Services (AWS), which is among the most established cloud providers. These providers boast highly robust data protection and privacy programs, earning the trust of even the federal government.[^6] However, language covering these typical types of uses, and links to Amazon's own privacy policies, were only added to Flo Health's privacy policy in 2020.[^7]

The WSJ investigation predominantly concentrated on the fact that Flo Health users consented to Flo Health's data collection under an agreement of trust that assured it would only be used for the purposes of the app. Users on a computing platform like a smartphone typically lack administrative access to their own operating systems, making this trust especially important.[^8] The crux of the resultant lawsuits against Flo Health was that the company made the private health data potentially available to Facebook, Google, and other third party vendors for a variety of secondary uses unrelated to the purpose of the app.[^9] These violations of privacy and trust made Flo Health's users vulnerable to multiple types of harm.

### The potential harms caused

Both Facebook and Google denied that they used the data they received from Flo Health for the purposes of ads personalization or other data science products, although lawsuits continue to proceed over the matter.[^10] But by passing the data of users to both of these platforms, Flo Health opened their users' personal information to a veritable black-box of machine-learning models and other products.[^11] Even if the only resultant effect was increased ads personalization, this could still result in inadvertent consequences for some users if the ads they received were viewed by coworkers or employers. Additionally, once in the platform, data could potentially be extracted by other developers for any number of other applications, including ones that could result in negative impacts or judgements of Flo Health users. Despite Flo Health's claims that it had "depersonalized" the data it was sending to Facebook, the WSJ investigation revealed that Flo Health's app had passed on a unique identifier for the data that could have assisted in any data aggregation efforts.[^12]

The state of insecurity created by the privacy breach can also have resultant consequences on future decisions of individuals. Individuals with low trust in the confidentiality of health care systems are more likely to withhold important health information from their providers.[^13] In addition, when certain forms of reproductive health care are criminalized, unintended information dissemination can result in direct negative legal consequences for the user.[^14] Finally, in a legal environment where one has no final right to delete or remove oneself or their data from a platform, and where tech companies can routinely flout their own terms of service with little threat to their business models - one cannot predict how data can be used in the future. Official statements from a Facebook spokesperson that data is not being used for certain purposes means little from a company that has been known to violate its own terms of service in the past.[^15]

### What should be learned

On the part of Flo Health, it is clear that the company's application developers need to be more aware of and respect the terms that the users of its services agreed to. Whether the original problem stemmed from ignorance, poor internal communication, or malfeasance is uncertain. However, what is known is that after the repeal of Roe vs. Wade in the United States, Flo Health created the option of using the app in "Anonymous Mode," allowing users to utilize the app without entering any personal information.[^16] **While a welcome change, the release of this feature can also be interpreted as acceptance on the part of Flo Health that it still cannot guarantee security and privacy for the data its users choose to submit.**



On the side of Facebook, part of the problem is that documentation for developers on the Facebook platform often lags behind the release of features and functionality by months to years. Sometimes it is even incorrect.[^17] As an example, custom app events are minimally documented in Facebook's SDK documentation - and no information is given on what types of events or data are appropriate to be sent through the service.[^18] If the developer facing documentation could be improved, correct uses of the SDKs would be more clear and there would be less need for developers to coordinate with legal departments to understand and comply with terms of service.[^19] **However, it is probably not in Facebook's interests to give clear warnings as it may push developers to use platforms that don't require developers to hand over broad rights to the data they send.**


[^1]: Sam Schechner, "You Give Apps Sensitive Personal Information. Then They Tell Facebook.," The Wall Street Journal, June 25, 2022, https://www.wsj.com/articles/you-give-apps-sensitive-personal-information-then-they-tell-facebook-11550851636.
[^2]: Ibid.
[^3]: Ibid.
[^4]: "Flo Health Tech Stack - Flo Health, Inc.," StackShare, accessed November 21, 2023, https://stackshare.io/flo-health-inc/flo-health-tech-stack.
[^5]: "August 6, 2018 - Flo Privacy Policy (Archived)," Flo Health, August 6, 2018, https://flo.health/privacy-policy-archived/aug-6-2018.
[^6]: "Data Protection & Privacy at AWS," Amazon, accessed November 21, 2023, https://aws.amazon.com/compliance/data-protection.
[^7]: "Privacy Policy: Archived Versions," Flo.health - #1 mobile product for women's health, January 1, 2020, https://flo.health/privacy-policy-archived/january-1-2020. However, the policy that Flo Health links to doesn't cover the uses Flo Health describes in its own policy, indicating it is either out of date, or was never correct in the first place.
[^8]: Chris Hoffman, "What's the Difference between Jailbreaking, Rooting, and Unlocking?," HowToGeek, February 2, 2016, https://www.howtogeek.com/135663/htg-explains-whats-the-difference-between-jailbreaking-rooting-and-unlocking/.
[^9]: Schechner, "You Give Apps Sensitive Information"
[^10]: Jon Styf, "Google Seeks Dismissal of Flo App Privacy Class Action," Top Class Actions, October 13, 2023, https://topclassactions.com/lawsuit-settlements/consumer-products/mobile-apps/google-seeks-dismissal-of-flo-app-privacy-class-action/.
[^11]: Jessie G Taft, "Facebook and Google Are the New Data Brokers," DLI @ Cornell Tech, January 6, 2021, https://www.dli.tech.cornell.edu/post/facebook-and-google-are-the-new-data-brokers.
[^12]: Schechner, "You Give Apps Sensitive Information." Passing a unique identifier allows Facebook to more easily join data between datasets. Without the identifier Facebook must rely on a "fuzzy" matching process that will typically result in a lower and less accurate match rate.
[^13]: Bradley E Iott, Celeste Campos-Castillo, and Denise L Anthony, "Trust and Privacy: How Patient Trust in Providers Is Related to Privacy Behaviors and Attitudes," AMIA - Annual Symposium proceedings. AMIA Symposium, March 4, 2020, https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7153104/.
[^14]: Jennifer Korn and Clare Duffy, "Search Histories, Location Data, Text Messages: How Personal Data Could Be Used to Enforce Anti-Abortion Laws | CNN Business," CNN, June 24, 2022, https://www.cnn.com/2022/06/24/tech/abortion-laws-data-privacy/index.html.
[^15]: Robinson Meyer, "Everything We Know about Facebook's Secret Mood-Manipulation Experiment," The Atlantic, August 5, 2021, https://www.theatlantic.com/technology/archive/2014/06/everything-we-know-about-facebooks-secret-mood-manipulation-experiment/373648/.
[^16]: Nicole Wetsman and Corin Faife, "Flo Period Tracker Launches 'anonymous Mode' to Fight Abortion Privacy Concerns," The Verge, September 14, 2022, https://www.theverge.com/2022/9/14/23351957/flo-period-tracker-privacy-anonymous-mode.
[^17]: Developer support home search - META for developers, accessed November 21, 2023, https://developers.facebook.com/support/search/?query_string=documentation+incorrect.
[^18]: "App Events API for Marketing API," Meta Marketing API - Documentation - Meta for Developers, accessed November 21, 2023, https://developers.facebook.com/docs/marketing-api/app-event-api/. Additionally, App Events are no longer the standard for tracking custom actions through the SDK, Facebook officially recommends that new apps use the Conversions API which also suffers from similar documentation issues.
[^19]: This does not absolve developers of responsibility in either case. The issue remains that while many developers today are geared to asking questions such as "Is this SDK open source, and licensed for commercial use?," they may be less likely to ask "Is this SDK license compatible with the license our app's users are signing?"
