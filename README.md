> **AI-Generated Content Disclaimer**
> 
> This document was created using Anthropic Claude Sonnet 4 and may contain factual errors or outdated information. While care has been taken to minimize inaccuracies and verify information against official sources, all facts, pricing, and technical details should be independently verified using the source links provided throughout this document.
> 
> Please check official platform websites and documentation before making business decisions based on this content.

# n8n Astronomy Workflows

Notes and resources from n8n talk @ Christchurch AI Meetup. These workflows serve as practical demonstrations of n8n's automation capabilities, modern AI integration patterns, and real-world data processing scenarios. They provide some example workflows that show the integration of n8n, Slack, and Pinecone, in the context of extracting and using information from the [Phys.org](https://phys.org/) website. In addition to this, a simple webform workflow is provided as an introductory example, and a chatbot app in Slack to show a basic LLM workflow.

## Table of Contents

- [How to Import Workflows into n8n](#how-to-import-workflows-into-n8n)
- [Ask an Astronomer Slack Chatbot](#ask-an-astronomer-slack-chatbot)
- [Ingest Astronomy Articles from RSS Feed](#ingest-astronomy-articles-from-rss-feed)
- [RAG Agent-Assisted Chat with Astronomy Articles](#rag-agent-assisted-chat-with-astronomy-articles)
- [RSS Feed Summary to Slack](#rss-feed-summary-to-slack)
- [Web Contact Form to Slack](#web-contact-form-to-slack)

## About These Demonstration Workflows

The demonstration workflows were built around n8n and Slack, with the addition of Pinecone as a vector database. As there are a large number of integrations available in n8n, I chose Slack - a familiar platform - to demonstrate some of the key automation concepts and capabilities.

Pinecone was included to demonstrate how vector stores work and how to use them as part of a RAG (Retrieval-Augmented Generation) system, showcasing modern AI-powered workflow capabilities.

Because I am passionate about astronomy and astrophysics, I built the demonstration workflows around extracting and using data from the [Phys.org](https://phys.org/) website, creating practical examples that combine my interests with real-world automation scenarios.

## How to Import Workflows into n8n

To use these workflows in your n8n instance, follow these steps:

1. **Create a new workflow** - In your n8n dashboard, click "Create Workflow"

2. **Copy the JSON file** - Click on any workflow JSON file link below (e.g., [`ask-an-astronomer-slack-channel-chatbot.json`](./ask-an-astronomer-slack-channel-chatbot.json))

3. **Get the raw content** - On the GitHub file page, click the "Copy raw file" button (📋 icon) located just to the right of the "Raw" button

4. **Paste into n8n** - Go back to your new n8n workflow, click on the blank canvas, and paste:
   - **Windows**: Ctrl+V
   - **Mac**: Command+V (⌘+V)
   - **Linux**: Ctrl+V

The workflow will automatically populate with all nodes and connections from the JSON file.

5. **Configure credentials** - You will need to create your own credentials in n8n for:
   - **Slack** - For Slack integrations and bot functionality
   - **OpenAI** - For AI chat models and embeddings
   - **Pinecone** - For vector database operations

   If you use the same credential names as already referenced in the workflow nodes, they should automatically populate (though this isn't guaranteed). Otherwise, you'll need to manually assign your credentials to each relevant node.

## Ask an Astronomer Slack Chatbot

**File:** [`ask-an-astronomer-slack-channel-chatbot.json`](./ask-an-astronomer-slack-channel-chatbot.json)

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

[Back to TOC](#table-of-contents)

## Ingest Astronomy Articles from RSS Feed

**File:** [`ingest-astronomy-articles-from-rss-feed-into-vector-store.json`](./ingest-astronomy-articles-from-rss-feed-into-vector-store.json)

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

[Back to TOC](#table-of-contents)

## RAG Agent-Assisted Chat with Astronomy Articles

**File:** [`rag-agent-assisted-chat-with-astronomy-articles-in-vector-store.json`](./rag-agent-assisted-chat-with-astronomy-articles-in-vector-store.json)

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

[Back to TOC](#table-of-contents)

## RSS Feed Summary to Slack

**File:** [`rss-feed-summary-to-slack.json`](./rss-feed-summary-to-slack.json)

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

[Back to TOC](#table-of-contents)

## Web Contact Form to Slack

**File:** [`web-contact-form-to-slack.json`](./web-contact-form-to-slack.json)

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

[Back to TOC](#table-of-contents)

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

## Configuration Notes

### Important Customizations Required

**Slack Setup**: 
- Create the following channels in your Slack workspace:
  - `#ask-an-astronomer` (for the chatbot workflow)
  - `#contact-form` (for web contact form notifications)
  - `#n8n-test` (for article ingestion notifications)
  - `#all-astrotope-dev` (for RSS feed summaries)
- Configure channel IDs in each workflow's Slack nodes
- Set up Slack credentials and bot permissions

**n8n Documentation**:
- [Slack Trigger Node Setup](https://docs.n8n.io/integrations/builtin/trigger-nodes/n8n-nodes-base.slacktrigger/)
- [Slack Credentials Configuration](https://docs.n8n.io/integrations/builtin/credentials/slack/)
- [Form Trigger Node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.formtrigger/) - Creates web-based forms with customizable fields, supports basic authentication, provides separate test and production URLs, and can be configured with custom form paths, titles, and descriptions. Forms can be embedded in websites or used as standalone hosted pages.
- [Chat Trigger Node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-langchain.chattrigger/) - Creates public chat interfaces for AI agent interactions, enables dynamic chat generation for workflow automation and user interaction. Chat interfaces can be embedded in websites or accessed as standalone hosted pages.

**Pinecone Vector Store**: 
- Create a new index named `astro-articles`
- Use OpenAI "text-embedding-3-small" model with dimension 1536
- Leave all other settings unchanged
- The RAG chat workflow depends on the ingestion workflow - run ingestion first to populate the vector store

**OpenAI Models Used**:
- Ask an Astronomer: GPT-4.1-mini
- RAG Agent Chat: GPT-4o-mini
- All workflows: OpenAI embeddings with 1536 dimensions

### Workflow Dependencies

1. **Run ingestion workflow first**: The "Ingest Astronomy Articles" workflow must be executed before the "RAG Agent-Assisted Chat" workflow will function properly
2. **Vector store population**: The chat agent relies on articles being stored in the Pinecone vector database
3. **Daily scheduling**: The RSS summary workflow runs daily at 11:38 AM - adjust timing as needed

### Technical Details

- **Article filtering**: Ingestion workflow filters for articles from last 7 days in categories: Astronomy, Space Exploration, Planetary Sciences, Astrobiology
- **Batch processing**: Article downloads are processed one at a time to avoid rate limiting
- **Public webhooks**: Contact form and RAG chat have public-facing webhook URLs
- **Bot exclusions**: Slack chatbot ignores messages from SLACKBOT and the app bot to prevent recursion

### Data Sources

**Phys.org RSS Feeds**:
- Main RSS feeds page: [https://phys.org/feeds/](https://phys.org/feeds/)
- Space news RSS feed used by workflows: [https://phys.org/rss-feed/space-news/](https://phys.org/rss-feed/space-news/)
- Example article format: [Webb fresh take on a classic deep field](https://phys.org/news/2025-08-image-webb-fresh-classic-deep.html)
- Sample RSS feed data: `phys-org-space-news-example-feed.rss` (included in repository)

## Slack Integration Setup

### 1. Slack App Strategy

**Recommended Approach: Single App**
- Use one Slack app for all workflows for simplicity
- Easier credential management in n8n
- Streamlined setup and maintenance

**Advanced Approach: Separate Apps**
- Create individual apps for each workflow type
- **Benefits**: Minimal permissions per app, better security isolation, easier permission auditing
- **Trade-offs**: More complex setup, multiple credential sets to manage
- Consider for production environments with strict security requirements

### 2. Create Your Slack App

1. Go to [api.slack.com/apps](https://api.slack.com/apps)
2. Click "Create New App" → "From scratch"
3. Enter app name (e.g., "Astronomy Workflows") and select your workspace
4. Click "Create App"

### 3. Configure OAuth Scopes

Navigate to **OAuth & Permissions** in your Slack app settings.

**Essential Bot Token Scopes** (required for all workflows + n8n GUI):
- `chat:write` - Send messages to channels
- `channels:read` - List and view basic channel information (enables n8n GUI channel selection)
- `channels:history` - View messages in channels (required for chatbot workflow and n8n event listening)
- `users:read` - View user information (enables n8n GUI to show usernames/IDs)

**Additional Bot Token Scopes** (if using advanced features):
- `im:read` - View direct message info (may be needed for n8n GUI functionality)
- `files:read` - If workflows need to access shared files

**User Token Scopes** (only if needed for advanced workspace management):
- Generally not required for these astronomy workflows
- Consider only if you need elevated workspace permissions

### 4. Event Subscriptions (Required for Chatbot Workflow)

1. Go to **Event Subscriptions** in your Slack app settings
2. Turn on "Enable Events"
3. **Get Request URL from n8n**:
   - Open your chatbot workflow in n8n
   - Click on the "Slack Chat Trigger" node
   - Copy the webhook URL (format: `https://[your-instance].app.n8n.cloud/webhook/[webhook-id]/webhook`)
4. Paste the webhook URL into the "Request URL" field
5. Slack will verify the URL (n8n handles this automatically)
6. Subscribe to **Bot Events**:
   - `message.channels` - Listen for messages in public channels

### 5. Install App and Get Tokens

1. Go to **OAuth & Permissions**
2. Click "Install to Workspace"
3. Review permissions and click "Allow"
4. Copy the **Bot User OAuth Token** (starts with `xoxb-`)
5. If you configured User Token Scopes, also copy the **User OAuth Token** (starts with `xoxp-`)

### 6. Set Up n8n Credentials

1. In n8n, go to **Settings** → **Credentials**
2. Click "Create Credential" → "Slack OAuth2 API"
3. Enter your **Bot User OAuth Token** (`xoxb-...`)
4. Optionally enter **User OAuth Token** if you have one
5. Test the connection
6. Save the credential with a descriptive name

**Verification**: Once credentials are saved, n8n should be able to:
- List your Slack channels by name in workflow nodes
- Show user IDs and usernames in relevant fields
- Access channel information for configuration

### 7. Configure Workflows

**Channel Setup**:
1. Create these channels in your Slack workspace:
   - `#ask-an-astronomer` - For the AI chatbot
   - `#contact-form` - For contact form notifications  
   - `#n8n-test` - For article ingestion notifications
   - `#all-astrotope-dev` - For RSS feed summaries

2. **Invite your bot to channels**:
   - Type `/invite @YourBotName` in each channel
   - Or use channel settings → Integrations → Add apps

**n8n Workflow Configuration**:
- Import the workflow JSON files into n8n
- Update Slack credential references to use your new credential
- Select appropriate channels using the n8n GUI dropdowns (no manual ID editing needed)
- Test each workflow individually

### 8. Webhook URL Management

**Getting Webhook URLs**:
- Each trigger node in n8n generates its own webhook URL
- URLs follow pattern: `https://[instance].app.n8n.cloud/webhook/[unique-id]/webhook`
- **Production vs Test**: n8n provides different URLs for test and production modes

**Using Webhook URLs**:
- **Slack Trigger**: Copy URL to Slack app Event Subscriptions
- **Form Trigger**: Use URL for website integration or standalone form
- **Chat Trigger**: Use URL for public chat interface

### 9. Security Considerations

**Single App Benefits**:
- Simplified management and setup
- One set of credentials to maintain
- Good for development and small teams

**Separate Apps Benefits** (Advanced):
- **Chatbot App**: Only needs `channels:history`, `chat:write`, `users:read`
- **Notification App**: Only needs `chat:write`, `channels:read`
- **Form App**: Only needs `chat:write` for notifications
- Better security isolation and permission auditing

### 10. Troubleshooting

**n8n GUI Issues**:
- **Can't see channels**: Check `channels:read` scope is granted
- **Can't see users**: Check `users:read` scope is granted
- **Events not working**: Verify webhook URL and `channels:history` scope

**Webhook Problems**:
- **Verification failed**: Ensure n8n workflow is active and webhook URL is correct
- **Events not received**: Check bot is invited to relevant channels
- **Wrong URL format**: Verify you're using the full webhook URL from n8n

**Permission Errors**:
- **Bot not in channel**: Use `/invite @BotName` in the channel
- **Insufficient scopes**: Review and add missing OAuth scopes
- **Token expired**: Regenerate tokens in Slack app settings

**Message Formatting Issues**:
- Test Block Kit JSON using [Slack's Block Kit Builder](https://app.slack.com/block-kit-builder)
- Verify proper escaping of special characters in message content

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