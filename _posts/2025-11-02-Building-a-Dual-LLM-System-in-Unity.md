---
layout: post
title: "Building a Dual LLM System in Unity"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
image: goahead00.jpg
---

As mentioned in my [previous post](https://gerardrobertkirwin.com/blog/2025/10/14/using-llms-for-npc-dialogue-in-unity), I have been working on a Master's degree. For my dissertation, I wanted to create a game that would improve the workplace training experience. 

I had one coworker who said he had taken the same anti-money laundering training every year for the last decade. How much engagement do employees have when they know all the answers to the quiz? How much do they really know about the subject matter and would they learn more if the difficulty of these quizzes changed on ability level? These were the questions going through my head when coming up with the idea for the game that would become *Go Ahead*.

To do this, I would build upon the work I did for my game *Brollyland* where I had one NPC speaking AI generated lines. I wanted to create AI generated scenarios, quizzes and dialogues. I wanted the ability to change between different topics. I wanted someone non-technical, like a HR professional, to be able to write a topic such as "first aid" or "GDPR" and the game would create the scenarios and quizzes needed to assess employees.

*Welcome to GoAhead*
----------

<img src="https://raw.githubusercontent.com/gerardrobertkirwin/gerardrobertkirwin.github.io/6ee433707065a532697f05de8378b6467f581a40/assets/img/goahead_office.jpg" class="center">

In building the game, I wanted to create a world and characters that are relatable. Explicitly in the scenario prompting, GoAhead is described as "a large, modern yet ordinary workplace". I retained the 2D format and similar art from *Brollyland* for practical reasons, it saved time and allowed me to focus on the AI part of the project. I also kept it similar because I felt like the format would be more relatable to a larger audience. It is visually similar to casual games such as *Animal Crossing* and *Stardew Valley*.

In my research, I found that LLMs provide better dialogue and personalities for characters when they base them off of existing characters. So I prompted the LLMs to take on the personalities of characters from the television shows *The Office* and *Parks and Recreation*. For example, the character Ade "speaks like Dwight Schrute from The Office", with "endency to interpret situations with a sense of order" but explicitly warning "Avoid specific Dwight-isms like mentions of beets or farming. Avoid references to Dunder Mifflin."


*Running Gemini and Ollama*
----------

I reused the GeminiServerLauncher from *Brollyland* but since I had to test out more robust prompting, I would have to call the API dozens, if not hundreds of times. 


*Auto Launching and Switching Models*
----------



*Conclusion and Next Steps*
----------
