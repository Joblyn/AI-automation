# AI-automation

AI automation workflows with n8n

## Overview

This repository contains automation workflows and resources for leveraging AI in business processes using [n8n](https://n8n.io/). The workflows are designed to streamline content creation and other repetitive tasks, integrating AI models and external services.

## Folder Structure

- `linkedinContentCreator/`
  - `LinkedIn_Content_Creator_WorkFlow.json`: n8n workflow for automating LinkedIn content creation.
  - `PromptTemplate.md`: Templates and examples for AI prompt engineering.
  - `WorkflowCreationGuide.md`: Step-by-step guide for creating and customizing workflows.

## Getting Started

1. **Clone the repository:**
	```sh
	git clone https://github.com/Joblyn/AI-automation.git
	cd AI-automation
	```

2. **Import Workflows into n8n:**
	- Open your n8n instance.
	- Import the `.json` workflow files from the `linkedinContentCreator` folder.

3. **Configure Environment:**
	- Set up required API keys and environment variables in n8n as needed by the workflows.

## Usage

- Use the provided workflow files to automate LinkedIn content creation or as templates for your own automation needs.
- Refer to `PromptTemplate.md` for crafting effective AI prompts.
- Follow `WorkflowCreationGuide.md` for instructions on customizing and deploying workflows.
