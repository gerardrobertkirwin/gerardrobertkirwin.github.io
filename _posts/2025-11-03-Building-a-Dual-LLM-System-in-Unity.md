---
layout: post
title: "Building a Dual LLM System in Unity"
author: "Gerard Kirwin"
categories: blog
tags: [sample]
image: goahead00.jpg
---

As mentioned in my [previous post](https://gerardrobertkirwin.com/blog/2025/10/14/using-llms-for-npc-dialogue-in-unity), I have been working on a Master's degree. For my dissertation, I wanted to create a game that would improve the workplace training experience. 

I had a coworker who said he had taken the same anti-money laundering training every year for the last decade. How much engagement do employees have when they know all the answers to the quiz? How much do they really know about the subject matter and would they learn more if the difficulty of these quizzes changed on ability level? These were the questions going through my head when coming up with the idea for the game that would become *Go Ahead*.

To do this, I would build upon the work I did for my game *Brollyland* where I had one NPC speaking AI generated lines. This time, I wanted to create AI generated scenarios, quizzes and dialogues. One of the goals was to have someone non-technical, like a HR professional, be able to write a topic such as "first aid" or "GDPR" and the game would create the scenarios and quizzes needed to assess employees.

*Welcome to GoAhead*
----------

<img src="https://raw.githubusercontent.com/gerardrobertkirwin/gerardrobertkirwin.github.io/ca10c7f3aaa379a5bb34f7712aacd9a52e26ed5b/assets/img/vlcsnap-2025-11-03-19h47m42s714.png">

In building the game, I wanted to create a world and characters that are relatable. Explicitly in the scenario prompting, GoAhead is described as "a large, modern yet ordinary workplace". I retained the 2D format and similar art from *Brollyland* for practical reasons, it saved time and allowed me to focus on the AI part of the project. I also kept it similar because I felt like the format would be more relatable to a larger audience. It is visually similar to casual games such as *Animal Crossing* and *Stardew Valley*.

In my research, I found that LLMs provide better dialogue and personalities for characters when they base them off of existing characters. So I prompted the LLMs to take on the personalities of characters from the television shows *The Office* and *Parks and Recreation*. This is done through a local ScriptableObject that has a specific description for each character, developed along with the LLM for optimisation. For example, the character Ade "speaks like Dwight Schrute from The Office", with "endency to interpret situations with a sense of order" but explicitly warning "Avoid specific Dwight-isms like mentions of beets or farming. Avoid references to Dunder Mifflin."


*Running Gemini and Ollama*
----------

I reused the GeminiServerLauncher from *Brollyland* to connect Unity to a local API proxy. However this game required more robust prompting, where I would have to call the API hundreds of times to iterate prompting and game mechanics. Gemini has a robust free plan but I also did not want to run into an issue where I would have to approach or exceed free usage limits. So I developed a solution where I could use a locally run LLM to test prompts and other game mechanics.

I changed my code so it could use the [Ollama](https://ollama.com/) model runner, which allows for powerful models to be run directly from my machine. To keep some consistency, I decided to continue using a Google product, in this case the gemma3n models. This allowed me to test the game offline, but there were some issues. The latency was poor and I still have not resolved why this was the case. I ran the model in a terminal window with the same prompts relatively quickly. But in Unity, it took minutes to load a scenario, dialogue and quiz.

Doing some research, there may be a few reasons why this is the case. CPU limitation, model size (my model was over 7 GB), quantization and the lack of persistent context. Due to the time restraints of my project, I did not investigate this further but in the future it would be worth another look. 

Running a model locally has many uses where running *GoAhead* locally could be very useful. A company or an organisation can do some context aware fine-tuning, that is they could create their own local LLM. This model could include relevant procedures and policies at inference time. Along with retrieval-augmented generation (RAG), this local model could act as a secure conduit between current documentation and training scenarios and dialogue serving as a more engaging experience for employees.


*Auto Launching and Switching Models*
----------

In later builds of the game, I decided that instead of replacing the Ollama set up entirely with a call to the Gemini server, I wanted to provide the ability to switch between the local LLM and the online version.

<img src="https://raw.githubusercontent.com/gerardrobertkirwin/gerardrobertkirwin.github.io/96d1403af4fc3195ea9cd9b3f9aec59b587fafb9/assets/img/vlcsnap-2025-11-03-15h55m19s208.png">

I did this by creating a dropdown menu in the options menu, where the player (or a user setting up the game) could select between Ollama and Gemini. In practice, players would need to have Ollama installed on their device, so for gameplay testing I allowed Gemini to be used.

    public IEnumerator Generate(string prompt, System.Action<string> callback)
    {
      if (backend == Backend.Ollama)
      {
        bool done = false;
        string result = null;

        yield return StartCoroutine(
            LocalModelManager.Instance.GenerateFromOllama(
                prompt,
                r => { result = r; done = true; },
                () => { done = true; }
            )
        );
        yield return new WaitUntil(() => done);

        if (!string.IsNullOrEmpty(result))
        {
            callback(result);
            yield break;
        }

        Debug.LogWarning("[UnifiedModelManager] Ollama failed, falling back to Gemini");
    }

    yield return GenerateFromGemini(prompt, callback);
    }

The script above provides a check to see if Ollama is the chosen option (it is the default model in the game) and tries it first. It falls back to Gemini if Ollama is unavailable for any reason. Again, the logic is kept within Unity with no separate launcher or manual switching needed. This set up also lends itself to future work such as multi-model fallback, load balancing and local enterprise models as described above.

*Conclusion and Next Steps*
----------

While the game is complete with functioning scenario, dialogue and quiz generation as well as game features such as points, badges and game advancement; it is still a prototype. While there are other workplace training games out there, and other games that use LLMs to power dialogue and plot, *GoAhead* is the only one I've seen so far to bring the two together.

Items that would be next steps in development would include:
* Difficulty adjustment is rudimentary at this point, simply based on a number of points determined by correct, semi-correct or incorrect answers. A more robust difficulty system, in real time and based on more data points would be the first place to start.
* Improving prompting to try and create more dynamic dialogue, scenarios and quizzes. The nature of the LLMs used and those on the market provide limitations and opportunities in this case. But the current game relies too much on a combination of my suggestions in prompts and it's training data. The game should move beyond one-shot prompting.
* As mentioned above, improvements to the models and the ability to use methods such as RAG to create hyper-specific and vetted materials for the game.
* More interactivity in the game. Currently the game involves moving through dialogue and answering multiple choice questions. Making the player type out an answer or move through different rooms would enhance the experience.
