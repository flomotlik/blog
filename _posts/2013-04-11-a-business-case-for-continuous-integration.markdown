---
layout: post
title: A Business Case for Continuous Integration
author: Joe Green
twitter: joegreen88 
<!-- google: 115123333592137547204 -->
description: Continuous Integration provides sound economical benefits. This post examines the bottom-line advantages of Continuous Integration with respect to any business dealing with software deployment.
image: http://blog.codeship.io/images/ci_as_a_business_case/200_200_ci_as_a_business_case.png
---

![Blog dependencies](/images/ci_as_a_business_case/codeship_ci_as_a_business_case.png)
*Original image by <a href="http://www.thechangeblog.com/author/ali-luke/" target="_blank">Ali Luke</a>*

***Recently we discovered a great article by Joe Green from smrtr.co.uk. He wrote about Continuous Integration as a business case. We liked it so much that we asked him to publish it on the Codeship blog.***

You can find Joe at <a href="http://smrtr.co.uk" target="_blank">smrtr.co.uk</a> or on twitter <a href="http://www.twitter.com/joegreen88" target="_blank">@joegreen88</a>.

* * *

Continuous Integration provides sound economical benefits. This post examines the bottom-line advantages of Continuous Integration with respect to any business dealing with software deployment.

*On the ground,* the benefits of <a href="http://martinfowler.com/articles/continuousIntegration.html" target="_blank">Continuous Integration</a> are:

* Eradication of manual FTP deployment
* Prevention & reduction of production & staging errors
* Generation of analysis & reporting on the health of the code base

*In business terms,* the value of Continuous Integration is:

* Reducing risk
* Reducing overheads across the development & deployment process
* Enhancing the reputation of the company by providing Quality Assurance

#CI Reducing Risk

Integrating code more frequently leads to reduced risk levels on any project. So long as there are metrics in place to measure the health of the application, code defects can be detected sooner and fixed quicker. When a development team integrates their work frequently it means there is less of a gap between the current state of the application and the state of the application as the developer has it. And so the scope for assumptions is reduced.

##Analysing and Reporting

Incorporating continuous testing and inspection into the CI process is valuable to developers and managers alike. There are insights to be gained on the progressive health of the project, and it is useful to have a historical timeline of metrics to draw upon and look to for trends. The availability of these analyses tends to encourage a period of reflection after each integration which will underpin the direction of the project.

The metrics of greatest interest will depend on the key requirements of the project. Some typical code metrics include:

* unit tests
* complexity
* dependency
* length
* <a href="http://martinfowler.com/bliki/CodeSmell.html" target="_blank">code smell </a>

##Quarantining Errors

Because Continuous Integration integrates and runs tests and inspections regularly, there is a good chance that problems will be discovered when they are introduced as opposed to during post-deployment testing, or worse, in the production environment. Having [a good CI system](https://www.codeship.io) is akin to having a fire alarm in your house. It won't fix your error for you but you will rest assured that it will flag up the error as soon as it's integrated and that the build containing the error will not make it into your staging or production environments.

##Validating Assumptions

You write a piece of software in your language of choice. It has to work on Linux and Windows. You develop on a Linux machine while your co-worker develops on Windows. You both commit your work to version control regularly. You both work on different parts of the project, diligently testing locally along the way. You're both happy with what you have written. You release your code into the world and much to your surprise you see some of your work isn't working on Windows and some of your co-worker's work isn't working on Linux. What happened? You made assumptions about your work which were never verified.

Continuous Integration with unit testing gives you the chance to replace assumptions with knowledge. Referring back to the previous example, two CI environments could be used - one on Linux and one on Windows - which will each integrate the work with each commit to version control. Under this system there's no way that those cross-platform errors would have made it past the development environment.

#CI Reducing Overheads

The initial overhead of setting up a CI system will be outweighed by the man-hours saved along the way. [Finding a bug while it's limited to the development level](http://blog.codeship.io/2013/03/15/Testing-top-to-bottom.html) is the cheapest possible way to find it. If that bug needs to be fixed in any other environments, then your costs are higher. Some of the newer CI systems are quite easy to set up, so the overhead is not really that huge. Setting up the Codeship for instance only takes a couple of minutes. 

Another thing to consider is what your current Quality Assurance process is like and how much of that can be replaced by a good CI setup.

##Automating Processes

Repetitive manual processes are slow and prone to human error. [Deployment tasks should be bundled up and automated.](http://blog.codeship.io/2013/03/11/New-deployment-configuration.html) On the Codeship you can run as many deployment methods in a row as you want.

Reducing repetitive manual processes lets you be assured that:

* An ordered process is followed
* The process runs the same way every time
* The process will run every time code is committed to version control

These assurances serve to facilitate:

* The reduction of labour on repetitive processes, freeing people to do higher-value work
* The capability to overcome resistance (from other team members) to implement improvements

Too often we see time spent on deployment tasks such as creating space for server-generated files. Processes should always be more streamlined and cater to the development of deployment scripts which run automatically upon deployment.

#CI is Quality Assurance

Continuous Integration can enable you to release deployable software at any point in time. From a client perspective, this is the most obvious benefit. Projects that do not embrace this approach are prone to delayed releases. Or, worse, a release cycle being prevented by unforeseen issues which arise from deployment.

##CI enables Better Project Visibility

Continuous Integration provides the opportunity to notice trends and make informed decisions. It helps build the confidence to innovate new improvements. Projects suffer when there is no real or recent data to support decisions. Typically, team members collate this information manually, requiring time and effort. The result is that often the relevant information is never gathered. Continuous Integration can facilitate:

* *Effective Decisions –* providing near-real-time information on the recent build status and code quality metrics. 
* *Noticing Trends –* since integrations occur frequently with a Continuous Integration system, the ability to notice trends in build success or failure, overall quality, and other pertinent information becomes possible.
* *Documentation –* Continuous Integration offers the ability to generate API-like documentation for all committed code, making the software architecture immediately visible to new developers and other team members.

#CI increases Confidence in the Software

Overall, effective application of Continuous Integration practices can provide greater confidence in producing software. With every build, the team knows that tests are run against the software to verify behaviour, that project coding standards are being adhered to, and that the result is a functionally testable product.

Since Continuous Integration can inform you immediately when something goes wrong, developers and other team members have more confidence in making changes. Because Continuous Integration encourages a single-source point from which all software assets are built, there is greater confidence in its accuracy.

##Conclusions
The Codeship crew thinks Joe is spot on with his thoughts on Continuous Integration. We notice every day how much our CI setup speeds up our workflow. It encourages the team to push every change without being afraid of breaking something.