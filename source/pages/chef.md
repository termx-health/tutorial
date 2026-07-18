Chef is the module responsible for transforming FHIR StructureDefinitions between FSH format and JSON, and vice versa.
Chef consists of two projects:
- [Sushi](https://github.com/FHIR/sushi) for converting FSH to JSON
- [GoFSH](https://github.com/FHIR/GoFSH) for converting JSON to FSH

It is packaged as the `ghcr.io/termx-health/termx-chef` Docker image (the service and container are named `fsh-chef`) and exposes a REST API on port `3000`, published on host port `8500`. The web application reaches it through the `CHEF_URL` setting.


