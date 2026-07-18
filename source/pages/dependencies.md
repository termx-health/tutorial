TermX utilizes several open-source projects created by TermX. The built NPM packages, Java libraries, and Docker images are published to the TermX Nexus repository (ghcr.io/termx-health).


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


## Nexus 

* https://ghcr.io/termx-health/repository/npm/
* https://ghcr.io/termx-health/repository/docker-public/
* https://ghcr.io/termx-health/repository/maven-releases/
* https://ghcr.io/termx-health/repository/maven-snapshots/


## RAW

+++ Click here to open
### Gradle

|-------------------------------------------|
| com.kodality.commons:commons-cache        |
| com.kodality.commons:commons-db-core      |
| com.kodality.commons:commons-db           |
| com.kodality.commons:commons-http-client  |
| com.kodality.commons:commons-micronaut-pg |
| com.kodality.commons:commons-micronaut    |
| com.kodality.commons:commons-model        |
| com.kodality.commons:commons-sequence     |
| com.kodality.commons:commons-tenant       |
| com.kodality.commons:commons-util-spring  |
| com.kodality.commons:commons-util         |
| com.kodality.kefhir:fhir-rest             |
| com.kodality.kefhir:fhir-structures       |
| com.kodality.kefhir:kefhir-core           |
| com.kodality.kefhir:openapi               |
| com.kodality.kefhir:tx-manager            |
| com.kodality.kefhir:validation-profile    |
| com.kodality.taskflow:taskflow-service    |
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
| com.kodality.zmei:zmei-fhir-client        |
| com.kodality.zmei:zmei-fhir-jackson       |
| com.kodality.zmei:zmei-fhir               |
| commons-dbutils:commons-dbutils           |
{.dense}

`./gradlew dependencies | sed -n 's/.*--- \([^ ]*\).*/\1/p' | grep -v "^project$"   | sort | uniq | grep kodality`


### NPM

|-------------------------------------------|
| @termx-health/core-util                   |
| @termx-health/marina-markdown             |
| @termx-health/marina-quill                |
| @termx-health/marina-ui                   |
| @termx-health/marina-util                 |
| @termx-health/structure-definition-viewer |
{.dense}

`npm ls | grep kodality`
+++
