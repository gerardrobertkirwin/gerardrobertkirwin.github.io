---
layout: post
title: "Using LLMs for NPC Dialogue in Unity"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
image: kez03.jpg
---

In the last year, I have been working on a Master's degree in Human Centred AI for Games Development at Bournemouth University. I decided to make this change because I wanted to pursue a career in video games, wanted to enhance my skills in the burgeoning AI field and I also wanted an opportunity to return to the UK where I spent part of my childhood.

For my Ludonarrative Design coursework, I built a game called *Brollyland*. Brollyland is about an alien called Slorp as he tries to adjust to life in a thinly veiled satire of modern Britain. Along the way, I decided to play around with the idea of having one character have AI-driven dialogue because I wanted a character without static dialogue and see how well the AI handled.

Kez, as written in the NPC dialogue object, "talks like a London [roadman](https://www.urbandictionary.com/define.php?term=Roadman), uses slang and keeps it casual but funny" is our AI NPC. I picked a roadman for our AI NPC as it's a stereotypical character so it provides less room for ambiguity than inserting basic character traits. It also is a quintessential part of modern British life, it felt essential to include. 

*Running Gemini*
----------
Google's Gemini model is the power behind Kez's words. Based on a tip from a classmate, I found that it was a reliable model that I could use without paying for credits and running up a big usage bills. I was able to sign up using my existing Google account and get an API within minutes.

Gemini was connected through a local FastAPI proxy server. The local proxy runs from a small python script, called app.py. This script creates a lightweight local server that acts as a bridge between Unity and Google's API.

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
In early versions of the game, I had to set up the server manually by running lines in the console. I found this tedious and worked to find a way to eliminate the need for players to tinker with the console.

<img src="https://raw.githubusercontent.com/gerardrobertkirwin/gerardrobertkirwin.github.io/refs/heads/master/assets/img/brollymanual.jpg">

A C# script in the game, GeminiServerLauncher, runs when the game starts. It starts the server by triggering the python script from it's location. For the purpose of well-structured file organization, I tend to keep it in the same folder as the game itself. 

    using System.Diagnostics;
    using UnityEngine;
    using System.IO;
    
    public class GeminiServerLauncher : MonoBehaviour
    {
        public string pythonScriptPath = @"[path/to/your/app.py]";
        public string pythonExecutable = "python"; // or "python3"
        
        void Start()
        {
            StartGeminiServer();
        }
        
        void StartGeminiServer()
        {
            string workingDir = Path.GetDirectoryName(pythonScriptPath);
            
            ProcessStartInfo startInfo = new ProcessStartInfo
            {
                FileName = pythonExecutable,
                Arguments = $"\"{pythonScriptPath}\"",
                UseShellExecute = false,
                RedirectStandardOutput = true,
                RedirectStandardError = true,
                CreateNoWindow = true,
                WorkingDirectory = workingDir
            };
            
            var process = new Process { StartInfo = startInfo };
            process.OutputDataReceived += (_, e) => { if (!string.IsNullOrEmpty(e.Data)) Debug.Log("[Gemini] " + e.Data); };
            process.ErrorDataReceived += (_, e) => { if (!string.IsNullOrEmpty(e.Data)) Debug.LogError("[Gemini ERROR] " + e.Data); };
            
            process.Start();
            process.BeginOutputReadLine();
            process.BeginErrorReadLine();
        }
    }

*Prompt Flow*
----------
Line?



*Conclusions and Next Steps*
-------------
