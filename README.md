# BioHackathon Europe 2026 · Research Software Quality

Entry point for **Project 8** of the [BioHackathon Europe 2026](https://github.com/elixir-europe/biohackathon-projects-2026/blob/main/8.md): *Advancing Research Software Quality across ELIXIR: Sustainable, Reproducible, and Interoperable Life-Science Software*.

The project reviews, maps and prioritises research software quality indicators that matter to ELIXIR and related initiatives, such as [EVERSE](https://everse.software/), [B1MGplus](https://b1mg-project.eu/) and [ELIXIR-STEERS](https://elixir-europe.org/about-us/how-funded/eu-projects/steers). We also turn those indicators into automated checks in [resqui](https://github.com/EVERSE-ResearchSoftware/QualityPipelines), the EVERSE quality pipeline.

This repository collects the resources and instructions you need to contribute during the hackathon, with a focus on adding new tools and indicators to [`resqui`](https://github.com/EVERSE-ResearchSoftware/QualityPipelines).

**Co-leads:** Laura Portell-Silva, Shoaib Sufi, Styliani-Christina Fragkouli

## Code of Conduct

The BioHackathon follows the [ELIXIR Code of Conduct](https://elixir-europe.org/events/code-of-conduct). It applies to both in-person and virtual interactions in this project. See the [BioHackathon registration page](https://biohackathon-europe.org/attend/registration#code-of-conduct) for details.

## Resources

### Project

| Resource | Description |
|---|---|
| [Project page](https://github.com/elixir-europe/biohackathon-projects-2026/blob/main/8.md) | Official description of Project 8 in the BioHackathon projects repository. |
| [Slack channel](https://biohackeu.slack.com/archives/C0BBMFB0XEY) | Main communication channel for the project (BioHackathon Europe Slack workspace). |
| [Project overview](https://docs.google.com/document/d/1zGAxM7-GvqGruW7ceickFvOGYLyjlxloTAKQlunU8SM/edit?usp=sharing) | Participation tracks and how to join. |

### EVERSE

| Resource | Description |
|---|---|
| [EVERSE indicators](https://everse.software/indicators/website/indicators.html) | Catalogue of measurable research software quality indicators. |
| [RSQKit](https://everse.software/RSQKit/tasks) | Research Software Quality Kit: community best practices and guidelines. |
| [Three-tier view](https://everse.software/RSQKit/three_tier_view) | Classification of research software into three tiers (analysis code, prototype tools, research software infrastructure). |
| [TechRadar](https://everse.software/TechRadar/) | Catalogue of tools that measure or improve quality indicators. The full catalogue is also available as [JSON](https://everse.software/TechRadar/api/tools.json). |
| [Tool selection criteria](https://github.com/EVERSE-ResearchSoftware/RSQKit/blob/main/TOOLS_SELECTION_CRITERIA.md) | Criteria for adding a tool to RSQKit or the TechRadar. |
| [DashVERSE](https://www.dashverse.cloud/) | Dashboard to visualise quality assessments. |

### resqui

| Resource | Description |
|---|---|
| [Repository](https://github.com/EVERSE-ResearchSoftware/QualityPipelines) | Source code of `resqui`, the EVERSE quality pipeline. Indicators are implemented as plugins. |
| [Documentation](https://everse.software/QualityPipelines/) | User and developer documentation. |
| [Open issues](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/issues) and [pull requests](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/pulls) | Check these before starting, so you don't duplicate work in progress. |

<!-- TODO: add the resqui/TechRadar indicator tables here once they are published in the public Google Drive. -->

## Tracks

The project has three participation tracks, and you can join on site or remotely. No prior experience with software quality is needed for any of them.

| Track | Focus | Experience |
|---|---|---|
| **1. Coding** (this repository) | Add new tools and indicators to `resqui` as plugins, turning quality indicators into automated checks. | Some coding experience (Python, and Docker for some tools) and familiarity with Git and GitHub (forks, branches, pull requests) help. |
| 2. Workflows | Bring quality indicators to [WorkflowHub](https://workflowhub.eu/) and BioFAIR, using real workflows as test cases. | No coding experience needed. |
| 3. Health and genomic data | Shape the indicators B1MGplus needs, including security and a new *software responsibility* dimension for medical software. | No coding experience needed. |

**This repository is mainly for Track 1.** The rest of this README explains how to pick a tool or indicator and contribute it to `resqui`. If you're interested in Tracks 2 or 3, join the [Slack channel](https://biohackeu.slack.com/archives/C0BBMFB0XEY) and see the [project overview](https://docs.google.com/document/d/1zGAxM7-GvqGruW7ceickFvOGYLyjlxloTAKQlunU8SM/edit?usp=sharing).

## Key concepts

- **Indicator:** a measurable property of research software quality, for example `software_has_license` or `has_ci_tests`. Indicators are defined in the [EVERSE indicators catalogue](https://everse.software/indicators/website/indicators.html), and each one has an `@id` in the EVERSE vocabulary.
- **Tool:** an existing piece of software that can assess one or more indicators, for example [Gitleaks](https://everse.software/TechRadar/#/tool/gitleaks) or [OpenSSF Scorecard](https://github.com/ossf/scorecard). Tools are catalogued in the [TechRadar](https://everse.software/TechRadar/).
- **Plugin:** the `resqui` code that wraps a tool, runs it on a repository and reports the result for each indicator it covers. Plugins run the tool either as a Python package (`PythonExecutor`) or as a Docker image (`DockerExecutor`).

### Measuring vs. improving

The TechRadar links tools to indicators in two ways:

- `measuresQualityIndicator`: you run the tool on a piece of software and it reports whether, or how well, the indicator is met. For example, the [FAIRsoft Evaluator](https://everse.software/TechRadar/#/tool/fairsoft-evaluator) reads a repository's metadata and reports whether it has a license, documentation or a citation.
- `improvesQualityIndicator`: adopting the tool helps your software meet the indicator, but the tool doesn't evaluate anything. For example, Apptainer makes your software containerized, but it never inspects a repository or produces a score.

**Only measuring tools can become `resqui` plugins.** Some tools do both (for example `pytest`), and some TechRadar entries are not consistently curated. If you find a mismatch, such as a measuring tool listed only under "improves", it's worth reporting to the TechRadar.

### Software tiers

RSQKit classifies research software into [three tiers](https://everse.software/RSQKit/three_tier_view): **analysis code**, **prototype tools** and **research software infrastructure**. How relevant an indicator is depends on the tier. Something that is nice to have for analysis code can be essential for research software infrastructure. `resqui` ships [configuration files](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/tree/main/configurations) with recommended indicators for each tier (`indicators_analysis_code.json`, `indicators_prototype_tools.json` and `indicators_rs_infrastructure.json`).
