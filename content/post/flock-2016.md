+++
author = "Ratnadeep Debnath"
categories = ["Fedora", "flock", "waartaa", "ircb", "planet", "rtnpro", "krakow", "poland"]
date = 2016-08-13T16:35:11Z
description = ""
draft = false
image = "/images/2019/08/flock-2016-recap-1024x433.jpg"
slug = "flock-2016"
tags = ["Fedora", "flock", "waartaa", "ircb", "planet", "rtnpro", "krakow", "poland"]
title = "Flock 2016"

+++


![]()
This year, [Flock](https://flocktofedora.org) took place in [Krakow, Poland](https://en.wikipedia.org/wiki/Kraków) from Aug 2 - Aug 5, 2016. I reached at Krakow on July 31, 2016.

## Day 0
Next day, we met other attendees who started coming down from various places. In the afternoon, we went out for a city tour, visited [Oskar Schindler's Enamel Factory](www.krakow-info.com/schindler.htm), had lunch and returned back to the hotel. In the evening, we headed out to a nearby mall to have dinner with fellow attendees.

## Day 1
Joe Brockmeier kickstarted the first day of Flock followed by the keynote on "State of Fedora 2016" by our fearless Fedora Project leader, Mathew Miller. I then attended Adam Miller's talk on **Fedora Docker layered image builds**. Then, I attended the annual Flock talk on **The state of Fedora Infra** by *pingou* and *nirik*. Post lunch, I spent the day working on fixing my demos for the talk on Nulecule with help from [Tomas Kral](https://github.com/kadel).

## Day 2
After attending the keynote, I spent the time till lunch preparing for my talks on Nulecule and Waartaa.

After lunch, it was time for my **Nulecule** talk. Among the attendees were David Duncan, Scott Collier, Josh Berkus. In started my talk by introducing the Atomic Developer Bundle, followed by the state of the packages in container ecosystem, especially, with multi container applications, and why we need a better way to package multi container application. Thus, I introduced Nulecule and how it solves our problems by allowing parameterization of metadata and by being agnostic of container orchestration platforms. I introduced Atomic App, which is an implementation of the Nulecule spec. Then I demoed running multi container Nulecule applications on Docker, Kubernetes and Mesos/Marathon providers. It was followed by a healthy discussion about the future of Nulecule and where we want to go in the future.

After my talk was done, I attended the talk on "Docker in production" by Tomas Tomecek, followed by the talk on **Bugyou** by [Sayan Chowdhury](https://github.com/sayanchowdhury). Then, it was time for our talk on **Enabling realtime IRC chat on Fedora Hubs**. In this talk, myself and Sayan took the attendees through our journey from the monolithic Waartaa to the microservices one, built on top of server side flux. We also showcased IRCB, our scalable IRC bouncer service. Then, we demo'd our proof of concept implementation of the upcoming Waartaa/Fedora Hubs integration. The talk was well received by the attendees, followed by some useful discussions and arguments over choosing IRC over XMPP, etc.

Following our talk, we attended [Kushal](https://github.com/kushaldas)'s talk on **Test containers with Tunir**. In the evening, we all went to the cruise tour on beautiful Vistula River.

## Day 3

The day started with lightning talks, followed by FAMSCO meeting. Post lunch, I attended the CommOps workshop. I spent the rest of the day hacking on Waartaa. In the evening, we went to Brewery Lubicz, for drinks and food.

## Day 4

Day 4 had a laid back start for me, with the Fedora Budget workshop, where we discussed about the current state of budget in Fedora, issues with reimbursements and how to improve the workflow. We also discussed on how to create more impact with the money spent for Fedora.

After this, I attended the Fedora Hubs Hackfest/meetup led by [pingou](https://fedoraproject.org/wiki/User:Pingou) where he demoed the current state of Fedora Hubs and we discussed about our upcoming [Waartaa](https://github.com/waartaa) integration with Hubs. We also approached [Masha](https://www.facebook.com/masha.leonova) to help us with making mockups for waartaa, and she agreed to help us. Then, I spent most of my day learning React [Redux](http://redux.js.org/) and reading other source code, to get ready to start working with the front end for Waartaa.

In the evening, we went out for dinner with the Fedora folks and friends.

## Goodbye
I checked out from the hotel at 4 AM on August 6, 2016 to travel back home, to Bengaluru, India.

It was an awesome experience at Flock this year, speaking about the work I was doing, and learning about the great work others in the community are doing. We had loads fun during the conference days, be it in the day (hacking/speaking/learning) or in the evening (cruising on a river/drinking,eating and talking with friends). I'd like to thank Red Hat to sponsor my trip to Flock. I brought a bunch of tasks to work on, from Flock, the primary one being getting Waartaa ready for Fedora Hubs integration.

