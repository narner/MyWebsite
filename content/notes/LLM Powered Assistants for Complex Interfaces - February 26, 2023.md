---
title: "LLM Powered Assistants for Complex Interfaces"
date: "2023-02-26"

---



### Text Interfaces Aren't Enough

The last few months of LLM development have been staggering. It’s been amazing to see how powerful chat-based interfaces, heralded by [OpenAI’s ChatGPT](https://openai.com/blog/chatgpt/), can be. It’s inspired some to postulate that [text is the universal interface](https://scale.com/blog/text-universal-interface), and for others to say *that the humble text box is going to be the most important interface component there is.* 

This enthusiasm, understandable though it may be, risks creating the perception that any and all user interfaces can be more or less replaced with a chatbot or text-box based interface. 

This could not be further from the case. 

Many “expert” interfaces are extremely complex. They often have to be - if they’re focused on facilitating a complex task or workflow, such as CAD design or 3D modelling, the interface will have to be able to accommodate the user to facilitate the tasks at hand. This is, of course, not to say that these interfaces aren’t or can’t be well-designed - it’s that they are necessarily complex due to the fact that what they’re designed for is complex. 

![Autocad UI](/blog_assets/2023/autocad-ui.jpg)

*https://www.autodesk.com/products/autocad/features*

![Blender UI](/blog_assets/2023/blender-ui.jpg)

*https://www.fabrizioduroni.it/2018/01/31/blender-tutorial-1-user-interface/*

### LLMs for New Interface Design

This complexity can make it difficult for both domain novices and experts alike to learn how to use the interface. LLMs can help reduce this barrier by being leveraged to prove assistance to the user if they’re trying to accomplish something, but don’t exactly know how to navigate the interface.

The user could tell the program what they’re trying to do via a text or voice interface, or perhaps, the program may be able to infer the user's intent or goals based on what actions they’ve taken so far. 

Modern GUI apps are slowly starting to add in more features for assisting users with navigating the space of available commands and actions via [command palettes](https://capiche.com/e/consumer-dev-tools-command-palette); popularised in software iA Writer and [Superhuman](https://blog.superhuman.com/how-to-build-a-remarkable-command-palette/).

![Command Palette](/blog_assets/2023/Command-Palette.png)

*From the Superhuman blog post linked above*

Command palettes are helpful for finding discrete commands through a fuzzy search that a user wants to accomplish that they may not be familiar with. But, for executing a sequence of tasks as part of a complex workflow, LLM powered interfaces afford a richer opportunity for learning and using complex software. 

The program could walk them through the task they’re trying to accomplish by highlighting and selecting the interface elements in the correct order to accomplish the task, along with explanations provided. For some tasks, users could have the assistants step through these tasks autonomously. 

Expert interfaces that take advantage of LLMs may end up looking like they currently do - again, complex tasks require complex interfaces. However, it may be easier and faster for users to learn how to use these interfaces thanks to built-in LLM-powered assistants. This will help them to get into flow faster, improving their productivity and feeling of satisfaction when using this complex software. 

While I do not dispute that LLM based chat interfaces are incredibly powerful, I do not think they will necessarily wipe out all other forms of interface design. If anything, I think that they have a powerful role to play in augmenting any type of interface, particularly ones that are complex - by making them easier to use and navigate for new users. 



### Clippy 2.0 and Interface Agents

If you think this all sounds vaguely like Clippy, Microsoft’s famous Word assistant, you’d be correct. These types of assistants would live inside various applications like Clippy lived inside word, knowing the context of what you’re currently doing and offering assistance as needed. 

![Clippy](/blog_assets/2023/clippy.jpg)

*https://www.theverge.com/2019/3/22/18276923/microsoft-clippy-microsoft-teams-stickers-removal* 

However, unlike Clippy, these new types of assistant would be able to act on the interface directly. These actions will be made in accordance to the goals of the person using them, but each discrete action taken by the assistant on the interface will not be done according to explicit human actions - the goals are directed by he human user, but the steps to achieve those goals are unknown to the user, which is why they’re engaging with the assistant in the first place. 

[Henry Lieberman defines these types of assistants as interface agents](https://web.media.mit.edu/~lieber/Lieberary/Letizia/AIA/AIA.html) - “a program that can also affect the objects in a direct manipulation interface, but without explicit instruction from the user”. These agents can be autonomous, too; if the user is able to “directly observe autonomous actions of the agent and the agent” and that the agent is able to “observe actions taken autonomously by the user in the interface”. 

At first, these assistants would have to be connected to the internet in order to interface with powerful, cloud-based LLM’s. Developers of these types of applications will need to treat privacy as paramount concern - finding ways to develop a powerful LLM powered application that only sends the minimum amount of information required for the application to be useful.

Some recent commercial examples of LLM powered assistants that can act directly on the interface include [Adept](http://adept.ai/), [Lasso](http://getlassoai.com/), and [Natbot](https://github.com/nat/natbot). These assistants are all centered around the web browser, but, I expect that future versions of software like Autocad, Photoshop, Blender, etc will all have equivelant interfaces built in. Adobe has been [publishing research on ntelligent assistants](https://research.adobe.com/research/intelligent-agents-assistants/) already, and would be well positioned to integrate them into their Creative Suite. Eric Jang's [tweet](https://twitter.com/ericjang11/status/1627839912555995136) provides a glimpse of what that could look like at the start:

 ![Eric Jang Tweet](/blog_assets/2023/eric-jang-tweet.png)



### Closing Thoughts

Complex tasks and domains require complex interfaces. Expert users require these complex interfaces in order to do the work they need to do. LLMs, for all the power they afford text based interfaces, are not going to make the need for these complex applications and interfaces disappear. They can, however, make it easier for novice users or experts unfamiliar with a particular new piece of software to get up to speed with learning how to use it quicker via built-in assistants that can help them navigate and use these interfaces faster than they could learn on their own.

Looking to the future of software development, Lieberman states in [Autonomous Interface Agents](https://web.media.mit.edu/~lieber/Lieberary/Letizia/AIA/AIA.html): 

*“Agents will observe what human users do when they interact with interfaces, and provide assistance by manipulating the interface themselves, while the user is thinking or performing other operations. Increasingly, applications will be designed to be operated by users and their agents, simultaneously.”*

Developments in LLMs will lead to increasing adoption across software development in a huge swath of industries and sectors. Leveraging them to create embedded assistants in complex applications will allow for novices to learn complex software faster, and unlock even greater productivity gains amongst expert users.

***********************************************************************************************************************************************************************************************************************************************************************

If you’re working on any LLM - powered application assistants, or ways to scale LLM models to to run locally on laptops and mobile devices, please reach out to me! 





