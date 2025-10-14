---
layout: post
title: "Using LLMs for NPC Dialogue in Unity"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
image: kez03.jpg
---

In the last year, I have been working on a Master's degree in Human Centred AI for Games Development at Bournemouth University. I decided to make this change because I wanted to pursue a career in video games, wanted to enhance my skills in the burgeoning AI field and I also wanted an opportunity to return to the UK where I spent part of my childhood.

For my Ludonarrative Design coursework, I built a game called Brollyland. Brollyland is about an alien called Slorp as he tries to adjust to life in a thinly veiled satire of modern Britain. Along the way, I decided to play around with the idea of having one character have AI-driven dialogue because I wanted a character without static dialogue and see how well the AI handled.

Kez, as written in the NPC dialogue object, "talks like a London [roadman](https://www.urbandictionary.com/define.php?term=Roadman), uses slang and keeps it casual but funny".

*Running Gemini*
----------
Gemini

The local proxy runs from a small python script, called app.py. This script creates a lightweight local server that 

    from fastapi import FastAPI
    from pydantic import BaseModel
    from fastapi.middleware.cors import CORSMiddleware
    from google import generativeai as genai
    
    genai.configure(api_key="YOUR_GEMINI_KEY")
    model = genai.GenerativeModel("gemini-2.0-flash")
    
    app = FastAPI()
    app.add_middleware(CORSMiddleware, allow_origins=["*"], allow_methods=["*"], allow_headers=["*"])
    
    class Prompt(BaseModel):
        message: str
        
    @app.post("/generate")
    async def generate(prompt: Prompt):
        response = model.generate_content(prompt.message)
        return {"response": response.text}


*Auto-Launching the Server From Unity*
----------
