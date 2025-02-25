---
title: Arkhn’s Data Platform Goes Agentic
author: Olivier Girardot
type: post
date: 2025-02-24T9:40:00+00:00
lastmod: 2025-02-25T11:40:00+00:00
description: Secure AI Access to Healthcare Data with MCP Support
image: /posts/2025-02-25-mcp/images/cursor-arkhn.png
categories:
  - AI
  - OSS
  - TechZone
tags:
  - llm
  - agent
  - healthcare
  - arkhn

---

I have been the CTO of Arkhn for two years now, during this time we've been building the best data platform to empower healthcare providers, make better decisions and improve patient outcomes.
<!--more-->

Today I am proud to announce that we are crossing a step forward in our journey by announcing the support of the [Model Context Protocol](https://modelcontextprotocol.io/) with a server implementation that will allow LLMs and Agents to access all the data in our platform without any compromise on security or sovereignty giving the Hospitals new tools to interact and gain insights to improve patient care.

# What is the Model Context Protocol ?

The [Model Context Protocol](https://modelcontextprotocol.io/) is a new open source protocol designed by [Anthropic](https://www.anthropic.com/) to standardize the way Agents/LLMs can access and interact with external tools and data sources. This new protocol defines three key concepts exposed by the MCP Servers :

* *Resources & Resources Templates* : a way to expose data sources to the LLM parametrized or not to add context to the LLM's response
* *Tools* : a way to expose APIs to the LLM - it can be used to expose expensive computations, mutations of state or calls to external services
* *Prompts* : reusable prompts template & workflows designed to ease the interactions with the users and LLMs

MCP Servers can be exposed using two main transports :
* *HTTP/REST* - using [Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) to stream the responses
* *STDIO* - using STDIN/STDOUT for communication between the local server and the client

We support both protocols the stdio one being mainly used for local development and debug - Anthropic released the [MCP Inspector](https://github.com/modelcontextprotocol/inspector) to allow for easy testing while developing.

# Arkhn's data platform

[Arkhn Data Platform](https://www.arkhn.com/en/solutions/offer) is a modern data platform deployed locally within the hospitals' infrastructure or within our own sovereign [HDS](https://esante.gouv.fr/labels-certifications/hds/certification-des-hebergeurs-de-donnees-de-sante) / ISO27001 certified data center in France.

The platform itself is built on top of multiple OpenSource technologies and makes no compromise on security of the data it processes, with encryption at rest and in transit, with column level encryption as well and a strong access layer. We even developed secure Data Clean Rooms to access and process data without any risk of leaks or re-identification of the patients studied.

The field of Modern Data platforms is dominated by Cloud providers nowadays and our goal was to provide a best of breed experience for the developers and analysts working on critical data for the hospitals. We decided to go one step further and make the healthcare providers analysts benefit from the next revolution coming to the field - LLMs, Agents and the Agentic paradigm.

The emergence of Open Source models and the computational power requirements getting lower and lower every day makes it obvious that our clients will be able to run these models locally in the foreseable future. That is why as the natural Data access layer we want to help our clients reap the benefits of the LLM revolution as soon as possible.

# Why now ?

As I said we do not compromise with the security or sovereignty of the Data we protect within the hospitals. While we have been developping for quite some time now products built on LLMs, we have not given direct access to our data to LLMs preferring to control the datasets and expose APIs controlling who sees what and when.

We are now ready to take a step back and give the power to the users to interact with LLMs using the data they need because of several key changes :

* The introduction of the [MCP](https://modelcontextprotocol.io/) by Anthropic to standardize the way agents/LLMs can access and interact with external tools and data sources was a great news for us as we have always promoted interoperability formats like FHIR and OMOP
* But more recently the [new draft specification of the MCP protocol](https://spec.modelcontextprotocol.io/specification/draft/basic/authorization/) to add [support for fine-grained access control](https://github.com/modelcontextprotocol/typescript-sdk/releases/tag/1.6.0) was a game changer for us as it is crucial for us to give access to data knowing which user called and what data they can access.
* Finally the growing support of the MCP protocol amongst the GenAI community for example in the LangChain ecosystem with the [LangChain MCP Adapter](https://github.com/langchain-ai/langchain-mcp-adapters), [LangChain4j MCP](https://docs.langchain4j.dev/tutorials/mcp/), [LangGraph](https://www.youtube.com/watch?v=OX89LkTvNKQ) or for the end-user with [Claude Desktop support](https://modelcontextprotocol.io/quickstart/user), [CursorAI support](https://docs.cursor.com/context/model-context-protocol) and [Cline](https://github.com/cline/mcp-marketplace) to name a few.

# MCP integrations examples
We decided to expose the Data Platform using both Tools & Resource Templates because most of the products supporting MCP Servers are not yet supporting Resources & Resources Templates as a rule.

For example, Claude Desktop integration while being released by Anthropic supports Tools & Prompts but not [Resources](https://modelcontextprotocol.io/docs/concepts/resources) or [Resources Templates](https://modelcontextprotocol.io/docs/concepts/resources#resource-templates) yet, but as you'll see in the demo below it is just as powerful!

{{< figure src="images/claude-arkhn.png" 
    alt="Claude Desktop - Arkhn MCP Server integration example" 
    title="Claude Desktop <=> Arkhn" 
    loading="lazy"
    link="images/claude-arkhn.png"
    >}}

On the other hand, Cline is a VSCode extension that supports both Resources/Resources Templates & Tools :
  {{< figure src="images/cline-arkhn.png" 
    alt="Cline - Arkhn MCP Server integration example" 
    title="Cline <=> Arkhn" 
    loading="lazy"
    link="images/cline-arkhn.png"
    >}}

Finally Cursor.ai relies on Tools just as much for now.
{{< figure src="images/cursor-arkhn.png" 
      alt="Cursor.ai - Arkhn MCP Server integration example" 
      title="Cursor.ai <=> Arkhn" 
      loading="lazy"
      link="images/cursor-arkhn.png"
      >}}

<style>
.image-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    gap: 1rem;
    margin: 1rem 0;
}

.image-grid img {
    width: 100%;
    height: auto;
    border-radius: 2px;
}
</style>

# Demo in action
Now how does it work in practice, the LLM will ask your permission to call the tools you have access to, according to the prompt you provided, here's a quick demo using Claude Desktop integration with a local Arkhn MCP server using fake data:

<video src="videos/mcp-arkhn-demo.webm" width="100%" controls></video>

# To go further 
I'd recommend you to check the great post detailing the [integration of the MCP protocol in LangChain4j](https://www.linkedin.com/pulse/integrating-mcp-model-context-protocol-langchain4j-access-goncalves-pedze/) by [Antonio Goncalves](https://www.linkedin.com/in/agoncal/) and this example of [integration with LangGraph Agents](https://www.youtube.com/watch?v=OX89LkTvNKQ) to get a better understanding of how to use the MCP protocol from LangChain.
