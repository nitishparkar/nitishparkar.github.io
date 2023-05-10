---
title: "Get motorhead up and running with langchain"
---

LLMs are inherently stateless. If you are building an app like a chatbot that needs to maintain conversation context, you need memory. **Motorhead** makes it easy to add long-term memory to chatbots.

If you are unfamiliar with the concept of conversational memory, [here](https://www.pinecone.io/learn/langchain-conversational-memory/) is a brilliant explainer from Pinecone about it.


## Redis

Motorhead uses Redis as a backend. The Redis instance needs to have the **RediSearch** module enabled, though. The easiest way to get a Redis + RediSearch instance is through [Redis labs](https://app.redislabs.com/). It'll give you a Redis URL that you can pass to the Motorhead server.

## Motorhead server

Motorhead gives you a couple of [options](https://github.com/getmetal/motorhead#how-to-run) to run the server. The simplest way is using docker image, passing OPENAI_API_KEY and REDIS_URL.

```bash
docker run --name motorhead -p 8080:8080 -e MOTORHEAD_PORT=8080 -e REDIS_URL='<redis>' -e MOTORHEAD_LONG_TERM_MEMORY=true OPENAI_API_KEY='sk-<>' ghcr.io/getmetal/motorhead:latest
```

You can verify that the server is running correctly by hitting endpoints specified in [Motorhead README](https://github.com/getmetal/motorhead/blob/main/README.md). 

## Langchain

Once you have the Motorhead server up and running, you can use the **MotorheadMemory** class in Langchain to store chat messages to it. Langchain's [Python docs](https://python.langchain.com/en/latest/modules/memory/examples/motorhead_memory.html) do a decent job of explaining it.

[Here](https://github.com/nitishparkar/langchain-experiments-py/blob/master/summarization/motorhead_server.py) is a basic Flask server that accepts an input message, sends it to OpenAI + Motorhead, and returns AI's response.


> Gotcha:
>
> There is a bug in the current MotorheadMemory implementation. It doesn't include context from Motorhead while generating the prompt. I have reported it in [their discord](https://discord.com/channels/1071958892134793306/1091753611878486036/1101082293566713936). As a workaround, you can manually pass the context in the prompt like [this](https://github.com/nitishparkar/langchain-experiments-py/blob/master/summarization/motorhead_server.py#L27).


Motorhead (and even Langchain) is in the early stages of development. You are likely to face many issues while using it. The best place to get those resolved is their Discord. Motorhead does make creating conversational chatbots with long-term memory easier. It would be interesting to see how they evolve from here.  