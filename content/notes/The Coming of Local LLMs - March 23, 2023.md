---
title: "The Coming of Local LLMs"
date: "2023-03-23"
---

While there’s been a truly remarkable advance in large language models as they continue to scale up, facilitated by being trained and run on larger and larger GPU clusters, there is still a need to be able to run smaller models on devices that have constraints on memory and processing power. 

Being able to run models at the edge enables creating applications that may be more sensitive to user privacy or latency considerations - ensuring that user data does not leave the device.

This also enables the application to be able to always work without concerns over server outages or degradation, or upstream provider policy or API changes. 



## LLaMA Changes Everything

Recently, the weights of [Facebook’s LLaMA model](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/) [leaked via a torrent posted to 4Chan](https://www.theverge.com/2023/3/8/23629362/meta-ai-language-model-llama-leak-online-misuse). This sparked a flurry of open-source activity, including [llama.cpp](https://github.com/ggerganov/llama.cpp). Authored by the [creator](https://ggerganov.com) of [whisper.cpp](https://github.com/ggerganov/whisper.cpp), it quickly showed that it’s possible to get an LLM running on an M1 Mac:

[https://twitter.com/ggerganov/status/1634320862722551809](https://twitter.com/ggerganov/status/1634320862722551809?s=20)

![LLaMa_On_M1](/blog_assets/2023/LLaMA_On_M1.png)



Soon, Anih Thite [posted a video](https://twitter.com/thiteanish/status/1635188333705043969) of it running on a Google Pixel 6 phone. It was incredibly slow, at 1 token per second, but it was a start. 

The next day, [he posted a new video](https://twitter.com/thiteanish/status/1635678053853536256) - showing it running at 5 tokens a second. 

![LLaMA_Pixel6](/blog_assets/2023/LLaMA_Pixel6.png)



[Artem Andreenko](https://twitter.com/miolini) soon was able to get LLaMA running on a Raspberry Pi 4: 

![LLaMa_On_Raspberry_Pi](/blog_assets/2023/LLaMA_On_Raspberry_Pi.png)



[Kevin Kwok](https://twitter.com/antimatter15) created a [fork of llama.cpp](https://github.com/antimatter15/alpaca.cpp) that included an add-on for a Chat-GPT style interface:

![AlpacaCPPChat](/blog_assets/2023/AlpacaCPPChat.png)



And finally, yesterday, he [posted a demonstration of Alpaca running on an iPhone](https://twitter.com/antimatter15/status/1638695917514784775?s=20): 

![AlpacaCPPChat](/blog_assets/2023/Alpacca_On_iPhone.png)



## Consumer Electronics Companies and LLMs

With the rapid advances being made for running local LLMs in the open source community, the question is bound to be asked - what is Apple doing? While Apple does have significant ML capabilities and talent, a look at their [ML Jobs Listings](https://www.apple.com/careers/us/machine-learning-and-ai.html) does not indicate they are actively hiring for any LLM projects. The cream of the crop of expertise in LLM’s are at OpenAI, Anthropic, and DeepMind. 

Apple is generally always late to deploying big technological advancements into their products. If and when Apple does develop on-device LLM capabilities, it could either arrive in the form of [CoreML](https://developer.apple.com/documentation/coreml) models that are embedded in individual apps and come with different flavors; such as summarization, sentiment analysis, and text generation. 

Or, alternatively, they could deploy a single LLM as part of an OS update. Apps could then interact with the LLM through system frameworks, [similar to how the Vision SDK works](https://machinelearning.apple.com/research/face-detection). 

This, to me, seems the more likely of the two approaches - both to be sure that each app on a user’s phone does not embed their own, large model bundled with them, and - more importantly - that would allow Apple to have a much more capable version of Siri. 

It’s incredibly unlikely that Apple will ever license the use of LLMs from outside parties like OpenAI or Anthropic. Their strategy is much more the style of either building it all in-house, or to acquire small startups that they integrate into their products. This is what happened with the [Siri acquisition](https://www.forbes.com/sites/parmyolson/2011/10/06/steve-jobs-leaves-a-legacy-in-a-i-with-siri/?sh=4354901d5dd3). 

One consumer electronics company that isn't shy about having a partnership with OpenAI is [Humane](http://hu.ma.ne/). They recently [announced a $100M Series C round of financing](https://hu.ma.ne/media/humane-raises-100m-in-series-c-round-as-it-builds-device-and-services-platform-for-the-ai-era). While still in stealth, they’re widely considered to be a laser-projector based AR wearable. Such a device would surely have major constraints on power, due to the nature of the laser projector. This would probably have a major restriction on running an embedded LLM, at least in their first version.

As part of their fundraising announcement, they did disclose that they’re partnering with several companies, including OpenAI. [My guess](https://twitter.com/nickarner/status/1633519507997335552?s=20) is that they’re going to use a combination of [Whisper](https://openai.com/research/whisper) and [GPT4](https://openai.com/research/gpt-4) for some kind of personal assistant as part of their hardware product. While Whisper has been shown to be quite capable of running on-device, it will probably be some time until a powerful language model will be able to do so in production. 



## Closing Thoughts

I think we’re going to eventually see a demo showing an open source model running on an iPhone as well. I don’t have intuition as to how long it will take until we start seeing these models being baked into production apps, and eventually, into the OS’s themselves. I do anticipate this to eventually happen, opening the door to extremely personal and personalized ML models that we will carry with us in our pockets - having intelligent assistants with us at all times. 

If you’re doing work in the area of local LLMs, particularly ones that may be able to run on phones or other embedded devices, please reach out.



## Further Reading

- [Large language models are having their Stable Diffusion moment](https://simonwillison.net/2023/Mar/11/llama/) 
- [You can now run a GPT-3-level AI model on your laptop, phone, and Raspberry Pi ](https://arstechnica.com/information-technology/2023/03/you-can-now-run-a-gpt-3-level-ai-model-on-your-laptop-phone-and-raspberry-pi/?utm_social-type=owned&utm_medium=social&utm_source=twitter&utm_brand=ars)
- [Inference at the edge](https://github.com/ggerganov/llama.cpp/discussions/205)