+++
date = '2025-03-13T09:52:26Z'
draft = false
title = 'Smarter News Consumption with FreshRSS and AI'
+++
## Introduction
Lately, I’ve been reflecting on how I consume news. In an era dominated by data-driven algorithms, information is a valuable commodity, while attention and mindshare are finite resources. Filtering out the signal from the noise—distinguishing what’s truly useful from the deluge of content flooding social media, newsletters, and feeds—requires effort. Too often, low-value content slips through, cluttering our reading experience.

By "low-value content," I mean:
1. Sales pitches for products I don’t want or need.
2. Superficial, highly editorialized thought pieces with little original insight.
3. Clickbait articles designed to provoke emotional engagement rather than inform.

This kind of content often sneaks in alongside more valuable articles, making it tricky to avoid. While [Ground News](https://ground.news/) helps with political bias tracking, I wanted something more tailored to my trusted information sources—without relying on someone else's curated feeds.

Over the years, I’ve carefully built a collection of high-quality news feeds, migrating from Google Reader to Feedly and now to my self-hosted FreshRSS instance. Owning my data and controlling my news flow is important to me, but I also want to enhance how I interact with that information.

## The Problem: Staying Updated Without Overload
One challenge I face with FreshRSS is that I don't check it frequently enough. During the day, I’m mostly focused on work, responding to notifications, with occasional evening catch-ups. Ideally, I’d like a daily roundup of relevant news—without constant interruptions from notifications.

I explored existing tools, but most either required me to overhaul my feed management or migrate to a different system. Others were hosted solutions that meant giving up control over my data. None quite fit my needs.

So, I decided to build my own FreshRSS plugin.

## Diving into PHP (Reluctantly)
It’s been a long time since I’ve touched PHP. My memories of it mostly involve SQL injections and the perils of weak typing. But FreshRSS and its extensions are written in PHP, so I had to roll up my sleeves and dive in. Fortunately, FreshRSS and PHP are old enough that large language models (LLMs) are well-trained on it, making AI-assisted development viable.

I started an AI chat session with this prompt:

```
I am writing a plugin for FreshRSS. The relevant documentation is:

<SNIP>

I would like to:
* Automatically classify every article with tags.
* Generate an automatic roundup summary each day, updating as new articles arrive.
* Retitle every article with key information.
* Insert an executive summary at the start of each article.

How can I accomplish this?
```

The AI provided a solid first attempt, leveraging FreshRSS’s plugin documentation. Of course, it hallucinated a few non-existent class functions, but finding functional equivalents was straightforward. I took on the role of reviewer, refining the code iteratively rather than blindly following its output. This approach—guiding AI suggestions with hands-on oversight—proved much more efficient than simply pasting errors back into the model and hoping for a better response.

Another valuable resource was [reply2future/xExtension-NewsAssistant](https://github.com/reply2future/xExtension-NewsAssistant), an open-source extension with some overlapping features. By pasting snippets of its code into the AI chat, I could provide a working reference for the model to learn from.

## The Result: FreshRSS AI Assistant
The result of this effort is [cvlc/freshrss-ai-assistant](https://github.com/cvlc/freshrss-ai-assistant)!

This extension delivers on all the features I set out to build:
- **Automatic tagging**: Articles are classified based on themes, sentiment, or custom rules.
- **Daily roundup summaries**: A consolidated digest updates dynamically with new content.
- **Smart retitling**: Article headlines are rewritten for clarity and key information.
- **Inline executive summaries**: A brief synopsis appears at the start of each article.

The plugin is configurable, allowing users to customize prompts and choose their preferred AI model. By default, it uses `gpt-4o-mini` as a cost-effective option with a neutral, de-editorialized summarization approach. However, the possibilities are wide open, including:
- Summaries from a specific point of view (e.g., as a financial analyst, Otto von Bismarck, or even a pirate).
- Thematic tagging based on length, quality, or sentiment.
- Bold, tabloid-style headlines for added drama.

It connects to the OpenAI API, but for those seeking more control, I recommend using [LiteLLM](https://www.litellm.ai/) as a self-hosted proxy for alternative models or cost tracking. Simply adjust the base URLs and API keys accordingly.

For usability, I added an option to mark summarized articles as "read" when clicking the Summarize button. This allows for quick skimming while keeping feeds tidy—if something catches your eye, you can always search for the full article later.

## Next Steps
If time permits, I’d love to expand this further:
- **Newsletter support**: Integrate with tools like [Kill the Newsletter!](https://github.com/leafac/kill-the-newsletter) or [Atomail](https://github.com/remko/atomail) for seamless newsletter-to-RSS conversion.
- **Event-driven delivery**: Use [Windmill](https://www.windmill.dev/) or similar tools for scheduled summaries, sent via smart speaker or other channels.
- **API-based integrations**: The plugin already includes an API for fetching summaries programmatically—this could be extended to create a single consolidated newsletter or integrate with home automation systems.

For now, though, I’m happy to have a tool that makes my news consumption smarter, more efficient, and under my control. If you’re using FreshRSS and want to streamline your feeds, check out [cvlc/freshrss-ai-assistant](https://github.com/cvlc/freshrss-ai-assistant) and let me know what you think!
