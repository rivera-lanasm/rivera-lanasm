---
title: "Sociotechnical Systems in Fairness & Accountability Research"
date: 2020-12-25T20:16:30-05:00
subtitle: "Exploring the concept applied to social media platform security"
tags: [sociotechnical systems, social media, disinformation, content moderation]
draft: true
---

In contemporary discussions about fairness and accountability in the implmentation of ML, understanding a technology, like a social media platform, as a socio-technical system is a particular appraoch emphasizing how the social context in which the technology is used will influence its adoption and use patterns. 

I often see the term employed by research groups like Data and Society, conferences like the ACM Conference on Fairness, Accountability, and Transparency (ACM FAccT), and university research groups like [Princeton's CITP](https://citp.princeton.edu/) and [Cornell's Citizens and Technology Lab](https://citizensandtech.org/). 

[Data and Society](https://datasociety.net/about/)
> "Our work acknowledges that the same innovative technologies and **sociotechnical practices** that are reconfiguring society – enabling novel modes of interaction, new opportunities for knowledge, and disruptive business practices and paradigms – can be abused to invade privacy, provide new tools of discrimination, and harm individuals and communities."  

[ACM FAccT](https://facctconference.org/)
> "A computer science conference with a cross-disciplinary focus that brings together researchers and practitioners interested in fairness, accountability, and transparency in **socio-technical systems**."

Sociotechnical systems studies aim to demonstrate how technical infrastructure and social constructs, while imagined by traditional frameworks to largely exist separately, can constitute a system where powerful, self-reenforcing, and often unrecognized dynamics develop between previously disparate groups and entities. In the context of social media platforms, user targeting, recommender systems, and content moderation technologies are some examples of the technical constructs that enable, reenforce and govern (or fail to govern) relationships between previously unengaged communities. These novel proximities brought about between previously unentangled groups may ostensibly be the end platform designers intended. Empirical research on organized social abuse (such as harassment and disinformation campaigns) utilizing these same technical features, however, demonstrates that utilization patterns, and the outcomes produced, of these technologies are determined as much by the communities engaged with them as the designers of the features themselves.

I am primarily interested in empirical research investigating the proper measurement, propogation and effects of disinformation on social media platforms, as well as the unintended consequences of content moderation systems. However, encountering discussions of sociotechnical systems so frequently, I'm taking time to understand what the community of researchers means exactly when describing an analysis as employing this lense.

I'm starting here with a look into this [paper](https://www.usenix.org/conference/foci19/presentation/goerzen), titled **'Entanglements and Exploits: Sociotechnical Security as an Anlytic Framework'**, and authored by Matt Goerzen and Elizabeth Anne Watkins, researchers at the Data & Society Institute, and Gabrielle Lim, a researcher at the Technology and Social Change Project at Harvard's Shorrenstein Center. The paper was presented at the [2019 Free and Open Communication on the INternet Conference (FOCI)](https://www.usenix.org/conference/foci19).

The paper focuses the nature of threats to communities using social media platfroms, like disinformation and targetted harassment, and the security frameworks desinged to address these. By articulating the nature of participatory technologies, like social media platforms, and employing concepts from sociotechnical systems studies, the authors demonstrate how traditional security frameworks drawn from national security, computer security, and network security contexts, fail to address the emerging threats and inherent vulnerabilities associated with the sociotechnical systems, such as social media platforms. 

Some elements of traditional security frameworks addressed are the nature of the protected objects addressed by a security framework (the referent object), the nature of latent vulnerabilities of these participatory technologies stemming from deliberate design choices and incentive structures, and the systems of accountability between the public/regulators and platform creators and other responsible parties. Goerzen et al. investigate the following four incidents involving abuses of sociotechnical vulnerabilities on social media platforms to highlight the nature of these vulterabilities, as well as an analysis and security framework that might better address and anticipate similar abuses in the future.
- End FAthers day
- Christ Church

As an aside, I found [Paul Leonardi's 2012 paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2129878), which is referenced in the paper, as a great background to the usage of terms like "sociotechnical system". The paper investigates the increasing utitlization and evolving meaning of various terms, including sociotechnical system, by scholars investigating interactions between social and technological systems (focusing on non-physical information technology) between the mid 2000's and early 2010's.  

The meanings and targets of these terms shadow various simulatenous debates by scholars over theoretical frameworks and prioritizations of practical vs. theoretical research. These debates affect the aims of the studies employing terms like "sociotechnical", and thus similarly affect the meaning of the terms themselves. Furthermore, the evolution of new technologies in the 20th century, becoming more consistently defined and differentiated by software, instead of hardware, have also necessitated updated language.

All technical systems bear the capacity for abuse by users or external actors exploiting oversights in the system's design, and these systems typically adapt in order to protect their intended users, and encourage them to continue using the system for its ended ends.

For example, DDoS (Distributed denial-of-service) attacks, one of the oldest, but also most dynamically advancing methods of cybercrime, is an example of a threat to a system's technical vulnerability by external actors. First seen in 1996, the [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/#:~:text=A%20distributed%20denial%2Dof%2Dservice%20(DDoS)%20attack%20is,a%20flood%20of%20Internet%20traffic.) seeks to make a network resource unavailable to its intended user base by coordinating a network of compromised servers to flood a target with junk requests, overwhelming the target's capacity to communicate and effectively severing legitimate users. The technical methods used by perpetrators have evolved alongside network security, as demonstrated in early 2020, when [AWS defended against a DDoS attack](https://www.theverge.com/2020/6/18/21295337/amazon-aws-biggest-ddos-attack-ever-2-3-tbps-shield-github-netscout-arbor) with a peak traffic volume of 2.3 Tbps, the largest ever recorded.

While anticipating and defending against technical vulnerabilities, like DDoS attacks, is largely achieved by focusing on the technical aspects of the attack, analyzing the social components of these attack events also helps explain, and possibly help anticipate, similar attacks. In June 12, 2019, the encrypted messaging service, Telegram, was briefly [disrupted](https://www.bbc.com/news/business-48619804#:~:text=In%20a%20DDos%20attack%2C%20hackers,Americas%20and%20%22other%20countries%22.) for users in Hong Kong by a massive DDoS attack that was later linked to a network of bots with IP addresses mostly originating in China. The attack came as protesters in Hong Kong largely depended on Telegram to coordinate demonstrations as part of the Ani-Extradition Law Ammendment Bill Movement. This is to say, the attack on Telegram's technical vulnerabilities had social components as well, and understanding these, along with the political context of a technology, like Telegram, can help anticipate attacks of this nature in the future.

Still, this type of threat is not what is addressed by Goerzen et al.. "Sociotechnical exploits", the authors argue, are vulnerabitlies constituted by "both social and technical compoenents", as well as expoitable in ways that may harm specific communities engaged with the social media platform or other participatory technology.  

One example related in the paper is the #EndFathersDay Hoax in June 2014, where users on 4chan impersonated African-American feminist activists and started the Twitter hashtag, #EndFathersDay, in an attempt to coopt and sensationalize public conversations related to feminism or gender discrimination. The campaign was succesful in garnering the attention and amplification by conservative media outlets, like Fox News. 

As the Goerzen et al. state, "anonymity...long considered a pillar of free expression on the internet is just one condition that's been exploited", in this case, by organized hostile users to impersonate members of a marginzalied group, with the aim to discredit and confuse the aims of the group in the mind of the public. The authors stress that the attackers in this incident played to multiple vulnerabilities, both technical and social. 

According to research done by [Rachelle Hampton](https://slate.com/technology/2019/04/black-feminists-alt-right-twitter-gamergate.html) at Slate, some of tweets read as follows:

> “#EndFathersDay because I’m tired of all these white women stealing our good black mens.”
> “#EndFathersDay,” read one. “We’ll bring it back when men stop raping and killing us.”

The tweets and fake accounts, which originated from racist men's rights groups on 4chan, followed a pattern of utilizing

Hampton follows the investigation of Shafiqah Hudson as she first encountered and then began investigating the trail of the tweets associated with this hoax. Notably, Hampton notes that "To casual observers online, #EndFathersDay appeared to be the work of militant feminists, most of whom were seemingly women of color. To Hudson, the ruse was never anything but transparent...But the hashtag was already trending worldwide".

The technical vulnerabilities identified by Goerzen et al.:
- Ease of and annonymity afforded by account creation
- 
The social vulnerabilities identified by Goerzen et al.:
- mentions, followers, language, signifiers of legitimzy

According to Hampton, he hoax was ultimately uncovered by individuals like Hudson, who subsequently coordinated a counter campaign, using evidence





This distinction defines what the authors call Sociotechnical exploits. These, as the authors state, "are not related to a software bug or the failure of a particular technical component", but instead emerge as the social and technical aspects of a system interact. That is, "a tool that works as intended in a narrow se scenario...may demonstrate substantial capacity for abuse in another context". 

"However, these threats and the features that allow them to come into being, such as anonymous accounts and encrypted messaging, should not be conflated, as the latter serve a beneficial purpose. ". authors don't define a clear distinction between social, technical, and sociotechnical tyoically subjective. 

>"While these issues are not normatively considered a “security” threat by legacy technical computer security frameworks, we argue that ensuring the free and open communications of vulnerable people ought to be considered a key security concern, and that a sociotechnical security lens offers tools to grant entangled actors greater agency"

Section 2.2
vulnerabilities latent, incentives

Section 3: need for updated security framework 

> the strength of sociotechnical security as a concept
is in identifying the relationships between the various actors
(human and non-human), the positions of power that allow
the actors to engage with and influence the system, and most
importantly, the relational aspect of vulnerabilities and
threats within the system. 


Section 4: applying sociotechnical framework 
- key questions








