TermX utilizes several open-source projects created by TermX. The built NPM packages, Java libraries, and Docker images are published to GitHub Packages (github.com/orgs/termx-health/packages).


## Source code

### NPM

* https://github.com/termx-health/utils
* https://github.com/termx-health/marina

### Gradle

TermX Commons

* https://github.com/termx-health/kodality-commons
* https://github.com/termx-health/kodality-commons-micronaut
* https://github.com/termx-health/commons-dbutils

FHIR

* https://github.com/termx-health/kefhir
* https://github.com/termx-health/zmei

TermX

* https://github.com/termx-health/termx-server
* https://github.com/termx-health/termx-web
* https://github.com/termx-health/fsh-chef
* https://github.com/termx-health/termx-fml
* https://github.com/termx-health/termx-ssg
* https://github.com/termx-health/structure-definition-viewer


## Packages

* https://github.com/orgs/termx-health/packages?ecosystem=npm
* https://github.com/orgs/termx-health/packages?ecosystem=container
* https://github.com/orgs/termx-health/packages?ecosystem=maven


## RAW

+++ Click here to open
### Gradle

|-------------------------------------------|
| org.termx:bob                    |
| org.termx:edition-int            |
| org.termx:modeler                |
| org.termx:observation-definition |
| org.termx:snomed                 |
| org.termx:task                   |
| org.termx:terminology            |
| org.termx:termx-api              |
| org.termx:termx-core             |
| org.termx:ucum                   |
| commons-dbutils:commons-dbutils           |
{.dense}

`./gradlew dependencies | sed -n 's/.*--- \([^ ]*\).*/\1/p' | grep -v "^project$"   | sort | uniq | grep termx`


### NPM

|-------------------------------------------|
| @termx-health/core-util                   |
| @termx-health/marina-markdown             |
| @termx-health/marina-quill                |
| @termx-health/marina-ui                   |
| @termx-health/marina-util                 |
| @termx-health/structure-definition-viewer |
{.dense}

`npm ls | grep termx`
+++
