# n8n Astronomy Workflows

A collection of n8n workflows for astronomy content management, AI chatbots, and Slack integrations.

## Table of Contents

- [Ask an Astronomer Slack Chatbot](#ask-an-astronomer-slack-chatbot)
- [Ingest Astronomy Articles from RSS Feed](#ingest-astronomy-articles-from-rss-feed)
- [RAG Agent-Assisted Chat with Astronomy Articles](#rag-agent-assisted-chat-with-astronomy-articles)
- [RSS Feed Summary to Slack](#rss-feed-summary-to-slack)
- [Web Contact Form to Slack](#web-contact-form-to-slack)

## Ask an Astronomer Slack Chatbot

**File:** `ask-an-astronomer-slack-channel-chatbot.json`

An intelligent Slack chatbot that acts as "AstroGuide," an expert astronomer and cosmic storyteller. The workflow responds to messages in the #ask-an-astronomer Slack channel with scientifically accurate answers while occasionally blending in imaginative speculation and cosmic metaphors.

**Key Features:**
- Uses OpenAI GPT-4.1-mini for responses
- Responds to messages in specific Slack channel
- Balances scientific accuracy with creative storytelling
- Professional yet approachable astronomy expert persona
- Prevents recursion by ignoring messages from SLACKBOT and the app bot

**Workflow Steps:**
1. Slack Chat Trigger monitors the #ask-an-astronomer channel
2. Basic LLM Chain processes the question with expert astronomer persona
3. OpenAI Chat Model generates the response
4. Slack Chat Reply posts the answer back to the channel

[= Back to TOC](#table-of-contents)

## Ingest Astronomy Articles from RSS Feed

**File:** `ingest-astronomy-articles-from-rss-feed-into-vector-store.json`

A comprehensive data ingestion pipeline that fetches astronomy articles from Phys.org RSS feeds, processes them, and stores them in a Pinecone vector database for later retrieval by AI agents.

**Key Features:**
- Fetches from Phys.org space news RSS feed
- Filters for recent articles (last 7 days) and astronomy categories
- Downloads full article content from web pages
- Creates embeddings using OpenAI
- Stores in Pinecone vector database
- Posts article summaries to Slack

**Workflow Steps:**
1. Manual trigger initiates the workflow
2. Get RSS Feed fetches articles from Phys.org
3. Convert to JSON parses the RSS XML
4. Split Out processes individual articles
5. Extract Last Week filters for recent articles
6. Extract Astronomy Articles filters by category (Astronomy, Space Exploration, Planetary Sciences, Astrobiology)
7. Download Article Web Pages fetches full content
8. Extract Article Text extracts clean text from HTML
9. Merge combines RSS metadata with full article text
10. Load and Chunk Data prepares for vector storage
11. Use OpenAI Embeddings creates vector embeddings
12. Store Articles in Pinecone Vector Store saves to database
13. Post Summary to Slack shares articles in Slack channel

[= Back to TOC](#table-of-contents)

## RAG Agent-Assisted Chat with Astronomy Articles

**File:** `rag-agent-assisted-chat-with-astronomy-articles-in-vector-store.json`

A public-facing chat interface powered by a RAG (Retrieval-Augmented Generation) system that uses the stored astronomy articles to answer questions about current space news and developments.

**Key Features:**
- Public chat interface with astronomy theme
- Carl Sagan persona for engaging conversations
- RAG system using Pinecone vector store
- Memory management for conversation context
- Real-time access to ingested astronomy articles

**Workflow Steps:**
1. Chat Trigger provides public web interface
2. AI Agent processes user queries with Carl Sagan persona
3. Pinecone Vector Store retrieves relevant articles (top 10 matches)
4. OpenAI Chat Model generates responses
5. Embeddings OpenAI handles vector search
6. Simple Memory maintains conversation context

[= Back to TOC](#table-of-contents)

## RSS Feed Summary to Slack

**File:** `rss-feed-summary-to-slack.json`

A scheduled workflow that creates daily summaries of astronomy news from RSS feeds and posts them to Slack channels, providing team members with curated astronomy updates.

**Key Features:**
- Scheduled daily execution (11:38 AM)
- Filters for recent articles (last 7 days)
- Sorts by publication date
- Limits to top 10 articles
- Formats as markdown list for Slack
- Manual trigger option for testing

**Workflow Steps:**
1. Schedule Trigger runs daily at 11:38 AM
2. RSS Read fetches from Phys.org astronomy RSS feed
3. Edit Fields normalizes article data
4. Filter keeps only articles from last 7 days
5. Sort arranges by publication date (newest first)
6. Limit restricts to top 10 articles
7. Code formats articles as markdown list
8. Send a message posts summary to Slack channel

[= Back to TOC](#table-of-contents)

## Web Contact Form to Slack

**File:** `web-contact-form-to-slack.json`

A simple contact form integration that captures website visitor inquiries and forwards them to a designated Slack channel for team follow-up.

**Key Features:**
- Public web form with customizable fields
- Name, email, and message fields
- Automatic Slack notifications
- Rich formatting with timestamps
- Professional contact management

**Workflow Steps:**
1. Website Contact Form provides web interface
2. Log Contact in Slack posts formatted message to #contact-form channel

**Form Fields:**
- Name (required)
- Email (optional)
- Message (required, textarea)

[= Back to TOC](#table-of-contents)

---

## Technical Stack

- **n8n**: Workflow automation platform
- **OpenAI**: GPT-4 models and embeddings
- **Pinecone**: Vector database for article storage
- **Slack**: Team communication and notifications
- **Phys.org**: RSS feed source for astronomy articles

## Setup Requirements

1. n8n instance with required node packages
2. OpenAI API credentials
3. Pinecone vector database setup
4. Slack workspace and bot tokens
5. Webhook configurations for triggers

## Required Services

### Free Services (No Credit Card Required)

1. **n8n** - [14-day free trial](https://n8n.io/pricing/)
2. **Slack** - [Choose Pro plan with 30-day trial](https://slack.com/intl/en-nz/pricing/) (sign-up, create a workspace, choose the 30-day trial not the subscription)
3. **Pinecone** - [Free Starter plan](https://www.pinecone.io/pricing/)

### Paid Services

**OpenAI API** - [Sign up here](https://openai.com/api/) by clicking "Log in" and selecting "API Platform" from the dropdown. Then:

1. Click the settings icon (gear wheel) in the top-right
2. Select "Billing" from the left-hand menu to setup a credit card and purchase credits (~$5 USD)
3. Select "API keys" from the left-hand menu and click "+ Create new secret key" to generate your API key