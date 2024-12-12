# Hello AdventAI - Day 1️⃣

[![Fork](https://img.shields.io/badge/CLONE%20THIS%20REPO-0088FF?style=for-the-badge)](https://github.com/new?template_owner=toolhouse-community&template_name=adventai-d1&owner=%40me&name=th-adventai-d1&description=AdventAI+Day+1+Fork&visibility=public)

## Table of Contents

- [Hello AdventAI - Day 1️⃣](#hello-adventai---day-1️⃣)
  - [Table of Contents](#table-of-contents)
  - [Introduction to AdventAI](#introduction-to-adventai)
    - [Registration](#registration)
  - [Introduction to Toolhouse](#introduction-to-toolhouse)
    - [Key Features](#key-features)
  - [About the App](#about-the-app)
    - [How to Deploy](#how-to-deploy)
  - [Getting Started](#getting-started)
    - [Fork and Clone the App](#fork-and-clone-the-app)
    - [Pre-requisites](#pre-requisites)
    - [Requirements](#requirements)
    - [Setup Toolhouse](#setup-toolhouse)
    - [Installation](#installation)
    - [Development](#development)
    - [Build a New Use Case](#build-a-new-use-case)
  - [Get Help](#get-help)
  - [License](#license)
  - [Code of Conduct](#code-of-conduct)

## Introduction to AdventAI

AdventAI is an initiative from Toolhouse that blends the festive spirit of Christmas through December with daily challenges designed for AI enthusiasts.

Participants unlock a new app daily that they can use, configure, and distribute or even monetize. Each app bridges the gap between AI and real-time functions with the use of Tools.

> [!NOTE]
> Tools are serverless functions that run on the Toolhouse infrastructure, and empower LLMs with new functionalities like scraping the internet, a particular website, or sending emails.

### Registration

To sign up and receive notifications and guides about upcoming apps throughout the initiative, register on [adventai.dev](https://adventai.dev/).

## Introduction to Toolhouse

Toolhouse is the first cloud platform that allows developers to quickly build, manage, and run AI function calling. We take care of every aspect of connecting AI to the real world, from performance optimization to prompting to integrations with all foundational models, in just three lines of code.

[![Read The Docs](https://img.shields.io/badge/Read%20the%20Documentation-0088ff?style=for-the-badge&logo=read-the-docs)](https://docs.toolhouse.ai/)

### Key Features

✅ Easy Implementation

✅ Universal SDK

✅ Fast and Secure

✅ Built for Privacy

✅ Fantastic Developer Experience

## About the App

On Day 1️⃣, we are unwrapping an AI-powered Meal Planner App, designed to transform the way you approach nutrition, cooking, and meal preparation. The capabilities include but are not limited to:

- **Personalized Nutrition**: Create custom meal plans tailored to your unique health goals, dietary restrictions, and preferences.
- **Intelligent Recipe Generation**: Generates creative, nutritionally balanced recipes that match your taste profile and dietary needs.
- **Smart Grocery Lists**: Automatically creates optimized shopping lists based on your meal plans, reducing food waste and unnecessary purchases.
- **Adaptive Learning**: Continuously improves meal suggestions by learning from your feedback, cooking habits, and nutritional preferences.
- **Daily Email Reminders**: Sends personalized daily email notifications with curated meal suggestions, recipes, and nutritional insights directly to your inbox.

> [!NOTE]
> This app requires the `send_email`, `memory_fetch`, `memory_store`, and `current_time` tools.

### How to Deploy

The app can deploy on any service that can host Python 3.12. For convenience, we made it easy to deploy on Heroku and Render.

[![Deploy to Heroku](https://img.shields.io/badge/deploy%20to%20heroku-%23430098.svg?style=for-the-badge&logo=heroku&logoColor=white)](https://www.heroku.com/deploy) 
[![Deploy to Render](https://img.shields.io/badge/Deploy%20to%20Render-%23000000.svg?style=for-the-badge&logo=render&logoColor=white)](https://render.com/deploy)

## Getting Started

### Fork and Clone the App

- [![Fork](https://img.shields.io/badge/CLONE%20THIS%20REPO-0088FF?style=for-the-badge)](https://github.com/new?template_owner=toolhouse-community&template_name=adventai-d1&owner=%40me&name=th-adventai-d1&description=AdventAI+Day+1+Fork&visibility=public)
  
  Or Create a [Fork](https://github.com/new?template_owner=toolhouse-community&template_name=adventai-d1&owner=%40me&name=th-project&description=AdventAI+Day+1+Fork&visibility=public)

> [!WARNING]
> We recommend forking the app first instead of directly cloning so you can add your own changes on your copy as you go. This also allows you to open a PR easily and showcase your app to other participants!

- Execute the following in your terminal, replacing your username below:

```bash
git clone https://github.com/{USERNAME}/adventai-d1/
cd adventai-d1
```

### Pre-requisites

- Python 3.12.7
- Poetry

### Requirements

- A [Toolhouse](https://toolhouse.ai) account. If you don't have one already, follow this [link](https://join.toolhouse.ai).
- An [Anthropic](https://www.anthropic.com/) account.

### Setup Toolhouse

- Login to your [Toolhouse Account](https://app.toolhouse.ai/) and grab your API Key. We'll be using it in the next steps.

> [!NOTE]
> The tools we'll be using in this project are pre-installed in a Bundle that's already linked to your account and the app.

### Installation

1. **Install Poetry**

   ```bash
   poetry install
   ```

2. **Configure Environment Variables**

   Copy the provided .env.example file to a new .env file:

   ```bash
   cp .env.example .env
   ```

   Fill in the required values in the .env file. Ensure you update all placeholders with the correct values.

3. **Run the Development Server**
   
   To run on http://0.0.0.0:8000:

   ```bash
   hypercorn main:app --bind 0.0.0.0:8000
   ```

   You can use `watchexec` to monitor changes and reload changes automatically.

   ```bash
     watchexec -r -e py "hypercorn main:app --bind 0.0.0.0:8000" 
    ```

   To reload changes you made to the frontend, simply refresh your browser.

### Development

Because your function calling infrastructure and code are hosted on Toolhouse, all apps from AdventAI are actually just a collection of prompts and some business logic.

Each app has this:

- **`main.py`** is the main entry point. It sets up the API routes and serves static content.
- **`api`** contains the backend endpoints:
  - **`api/chat`** streams responses to the LLM you choose
  - **`api/cron`** contains the logic to perform actions every day
  - **`api/config`** will read the configuration for the app you want and serve it back to the frontend
- **`static`** contains the frontend
- **`prompts`** contains the prompts needed by each use casees, as well as the UI configuration


Here is how an app gets loaded:

- Each AdventAI app will be served at `/app/<your_app_name>`, where `<your_app_name>` must match one of the filenames in the `prompts` folder (for example `/app/random-pet-fact`).
- The frontend will call `/app/config`. The backend will look at the referer header to infer the app name. It will then send the appropriate configuration. For example, if you're calling `/app/random-pet-fact`, `/app/config` will open `prompts/random-pet-fact.toml`, covert it to JSON, and serve it to the frontend.
- The frontend will read the configuration and set itself up.

### Build a New Use Case

Most of the work is already done for you! Simply copy `prompts/_template.toml` and fill in the blanks. You can follow one of the other pre-configured configurations for inspiration.

## Get Help

We have a thriving community of developers building with Toolhouse. Join us on Discord and share your questions, concerns and feedback!

[![Discord](https://img.shields.io/badge/Join%20Discord-%235865F2.svg?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/jWDzFNXWS4)

## License

All the apps under AdventAI are available under the MIT license. See the [LICENSE]() file for more information.

## Code of Conduct

AdventAI follows the Contributor Covenant [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) for all incoming contributions.

[Back to Top ⬆️](#hello-adventai---day-1️⃣)
