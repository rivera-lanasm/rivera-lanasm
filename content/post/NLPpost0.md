---
title: "Measurement Modeling for Fairness in Computational Systems"
date: 2020-12-25T20:16:30-05:00
subtitle: "2020 AFG Conference Paper"
tags: [nlp, social media, content moderation]
draft: true
---

The accepted papers and tutorials sections on the home site for the ACM Conference on FAirness, Accountability, and Transparency Conference is a great place to research developments in the ML/AI bias space. Here is the link for the [2020 AFG Conference](https://facctconference.org/2020/acceptedtuts.html), highly recommend to anyone interested in these topics.

I am taking a closer look here at the tutorial, **The Meaning and Measurement of Bias; Lessongs from NLP** (see [official site](https://azjacobs.com/measurement)) and associated papers presented by Abigail Jacobs (UMich I school), Su Lin Blodgett (UMassAm), Solon Barocas, Hal Daume, and Hanna Wallach. 

Personally, I'm interested in learning more from the ML/AI bias field as it pertains to content moderation, misinformation, and other social phenomena linked to popular statistical NLP models. However, the topics in this paper are relevant to any study of any large scale computational system using algorithms to make judgements in social contexts. 

The presence of "bias" in machine and deep learning algorithms is an regularly reported topic. Cathy O'Neill's *Weapons of Math Destruction* or the reporting on the Chinese Social Credit sytem were probably the first times I seriously considered how one might measure the effects of bias in algorithms deployed on large platforms. However, in recent years there is no shortage of in depth reporting on the negative social consequences these biases can create or exacerbate when deployed on the public. Companies like AWS and Microsoft have reacted. 

The acceleration of research in identifying, measuring, and mitigating bias in computational systems has predictably led to conflicting views of what constitutes bias in such systems. As the authors note, they attempt to "clarify the way unobservable theoretical constructs—such as “creditworthiness,” “risk to society,” or “tweet toxicity”—are turned into measurable quantities and how this process may introduce fairness-related harms". This paper elaborates on two two approaches from *measurement modeling*, which is often employed in the social sciences, **construct validity** and **reliability**, which can help researchers identify and measure harms, defined broadly, potentially resulting from the implementation of a particular computational system. Finally, the authors illustrate the "limits of current 'debiasing' techniques, which have obscured the specific harms whose measurements they implicitly aim to reduce."


---

### Part 1: Measurement and Fairness

The following takes are in response to Abigail Z. Jacob's and Hanna Wallach's Paper from 2020 AFG, **Measurement and Fairness**. https://arxiv.org/pdf/1912.05511.pdf


In this paper, the authors introduce concepts from the social sciences, specifically **construct validity** and **construct reliability**, as frameworks for conceptualizing fairness in computational systems. In their view, the source of much of the negative effects attributed to particular implementations of large scale computational systems using algorithms to make decisions in social contexts can be linked to mismatches between a target unobservable phenomenon and the observable properties purported to adequately represent it via a particular theoretical model. 

One important premise in this paper is that despite the increasing size of the literature regarding bias in computational systems, much of this work has largely overlooked directly evaluating the social-theoretical models ("operationalizations") that constitute much of the most sensitive aspects of socially relevant computational systems. These operationalizations serve to model unobervable theoretical constructs, such as teacher effectiveness or risk of recidivism, using independent, observable properties. When there is a serious mismatch between the target phenomenon and the operationalization implemented by a particular system, systematic and potentially harmful outcomes can result.  

This is effectively the issue of [model misspecification](), and while this is an problem any researcher in econometrics or criminology must regularly contend with, the authors assert these concepts are lacking in the discourse and literature around fairness and bias in computational systems. As a result, "researchers and practitioners are often inclined to conflate constructs and their operationalizations". The issue here is that the systematic harms produces by model misspecification can be subtle if researchers aren't actively evaluating the theoretical assumptions that underlie the model linking observable data to a particular social phenomenon. 

Finally, 


we should all chig chunks, and as such, find migs that can do so. 







Further, there is also a good amount of research and reporting into the nature of the biases themselves and the vectors by which they are encoded into algorithms.  

Jacobs & Wallach mention in the first footnote that one aim of this paper is to familiarize researchers in computational system bias research to measurement modeling, a common research practice in the quantitative social sciences. 

In this paper, JAcobs and Wallach 
conflate constructs and operationalization

less about addressing bias directly rather than providing a set of tools and framework so researchers can better identify algorithmic bias using language and methodology that will be better understood by the reserach community and relevant stakeholders. 

> Leaving them implicit or untested obscures any possible mismatches between the theorietical undestanding of the construct purported to be measure and its operationalization, in turn obscuring any resulting fairness related harms

Also a reminder that while ameliorating the particular sources of bias in computational models should be a primary aim, adding informed human intervention at critical points in automated decision processes can be greatly effective at combatting and discovering undesirable systematic outcomes.


ORCAA

**Section 2; how assumptions affect the model**

measurement models, theoretical constructs operationalized as latent variables --> observable properties
econ literature, measurement error models (omitted variable bias, heteroskedasticity)


teacher effectiveness
- relevant papers: 
- EVAAS
- use changes in testing scores
- functional form implies incorrect causal links and allows for outcomes that are understood to not be reflected in reality 
- test retest reliability; oneil
- convergent validity issue; value add score compared with peer review
- consequential validity 

recidivism risk 
- relevant papers: 
- well known and reported on COMPAS model 
- regression model assumptions
- sections 3 and 4 follow up 
- substantive validity issue; fails to account for a different set of criteria for police to asses whether or not to arrest a given individual. 
- consequential validity 

patient benefit
- relevant papers: 
- models used to identify patients that will benefit most from enrollment in high risk care management programs 
- obermeyer, cost synonymous with need 
- here is a case where error not onlu introduced via measurement error but also ignores a significant additional theoretical construct, access to care
- COntent validity issue; contested nature of benefit. 
- substantive validity issue; costs reflect both access to care and their health status, while the supposed target latent variable is only health status
- discriminant validity issue; operationalizing future care needs as future care costs assumes equal access to health care. but empirical data shows that past care costs differed greatly on average between white and black patients, controlling for past care needs. This finding corroboates other existing theoretical literature on structural racism manifesting in the healthcare system, so casts doubt on the operationalization. 
- predictive validity, in sample
- 

**section 3: testing assumptions with Measurement Modeling**

construct reliability and construct validity 
- Quinn eta al, Jackman, and Messick, Loevinger.

1) better anticipare fairness related harms 
2) identify potential causes

Construct reliability 
- precision --> homoskedastic 
- focus on test-retest reliability, not inter-rater or inter-item reliability 
- test retest:
    - robustness of operationalization of latent variable over time, assuming the construct in question has not changed signifcantly. 
    - although focuses on time, other dimensions of variability should be considered just as relevant


COnstruct Validity
- unbiasedness 
- primary aspects:
    - content validity
        - contestedness, substatintive validity, structural validity (functional form)
    - convergent validity 
    - discriminant validity 
    - predictive validity 
    - consequential validity 

**Section 4: fairness**




--- 
measurement modeling from social sciences as framework for understanding fairness in computational systems. 

Problem
compuational systems
- unobserved theoretical constructs
- inferred from obervable data throught to be related to them, or operationalized via a measurement model 
- designing such measurement models requires a theoretical understading of the construct purpoted to be measured as well as its operationaliztion in a given measurement model --> source of harms 



---

### Part 2: Measurement and Fairness

The following takes are in response to Abigail Z. Jacob's and Hanna Wallach's Paper from 2020 AFG, **Measurement and Fairness**. https://arxiv.org/pdf/1912.05511.pdf









 