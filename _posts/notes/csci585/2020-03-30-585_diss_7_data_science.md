---
layout: post
title: "[Notes] CSCI 585 discussion 7 Data Science
"
tags: [csci585, eng, note]
---

Functional programming, languages... Why is functional programming relevant/related to Big Data processing?
- passing the function to map which take function as an input

SIMD - how does this relate to Big Data processing?
- single instruction (map function) multiple data

Dataflow... again - how is this related to Big Data processing?
- data processing paradigm. connection through multiple stage map reduce. (chain of operation)

MR: programming in Java/Python/... vs Pig/Hive/...
- Pig/Hive gives higher levels of abstraction

what is the 'pub sub' architecture, what is its benefit?
- public subscriber (bus line), decoupling, gain flexibility on structure, easier maintenance, fewer connecting

Musketeer and Kafka - similarities?
- Musketeer decoupled front-end and back-end
- Kafka is a distributed pub-sub real-time messaging system that provides strong durability and fault tolerance guarantees.
- break the direction by introduce third part

Categories of people, in the data science pipeline (3, actually 4)?
- data engineer, data scientist, **data Ops**, and fourth categories

Data Science - why did YOU (want to) get into it?
-  

Connection between data science and the scientific hypothesis?
- data science collect data before analyse, then try to answer questions
- hypothesis given first, and collect certain data to prove the hypothesis

Platform convergence (end-to-end solutions) - Cloudera's DS workbench, Intel's Nervana and Saffron; DataOps appliances - Nexla, Data Kitchen, Qubole; Productivity enhancements - Data Robot, Bansai, SqlStream, Qubole; 'turnkey' appliances - Anodot, GeoStrategies. What do these 4 items point to?
- for non-tech background people
