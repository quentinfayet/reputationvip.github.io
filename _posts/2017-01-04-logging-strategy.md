---
layout: post
title: "ELK 1/3 : Logging strategies"
author: quentin_fayet
modified: 2017-01-04
excerpt: "Feedback and strategies about logging at Reputation VIP"
tags: [elk, elasticsearch, kibana, logstash, logging]
comments: true
image:
  feature: machine-learning-1-explained-grandma.jpg
  credit: Quentin Fayet
  creditlink: http://reputationvip.io
---

One of the most interesting work I've been doing at Reputation VIP, is
taking care and defining, along with my colleagues, the stack we'd use
to ingest our logs, and the logging strategy we'd adopt.

It has been 3 years now that we've been feeling the need of having a
robust logging stack, and a clean logging strategy that could give us
the possibility to leverage our logs to the maximum.

So, I decided to write 3 different articles, combining feedbacks and
advices, from the experience we had at Reputation VIP.

The first article will talk about our logging strategies: the pitfall
we fell into and the actions we decided to take to get our logs straight.
I will mostly talk about logging with Symfony 4 and Monolog, but most of
the things I'll write can be applied to other languages.

In the second article, I'll give a feedback about the different logging
ingestion stacks we've been using at Reputation VIP up to this day, what
was great, what was not, and why we chose to evolve from one stack to
another.

And, for the third article, I'll be happy to tell you more about our
current logging stack, it's diversity, how we manage to have a great,
non-costly stack using AWS and some incredible other services, how we
maintain it, and how we've built analysis dashboards.

# What is logging ?

Well, I guess most of the readers are aware of "what is logging", but
a little reminder cannot hurt anyone.

Logging is a way for your application to talk to you; inform you of
what's happening, when, and why. A big application, such as the one
we build at Reputation VIP, can output thousands of loglines every hours.
For instance, at the time, we're producing around 10 millions of loglines
every day, representing 4Go a day.

A log (or logline) consists of several information. Usually, we at least
find the following information:

- A timestamp, indicating when the log has been produced. This information
is, in most of the cases, handled by your logging library.
- A message, which is, in general, a human-readable text, indicating in
a few words, what's the log about. When written by you, this message
is really clear, most of the time. In the other hand, with some applications
sur as databased, these log messages are generated automatically and may
not be easy to read.
- A context, or metadata, or whatever you want to call it, that describes
the environment in which the log has been produced. It should contain some
information that may help you understand more deeply the message.

Log lines can have different formats. Some of them such as NGinx logs,
obey to patterns, that can be defined in the configuration file. Some
other (such as our logs at Reputation VIP) are JSON encoded, allowing to
get the information quickly. Each application, each software, each service
you may use, has its own way to produce log; it means you must adapt in
order to parse them for further analysis processes.

But, for ...