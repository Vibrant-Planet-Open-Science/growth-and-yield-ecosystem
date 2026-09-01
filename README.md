# Open Source Ecosystem for Forest Growth & Yield
*This is a snapshot of an emergent ecosystem with loosely-connected components. And an open invitation to coordinate across it.*
Last updated August 27, 2026.

## Overview

The United States has an enviable base of forest data and modeling infrastructure. The Forest Inventory & Analysis (FIA) program produces the most comprehensive national forest inventory in the world. Growth-and-yield models built on it — the Forest Vegetation Simulator (FVS) most prominently, alongside regional and species-specific models maintained by universities and industry — are applied every year across hundreds of millions of acres. Each of these was chartered for a specific mission and has served it well.

What no program was ever chartered to do is steward the **connections** between them — or between them and the growing set of tools built on top, or the users who have adopted the whole chain as national infrastructure. Wildfire risk analysis, timberland valuation, treatment prioritization, carbon accounting, and remote sensing for forest monitoring all now consume growth-and-yield outputs. Most of those users were never the intended audience of any single program.

Open-source development has begun bridging the gaps: interfaces to FIA data, remote-sensing pipelines that produce model-ready inputs, modernized growth engines and language bindings, build and validation infrastructure, web services, and downstream decision support systems. Each is a reasonable answer to a real need. Collectively, they have been uncoordinated.

**This project is being set up to make these connections visible, documented, and easier to contribute to.**

## Goals and purpose
With this initial offering, we hope to provide:
- A public map of the components in this ecosystem, who maintains each, and how they relate. 
- A home for shared infrastructure — building, testing, validating, and delivering growth-and-yield models — that any growth-and-yield engine can use. 
- An open invitation to maintainers and users to coordinate on interfaces, testing, packaging, provenance, and release practice.
- A venue for maintainers to share best practices and lessons learned, including around the rapidly-expanding use of generative AI and agentic coding.

## Non-goals
This project and this leaping off point are *__not__* aspiring to make:
- A fork of, replacement for, or authority over any component listed here. Every project keeps its own maintainers, license, roadmap, and release cadence.
- A claim of stewardship over federally "owned" software. USDA Forest Service repositories are upstream resources we track, consume, and contribute to, not projects we govern.
- A commitment that any listed component will be adopted, integrated, modified, or supported. Being on this map means a project plays an important role in this ecosystem, not that anyone has signed up for work on it.
- An FVS project. FVS is the most widely applied engine and the most connected node in this network, which is why most existing shared infrastructure was built around it first. We see FVS as a node in this ecosystem, not an anchor. Regional, species-specific, and next-generation engines belong here on the same terms, and we seek to support infrastructure built to accept them.
- Models limited primarily to use by researchers. Acknowledging the important roles for basic research, we also recognize urgent and growing needs for use-inspired forest data and models to deliver actionable science for land managers and decision-makers navigating catastrophic risks and complex tradeoffs in rapidly changing landscapes and markets.

## How the pieces fit together

```
        FIA inventory data                  remote sensing
                |                                 |
     rFIA / pyFIA / forestTIME              fastfuels / TreeMap / LANDFIRE
                \                              /
                 +------> model inputs <------+
                                |
              growth-and-yield engines (FVS and others)
                                |
       shared infrastructure:  build - test - calibrate - validate - deliver
                                |
        language bindings  |  web services  |  hosted GUIs
                                |
     treatment planning  |  fire  |  timber  |  carbon  |  habitat  |  water
```

Read from top to bottom: **inventory and remote sensing produce model inputs → an engine projects them forward → shared infrastructure verifies that the engine builds correctly and predicts well → bindings and services deliver results → downstream tools and analyses consume them.** 

---

## The primary components in this open source ecosystem

Maintainer and license entries are recorded as published by each project. Maintainers are identified as those who have contributed 5 or more commits to a project within the past 12 months. 

### Inventory data producers
These components produce snapshots or time-series of the state of trees and forests. These are inputs that engines project forward, and the reference data validation and calibration workflows evaluate against.

| Component | What it does | Maintainers | License |
|---|---|---|---|
| [`doserjef/rFIA`](https://github.com/doserjef/rFIA) | R interface to the FIA database | Jeffrey Doser | GPL-3 |
| [`pyFIA`](https://github.com/mihiarc/pyfia) | Python toolkit for FIA data | Chris Mihiar | MIT |
| [`Evans-Ecology-Lab/forestTIME`](https://github.com/Evans-Ecology-Lab/forestTIME) | Annualized versions of FIA data | Eric Scott, Dani Steinberg | MIT |
| [`silvxlabs/fastfuels-core`](https://github.com/silvxlabs/fastfuels-core) | 3D tree, stand, and fuels attribution from remote sensing | Anthony Marcozzi, Daithi Martin, Lindsay Wiard | MIT |   
| [`silvxlabs/FastFuels-API-v2`](https://github.com/silvxlabs/FastFuels-API-v2) | REST API for fastfuels functionality | Anthony Marcozzi, Lindsay Wiard, Daithi Martin | MIT |   
| [`TreeMap`](https://research.fs.usda.gov/firelab/products/dataandtools/treemap-tree-level-model-united-states-forests) | Wall-to-wall imputation of FIA plots across CONUS | USDA Forest Service | Public Domain, code not published |
| [`LANDFIRE`](https://www.landfire.gov) | Large set of data layers on vegetation and fuels | USDA & DOI fire offices | Public Domain, code not published |


### Growth-and-yield engines
Engines are where inventory data get compiled into a suite of other derived tree-, plot-, and stand-level metrics, and where these state variables can be projected through time. FVS is the most connected engine in this network and therefore the first target for shared infrastructure, but the ecosystem is deliberately not organized to be anchored by any single model or repository. Other growth-and-yield engines — regional, species-specific, or next-generation, maintained by universities, agencies, or industry — belong here on the same terms.

| Component | What it does | Maintainers | License |
|---|---|---|---|
| [`USDAForestService/ForestVegetationSimulator`](https://github.com/USDAForestService/ForestVegetationSimulator) | Official FVS engine source; 22 US and 2 Canadian variants in Fortran 77 | Daniel Wagner | CC0 - Public Domain |
| [`USDAForestService/ForestVegetationSimulator-Interface`](https://github.com/USDAForestService/ForestVegetationSimulator-Interface) | Official FVS interface bundle: `rFVS` bindings, the `fvsOL` Shiny application, keyword construction, post-run utilities | Daniel Wagner, Ben Rice, Michael Shettles | CC0 - Public Domain |
| [`advanced-forestry-systems/fvs-modern`](https://github.com/advanced-forestry-systems/fvs-modern) | Community modernization of the FVS Fortran source, with associated calibration, validation, and build patterns | Aaron Weiskittel, Greg Johnson, David Marshall | MIT |
| [`mihiarc/pyfvs`](https://github.com/mihiarc/pyfvs) | Independent Python rewrite emulating FVS with a subset of regions and functionality | Chris Mihiar | MIT |

> [!NOTE]
> The engines listed above are designed for use at plot-to-stand scales (the scale at which silvicultural interventions are typically planned and implemented). Larger landscape and disturbance simulators that are open source and primarily used for research also exist in the US (e.g., [iLand](https://github.com/edfm-tum/iland-model), [LANDIS-II](https://www.landis-ii.org/)). There are also several repositories with implementations in R and Python of the 3-PG model, which operates at the gap-scale, but none appear to have been updated for 3+ years.

To our north, the Canadian Council of Forest Ministers newly-released [*Strategic Plan for the Advancement of National Climate-Sensitive Growth and Yield Modeling in Canada*](https://www.ccfm.org/releases/ccwg-strategic-plan-for-climate-sensitive-growth-and-yield-modelling/) lays out a vision for nationally-consistent open-source growth-and-yield models prioritizing modularity, interoperability, scalability, and complementarity with models used locally by provincial and territorial forest management agencies. With examples of open-source growth-and-yield models like the Acadian Variant of FVS already demonstrating the potential for US-originated models to cross international boundaries, productive engagement and coordination between open source ecosystems for growth-and-yield across North America may be more timely than ever before.

### Shared infrastructure for engines
There are commonly-shared engineering and quality assessment needs for growth-and-yield models. Common interfaces and data contracts enable the creation and adoption of reusable infrastructure for testing, calibration, and validation of models. The following components are produced or planned for release by the `Vibrant-Planet-Open-Science` organization on GitHub. The three prospective components below anticipate needs in FVS modernization that are generalizable for other engines which expose a comparable API and honor a shared data contract. 

| Component | What it does | Maintainers | License |
|---|---|---|---|
| [`fvs-build`](https://github.com/Vibrant-Planet-Open-Science/fvs-build) | Reusable GitHub workflows producing native binaries for all major operating systems and container images | David Diaz | CC-BY-NC-SA  |
| [`microfvs`](https://github.com/Vibrant-Planet-Open-Science/microfvs) | REST API for cloud-native model execution; consumes container images from `fvs-build` | David Diaz, Ben Smith | CC-BY-NC-SA |
| `engine-regression` | **Prospective.** Automated regression testing to detect changes in engine outputs so scientific continuity is enforceable during refactoring and development. Intended to ensure development in community forks can safely demonstrate parity with official upstream distributions. | — | — |
| `engine-calibration` | **Prospective.** Worfklows that allow an engine's suite of parameters to be tuned via black box optimization or heuristic to seek better overall behavior at replicating target forest attributes and dynamics. Sensitivity of engine behavior parameter settings can also be characterized. | — | — |
| `engine-validation` | **Prospective.** Execution of standardized validation protocols to characterize a model's predictive accuracy against FIA remeasurements, reference yield curves, and other reference datasets, producing comparable, release-quality reports using community-supported benchmarking datasets | — | — |

### Accessible engine wrappers

| Component | What it does | Maintainers | License |
|---|---|---|---|
| [`forest-modeling/PyFVS`](https://github.com/forest-modeling/PyFVS) | Python wrapper for an extended/modernized FVS Fortran API for a subset of US regions | Tod Haren | MIT |
| [`Vibrant-Planet-Open-Science/fvs2py`](https://github.com/Vibrant-Planet-Open-Science/fvs2py) | Python bindings to the canonical FVS API | David Diaz | CC-BY-NC-SA |
| [`rFVS`](https://github.com/USDAForestService/ForestVegetationSimulator-Interface) | R bindings to the FVS API, currently distributed inside the `USDAForestService/ForestVegetationSimulator-Interface` repository | Daniel Wagner, Ben Rice, Michael Shettles | CC0 - Public Domain |

> [!NOTE]
> Bindings are both a delivery channel and a testing surface for growth-and-yield engines written in lower-level languages. They build against engine releases, so source code modifications that alter engine behavior often surface here first. In the absence of unit and integration testing in official engine repositories, these wrapper libraries are the most promising candidates for enabling more thorough testing patterns.

### Downstream consumers in this ecosystem

Inclusion here is a descriptive sample — it is not a claim of participation nor obligation, and is not an exhaustive census. If you maintain one of these projects and would like it listed differently, or not at all, open an issue or contact us directly.

| Component | What it does | Maintainers | License |
|---|---|---|---|
| [`FSim`](https://research.fs.usda.gov/firelab/projects/fsim) | Fire spread engine (C++) distributed in binary form | USDA Forest Service | Public domain, code not published |
| [`ForSys`](https://www.forsysplanning.org) | Treatment prioritization | USDA Forest Service | Public domain, code not published | 
| [`forsys-sp/ForSysR`](https://github.com/forsys-sp/forsysr) | R implementation of ForSys | Cody Evers | GPL-3 |
| [`OurPlanscape/Planscape`](https://github.com/OurPlanscape/Planscape) | Landscape planning | Spatial Informatics Group | CC0 - Public Domain |
| [`Vibrant Planet Platform`](https://vibrantplanet.com/platform) | Landscape planning | Vibrant Planet, PBC | Proprietary, code not published |
| [`Forest Analytics for Carbon Tracking`](https://www.usendowment.org/what-we-do/ecosystem-markets/forestry-analytics-for-carbon-tracking) | Carbon accounting and reporting toolkit | US Endowment for Forestry & Communities | pre-release, code not published |  

In addition to these software systems that consume them, there is a much broader array of applications of growth-and-yield output data and projections in reports and planning documents spanning state and federal forest management plans, forest carbon project documentation, community wildfire protection plans, habitat conservation plans, timberland valuation and acquisition, sustainable harvest determinations, corporate and supply-chain GHG inventory and target-setting, forest product life cycle assessment, forest climate adaptation and vulnerability assessments, and more.

---

## What we are working on together

The near-term goal for this effort is coordination:

- **A shared map** — this page, kept current, so a newcomer can see the whole chain in one place.
- **Documented interfaces** at the boundaries where inventory becomes model input and model output becomes analysis.
- **Reproducible builds and verifiable artifacts**, so a user can confirm which engine binary produced a result.
- **End-to-end provenance** from inventory data through model output.
- **A governance conversation** among maintainers and major users about how this ecosystem coordinates, contributes, and sustains itself.
- **Sharing and encouragement of best practices** for software and data engineering including the uses of generative AI and agentic coding.

Each of these is scoped so that progress does not depend on any single organization's capacity, schedule, or approval.

## A proposal to seed a managing organization

A proposal is being prepared for the U.S. National Science Foundation's **Pathways to Enable Secure Open-Source Ecosystems (PESOSE)** program, Track 1: Scoping and Planning. The project is proposed to deliver an ecosystem assessment, a governance charter negotiated among participants, a risk and security assessment, and a community sustainability strategy.

Proposing team:
* David Diaz, Vibrant Planet Data Commons (Principal Investigator)
* Aaron Weiskittel, University of Maine (Co-PI)
* John Coulston,  National Council for Air & Stream Improvement (Co-PI)
* Holly Munro, National Council for Air & Stream Improvement (Co-PI)
* Ben Rice, Midgard Natural Resources (Co-PI)
* Tod Haren, Oregon Department of Forestry (Co-PI)

The proposing team is a starting point, not the governing body. Any organization that emerges from this effort is envisioned to include representation from the wider community of maintainers and stakeholders including representatives engaged in carbon markets, private timber industry, state and federal natural resource agencies, university and other research institutions, environmental NGOs, consulting forestry, and technology development.

## Get involved

- **Maintain a project that belongs on this map?** Open an issue. Being listed carries no obligation and no transfer of control.
- **Use these tools and hit a broken handoff?** Tell us where. Documenting real failure points is more useful than novel feature requests. Post an issue here or start a discussion.
- **Want to contribute code?** Start with the component repositories directly.

## License

Code in this repository is licensed under the [MIT License](LICENSE). The documentation in [`docs/`](docs/) is dedicated to the public domain under [CC0 1.0](docs/LICENSE) — reusable by anyone, for any purpose, without attribution.

Licenses of the components mapped above belong to their own maintainers and are unaffected by this repository.

## About this page

This map is maintained on a best-effort basis from public sources. Maintainer, license, and status entries reflect what each project publishes and may lag behind reality. Nothing here is a statement on behalf of any listed project or organization.
