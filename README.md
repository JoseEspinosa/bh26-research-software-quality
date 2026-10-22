# BioHackathon Europe 2026 · Research Software Quality

Entry point for **[Project 8](https://github.com/elixir-europe/biohackathon-projects-2026/blob/main/8.md)** of the [BioHackathon Europe 2026](https://biohackathon-europe.org/): *Advancing Research Software Quality across ELIXIR: Sustainable, Reproducible, and Interoperable Life-Science Software*.

The project reviews, maps and prioritises research software quality indicators that matter to ELIXIR and related initiatives, such as [EVERSE](https://everse.software/), [B1MGplus](https://b1mg-project.eu/) and [ELIXIR-STEERS](https://elixir-europe.org/about-us/how-funded/eu-projects/steers). We also turn those indicators into automated checks in [resqui](https://github.com/EVERSE-ResearchSoftware/QualityPipelines), the EVERSE quality pipeline.

This repository collects the resources and instructions you need to contribute during the hackathon, with a focus on adding new tools and indicators to [`resqui`](https://github.com/EVERSE-ResearchSoftware/QualityPipelines).

**Co-leads:** Laura Portell-Silva, Shoaib Sufi, Styliani-Christina Fragkouli


## Resources

### Project

| Resource | Description |
|---|---|
| [Project page](https://github.com/elixir-europe/biohackathon-projects-2026/blob/main/8.md) | Official description of Project 8 in the BioHackathon projects repository. |
| [Slack channel](https://biohackeu.slack.com/archives/C0BBMFB0XEY) | Main communication channel for the project (BioHackathon Europe Slack workspace). |

### EVERSE

| Resource | Description |
|---|---|
| [EVERSE indicators](https://everse.software/indicators/website/indicators.html) | Catalogue of measurable research software quality indicators. |
| [RSQKit](https://everse.software/RSQKit/tasks) | Research Software Quality Kit: community best practices and guidelines. |
| [Three-tier view](https://everse.software/RSQKit/three_tier_view) | Classification of research software into three tiers (analysis code, prototype tools, research software infrastructure). |
| [TechRadar](https://everse.software/TechRadar/) | Catalogue of tools that measure or improve quality indicators. The full catalogue is also available as [JSON](https://everse.software/TechRadar/api/tools.json). |
| [Tool selection criteria](https://github.com/EVERSE-ResearchSoftware/RSQKit/blob/main/TOOLS_SELECTION_CRITERIA.md) | Criteria for adding a tool to RSQKit or the TechRadar. |
<!-- | [DashVERSE](https://www.dashverse.cloud/) | Dashboard to visualise quality assessments. |
| [DashVERSE](https://github.com/EVERSE-ResearchSoftware/DashVERSE) | Dashboard to visualise quality assessments. | 
TODO: the link to dashverse does not work, not sure if the github repo should be visible
--> 

### resqui

| Resource | Description |
|---|---|
| [Repository](https://github.com/EVERSE-ResearchSoftware/QualityPipelines) | Source code of `resqui`, the EVERSE quality pipeline. Indicators are implemented as plugins. |
| [Documentation](https://everse.software/QualityPipelines/) | User and developer documentation. |
| [Open issues](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/issues) and [pull requests](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/pulls) | Check these before starting, so you don't duplicate work in progress. |

<!-- TODO: add the resqui/TechRadar indicator tables here once they are published in the public Google Drive. -->

## Project Tracks

The project has three participation tracks, and you can join on site or remotely. No prior experience with software quality is needed for any of them.

| Track | Focus | Experience | Coordinated |
|---|---|---|---|
| **1. Coding** (this repository) | Add new tools and indicators to `resqui` as plugins, turning quality indicators into automated checks. | Some coding experience (Python, and Docker for some tools) and familiarity with Git and GitHub (forks, branches, pull requests) help. | On site |
| 2. Workflows | Bring quality indicators to [WorkflowHub](https://workflowhub.eu/) and BioFAIR, using real workflows as test cases. | No coding experience needed. | Online |
| 3. Health and genomic data | Shape the indicators B1MGplus needs, including security and a new *software responsibility* dimension for medical software. | No coding experience needed. | On site |

**This repository is mainly for Track 1.** The rest of this README explains how to pick a tool or indicator and contribute it to `resqui`. If you're interested in Tracks 2 or 3, join the [Slack channel](https://biohackeu.slack.com/archives/C0BBMFB0XEY) please. 
<!-- TODO add were the info for the other tracks are -->

## Key concepts

- **Indicator:** a measurable property of research software quality, for example `software_has_license` or `has_ci-tests`. Indicators are defined in the [EVERSE indicators catalogue](https://everse.software/indicators/website/indicators.html), and each one has a unique identifier (its `@id`), a URI such as `https://w3id.org/everse/i/indicators/software_has_license`, which TechRadar and `resqui` use to refer to it.
- **Tool:** an existing piece of software that can assess one or more indicators, for example [Gitleaks](https://everse.software/TechRadar/#/tool/gitleaks) or [OpenSSF Scorecard](https://github.com/ossf/scorecard). Tools are catalogued in the [TechRadar](https://everse.software/TechRadar/).
- **Plugin:** the `resqui` code that wraps a tool, runs it on a repository and reports the result for each indicator it covers. Plugins run the tool either as a Python package (`PythonExecutor`) or as a Docker image (`DockerExecutor`).

### Measuring tools

In the TechRadar, a tool that assesses an indicator is linked to it with `measuresQualityIndicator`: you run the tool on a piece of software and it reports whether, or how well, the indicator is met. For example, the [FAIRsoft Evaluator](https://everse.software/TechRadar/#/tool/fairsoft-evaluator) reads a repository's metadata and reports whether it has a license, documentation or a citation.

**Only measuring tools can become `resqui` plugins.**

<!-- The TechRadar also links tools with `improvesQualityIndicator`: adopting the tool helps your software meet the indicator, but the tool doesn't evaluate anything. For example, Apptainer makes your software containerized, but it never inspects a repository or produces a score.
Some tools do both (for example `pytest`), and some TechRadar entries are not consistently curated. If you find a mismatch, such as a measuring tool listed only under "improves", it's worth reporting to the TechRadar. -->

### Software tiers

RSQKit classifies research software into [three tiers](https://everse.software/RSQKit/three_tier_view): **analysis code**, **prototype tools** and **research software infrastructure**. How relevant an indicator is depends on the tier. Something that is nice to have for analysis code can be essential for research software infrastructure. `resqui` ships configuration files with the recommended indicators for [analysis code](https://raw.githubusercontent.com/EVERSE-ResearchSoftware/QualityPipelines/main/configurations/indicators_analysis_code.json), [prototype tools](https://raw.githubusercontent.com/EVERSE-ResearchSoftware/QualityPipelines/main/configurations/indicators_prototype_tools.json) and [research software infrastructure](https://raw.githubusercontent.com/EVERSE-ResearchSoftware/QualityPipelines/main/configurations/indicators_rs_infrastructure.json).

EVERSE rates how relevant each indicator is for each tier: *crucial*, *recommended*, *good to have* or *not relevant*. The table below is taken from [Relevance of Indicators for RS Tiers](https://everse.software/indicators/website/rs_tiers.html).

<details>
<summary>Relevance of indicators for each tier</summary>

| Indicator | RS infrastructure | Prototype tools | Analysis code |
|---|---|---|---|
| [No leaked credentials](https://w3id.org/everse/i/indicators/no_leaked_credentials) | crucial | crucial | crucial |
| [Software has license](https://w3id.org/everse/i/indicators/software_has_license) | crucial | crucial | crucial |
| [Software makes use of version control](https://w3id.org/everse/i/indicators/version_control_use) | crucial | crucial | crucial |
| [Software has dependency management solution](https://w3id.org/everse/i/indicators/dependency_management) | crucial | recommended | recommended |
| [Software is listed in a registry](https://w3id.org/everse/i/indicators/listed_in_registry) | crucial | recommended | recommended |
| [Software has persistent and unique identifier](https://w3id.org/everse/i/indicators/persistent_and_unique_identifier) | crucial | recommended | recommended |
| [Has a measure of functional correctness](https://w3id.org/everse/i/indicators/functional_correctness) | crucial | recommended | good to have |
| [All tests defined by the software component pass](https://w3id.org/everse/i/indicators/passed_tests_ok) | crucial | recommended | good to have |
| [Software specifies requirements](https://w3id.org/everse/i/indicators/requirements_specified) | crucial | recommended | good to have |
| [Software has documentation](https://w3id.org/everse/i/indicators/software_has_documentation) | crucial | recommended | good to have |
| [Software follows versioning standards](https://w3id.org/everse/i/indicators/versioning_standards_use) | crucial | recommended | good to have |
| [Software is archived in a scholarly repository](https://w3id.org/everse/i/indicators/archived_in_scholarly_repository) | crucial | good to have | good to have |
| [Software has continuous integration tests](https://w3id.org/everse/i/indicators/has_ci-tests) | crucial | good to have | good to have |
| [Software has no linting issues](https://w3id.org/everse/i/indicators/has_no_linting_issues) | crucial | good to have | good to have |
| [Software has releases](https://w3id.org/everse/i/indicators/has_releases) | crucial | good to have | good to have |
| [No critical vulnerabilities](https://w3id.org/everse/i/indicators/no_critical_vulnerability) | crucial | good to have | good to have |
| [Software has ci/cd workflows in its repository](https://w3id.org/everse/i/indicators/repository_workflows) | crucial | good to have | good to have |
| [Software uses citation](https://w3id.org/everse/i/indicators/software_has_citation) | crucial | good to have | good to have |
| [Software has sufficient test coverage](https://w3id.org/everse/i/indicators/software_test_coverage) | crucial | good to have | good to have |
| [Has static analysis for common vulnerabilities](https://w3id.org/everse/i/indicators/static_analysis_common_vulnerabilities) | crucial | good to have | good to have |
| [Software has descriptive metadata](https://w3id.org/everse/i/indicators/descriptive_metadata) | crucial | good to have | not relevant |
| [Software provides tests](https://w3id.org/everse/i/indicators/software_has_tests) | crucial | good to have | not relevant |
| [Software provides issue tracking](https://w3id.org/everse/i/indicators/support_issue_tracking) | recommended | recommended | good to have |
| [Coupling between objects follows community conventions](https://w3id.org/everse/i/indicators/coupling_between_objects_ok) | recommended | good to have | not relevant |
| [Software is archived in Software Heritage](https://w3id.org/everse/i/indicators/archived_in_software_heritage) | recommended | good to have | good to have |
| [Code smells follow community conventions](https://w3id.org/everse/i/indicators/code_smells_ok) | recommended | good to have | good to have |
| [Software has up-to-date metadata](https://w3id.org/everse/i/indicators/metadata_is_up_to_date) | recommended | good to have | good to have |
| [Responses to issues and/or pull requests are done within a defined timeframe](https://w3id.org/everse/i/indicators/response_timeframe_ok) | recommended | good to have | good to have |
| [The project's repository includes a container build file](https://w3id.org/everse/i/indicators/software_is_containerized) | recommended | good to have | good to have |
| [Software uses a tool for warnings or mistakes](https://w3id.org/everse/i/indicators/uses_tool_for_warnings_and_mistakes) | recommended | good to have | good to have |
| [Code churn follows community conventions](https://w3id.org/everse/i/indicators/code_churn_ok) | recommended | good to have | not relevant |
| [Code duplication follows community conventions](https://w3id.org/everse/i/indicators/code_duplication_ok) | recommended | good to have | not relevant |
| [Project has active contributors beyond core maintainers](https://w3id.org/everse/i/indicators/has_active_contributors) | recommended | good to have | not relevant |
| [Software is published as a downloadable package](https://w3id.org/everse/i/indicators/has_published_package) | recommended | not relevant | not relevant |
| [Software repository has contribution guidelines](https://w3id.org/everse/i/indicators/has_contribution_guidelines) | good to have | recommended | good to have |
| [Project has active communication channels](https://w3id.org/everse/i/indicators/has_active_communication_channels) | good to have | good to have | good to have |
| [Project repository is active](https://w3id.org/everse/i/indicators/project_is_active) | good to have | good to have | good to have |
| [Software uses an appropriate license for different file types](https://w3id.org/everse/i/indicators/software_has_license_for_file_types) | good to have | good to have | good to have |
| [The documentation coverage for the codebase follows community conventions](https://w3id.org/everse/i/indicators/code_documentation_coverage_ok) | good to have | good to have | not relevant |
| [Cyclomatic complexity follows community conventions](https://w3id.org/everse/i/indicators/cyclomatic_complexity_ok) | good to have | good to have | not relevant |
| [Project repository has no binary artifacts](https://w3id.org/everse/i/indicators/has_no_binary_artifacts) | good to have | good to have | not relevant |
| [Cohesion among methods/functions of a module follows community conventions](https://w3id.org/everse/i/indicators/internal_cohesion_ok) | good to have | good to have | not relevant |
| [Number of lines of code follows community standards or expectations](https://w3id.org/everse/i/indicators/lines_of_code_ok) | good to have | good to have | not relevant |
| [Maintainability index follows community conventions](https://w3id.org/everse/i/indicators/maintainability_index_ok) | good to have | good to have | not relevant |
| [CodeMeta completeness](https://w3id.org/everse/i/indicators/codemeta_completeness) | good to have | not relevant | not relevant |
| [Software requires human code review](https://w3id.org/everse/i/indicators/human_code_review_requirement) | good to have | not relevant | not relevant |
| [Software uses fuzzing](https://w3id.org/everse/i/indicators/uses_fuzzing) | good to have | not relevant | not relevant |

</details>

## Candidate tools and indicators

The main target of Track 1 is **TechRadar tools that measure indicators not yet covered by `resqui`**. We compared the TechRadar catalogue with the plugins currently in `resqui` to build a list of candidates.

<!-- TODO: link the public Google Drive tables here:
     - TechRadar measuring tools vs. indicators already in resqui (candidates to add)
     - resqui indicators vs. TechRadar (mismatches worth reporting)
     - Prioritisation of candidate indicators per tier -->

### Work already in progress

Before picking a candidate, check that nobody is already working on it. These open `resqui` pull requests add new plugins or indicators:

| PR | Tool | Indicator |
|---|---|---|
| [#144](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/pull/144) | interrogate | `code_documentation_coverage_ok` |
| [#143](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/pull/143) | HowFairIs | `software_has_license` for file types |
| [#139](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/pull/139) | rsmetacheck | `metadata_is_up_to_date` |
| [#137](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/pull/137) | pylint | `code_smells_ok` |
| [#134](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/pull/134) | repowise | `code_churn_ok` |

See also the issue [#51 "list of next plugins"](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/issues/51), where the `resqui` team tracks which plugins to implement next.


<!-- TODO: confirm with Dmitry and Jose Maria whether these libraries can be added to resqui. -->

<!-- ### Proposed example cases -->

<!-- Two libraries that detect **license inconsistencies** in software, one for Java and one for Python, are being considered as example cases for the hackathon. The Java one would run as a Docker image with a wrapper script, and the Python one as a Python package. -->

## Contributing a new tool or indicator to `resqui`

<!-- 

### 1. Pick and claim a candidate

1. Choose a tool and indicator from the [candidates](#candidate-tools-and-indicators), and check it's not in the [work already in progress](#work-already-in-progress).
2. Open an issue in `resqui` with the [plugin or indicator request](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/issues/new?template=plugin_or_indicator_request.yml) template, or comment on an existing one, so others know you're working on it.
3. Check that the indicator exists in the [EVERSE indicators catalogue](https://everse.software/indicators/website/indicators.html). Its `@id` must match the EVERSE vocabulary.


### 2. Fork and set up `resqui`

Fork [EVERSE-ResearchSoftware/QualityPipelines](https://github.com/EVERSE-ResearchSoftware/QualityPipelines) on GitHub, then clone your fork and create a branch from `main`:

```bash
git clone git@github.com:<your-user>/QualityPipelines.git
cd QualityPipelines
git remote add upstream https://github.com/EVERSE-ResearchSoftware/QualityPipelines.git
git checkout -b plugin/<tool-name>
```

Install the development environment and run the tests (Python 3.9+, and Docker for plugins that use Docker images):

```bash
make install-dev
make test
```

> [!TIP]
> Always work from the `main` branch of the repository rather than the release on PyPI. The PyPI version may pull some Docker images (such as OpenSSF Scorecard) from registries that are no longer publicly accessible.

### 3. Write the plugin

The [Add an Indicator Plugin](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/blob/main/docs/how-to/add-a-plugin.md) guide covers the details. In short:

1. Create `src/resqui/plugins/<tool-name>.py` with a class that subclasses `IndicatorPlugin`. Set its `name`, `version`, `id` (the tool's TechRadar `@id`) and `indicators`, and add one method per indicator that returns a `CheckResult`.
2. Choose how to run the tool:
   - `PythonExecutor` if the tool is a Python package.
   - `DockerExecutor` if the tool is distributed as a Docker image, or needs other languages or system dependencies.
3. Import the plugin in `src/resqui/plugins/__init__.py`.
4. Add the indicator to a configuration file in `configurations/`.
5. Add tests in `tests/` and document the plugin in `docs/explanation/plugins.md`.

### 4. Check it works

```bash
resqui indicators                                   # your plugin and indicators should be listed
resqui -c <your-config>.json -u <repo-url> -t $GITHUB_TOKEN -o test.json
make test
```

### 5. Open a pull request

Push your branch to your fork and open a pull request against `main` in `EVERSE-ResearchSoftware/QualityPipelines`, linking the issue you opened in step 1. Follow the [contributing guidelines](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/blob/main/CONTRIBUTING.md).

If the tool is not in the TechRadar yet, it also needs to be registered there. Ask in the [Slack channel](https://biohackeu.slack.com/archives/C0BBMFB0XEY) how to do it.

### Further reading

- [Add an Indicator Plugin](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/blob/main/docs/how-to/add-a-plugin.md)
- [Plugins and the indicators they cover](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/blob/main/docs/explanation/plugins.md)
- [Custom configurations](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/blob/main/docs/how-to/custom-configuration.md)
- [Contributing guidelines](https://github.com/EVERSE-ResearchSoftware/QualityPipelines/blob/main/CONTRIBUTING.md)

-->

## Code of Conduct

The BioHackathon follows the [ELIXIR Code of Conduct](https://elixir-europe.org/events/code-of-conduct). It applies to both in-person and virtual interactions in this project. See the [BioHackathon registration page](https://biohackathon-europe.org/attend/registration#code-of-conduct) for details.
