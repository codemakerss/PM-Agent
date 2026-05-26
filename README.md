# PM-Agent

PM-Agent is an early-stage agent project for product management and business analytics work.

The goal is to build an AI agent system that helps Business Analysts, Product Managers, and product teams understand customer needs, identify stakeholder requirements, analyze product documents, and produce high-quality product requirement materials.

## What We Want to Build

We want to build a fully functional BA/PM agent that can support, and eventually automate, large parts of a Business Analyst's work.

The agent should be able to:

- Read product design documents, feature specifications, business wikis, meeting notes, customer feedback, and market materials.
- Extract customer goals, pain points, hidden needs, business constraints, and product opportunities.
- Identify relevant stakeholders and analyze their interests, conflicts, risks, and success criteria.
- Generate PRDs, BRDs, user stories, acceptance criteria, feature matrices, launch checklists, and stakeholder analysis documents.
- Review product requirements for missing context, ambiguity, feasibility issues, and implementation risks.
- Ask clarification questions when the input is incomplete or inconsistent.
- Help teams turn rough ideas and scattered documentation into structured, actionable product decisions.

## Project Direction

This project is not intended to be a simple PRD generator.

The long-term direction is to build an agentic BA/PM workflow that can:

- Understand business context.
- Reason about customer and stakeholder needs.
- Compare trade-offs between user value, business value, technical feasibility, cost, and risk.
- Produce traceable requirement documents with sources and assumptions.
- Support real product delivery workflows from discovery to launch.

## Current Repository Contents

This repository currently contains:

- `plan.md`: Chinese project planning document for building the BA-Agent system.
- `业界prd/模版`: 30 curated PRD and product requirement templates based on public industry practices.
- `业界prd/样例`: 30 extended example requirement documents based on real public product features.

These materials are preparation assets for designing the future agent workflow, prompt system, evaluation framework, and product requirement generation pipeline.

## Planned Capabilities

Future implementation may include:

- Document ingestion and parsing.
- Retrieval-augmented generation over product and business documents.
- Multi-agent workflows for customer analysis, stakeholder analysis, PRD generation, feasibility review, and requirement QA.
- Evaluation harnesses for testing requirement quality.
- Prompt versioning and regression testing.
- Web interface for uploading documents and generating structured outputs.
- Integrations with tools such as Jira, Confluence, Notion, Slack, or other product management systems.

## Why This Matters

Business Analysts and Product Managers often spend large amounts of time reading fragmented documents, clarifying ambiguous requirements, aligning stakeholders, and writing structured product materials.

PM-Agent aims to reduce that manual workload while improving requirement quality, traceability, and decision clarity.

The project will start as a copilot for BA/PM work, then gradually evolve toward a more autonomous agent that can handle repeatable analysis and documentation tasks under human review.
