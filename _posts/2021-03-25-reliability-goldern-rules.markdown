---
layout: post
title:  "The Four Golden Signals"
date:   2021-03-24 16:06
categories: SRE, reliability, monitoring
---
# The Four Golden Signals

Monitoring distributed applications is a complex journey. Monitoring involves a set of tasks & practices of collecting, aggregating, measuring and
displaying (hopefully in real-time) informative data about a system.

There are many indicators to be measured and monitored, but according to Google, if you want to focus on the most important ones, you have to
learn about the "four golden signals". The four golden signals of monitoring are latency, traffic, errors, and saturation.

Measuring all four golden signals and page the team when one signal is problematic, your service will be pretty covered by monitoring.

## Latency

The time it takes to respond to a request.

## Traffic

A measure of how much demand is being placed on your system, measured in a high-level system-specific metric. For a web
service, this measurement is usually HTTP requests per second; For an audio streaming system, network I/O rate or concurrent sessions. For a given
storage system, might be the reads and writes per second.

## Errors

The rate of requests that fail, either explicitly such as HTTP 5xx or implicitly when getting HTTP 200 with the wrong content. Where protocol
response codes are insufficient to inform, end-to-end tests can detect when serving the wrong content.

## Saturation

How busy is the service? A measure of the system fraction, highlighting who is the most constrained (memory, I/O, etc.). Saturation is 
extremely important because performance degradation happens before achieving errors of 100% of utilization.


