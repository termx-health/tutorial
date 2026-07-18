This page lists possible important features and refactoring ideas for future major versions.
[Suggest new features / improvements](https://github.com/termx-health/termx-server/issues)

> Several items originally on this roadmap have since shipped — see the [release notes](page:release-notes):
> the **FHIR Terminology Ecosystem** discovery/resolution (R3.1), **FHIR tx-ecosystem conformance** with a
> built-in test runner (R3.3), the **SNOMED RF2 redesign** (R3.2), and **wiki PDF/HTML export** (R3.3).
{.is-success}

# Offline snapshots
> These are not set in stone and only serve as a discussion point.
{.is-warning}
- ~~Ability to create the snapshot from all released resources to the files (to GitHub)~~
- A simplified version of the Terminology Server that uses only files and provides only read operations and FHIR operations.
- Ability to compose a docker image from snapshot files and the simplified version of TermX. The main idea is to install docker in the same data center where other applications of a TermX customer run (for example in a hospital server room), preventing possible downtime from the central TermX site or in the case of network interruptions.
- Requests/Proposals for changing of vocabulary.

