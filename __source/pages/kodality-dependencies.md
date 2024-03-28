TermX utilizes several open-source projects created by Kodality. The built NPM packages, Java libraries, and Docker images are published to the Kodality Nexus repository (kexus.kodality.com).


## Source code

### NPM

* https://gitlab.com/kodality/ng/utils
* https://gitlab.com/kodality/ng/marina

### Gradle

Kodality Commons

* https://gitlab.com/kodality/kodality-commons
* https://gitlab.com/kodality/kodality-commons-micronaut
* https://github.com/kodality/commons-dbutils

FHIR

* https://gitlab.com/kodality/fhir/kefhir
* https://gitlab.com/kodality/fhir/zmei

TermX

* https://gitlab.com/kodality/terminology/termx-server
* https://gitlab.com/kodality/terminology/termx-web
* https://gitlab.com/kodality/terminology/fsh-chef
* https://gitlab.com/kodality/terminology/termx-fml
* https://gitlab.com/kodality/terminology/termx-ssg
* https://gitlab.com/kodality/terminology/structure-definition-viewer


## Nexus 

* https://kexus.kodality.com/repository/npm/
* https://kexus.kodality.com/repository/docker-public/
* https://kexus.kodality.com/repository/maven-releases/
* https://kexus.kodality.com/repository/maven-snapshots/


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
| com.kodality.termx:bob                    |
| com.kodality.termx:edition-int            |
| com.kodality.termx:modeler                |
| com.kodality.termx:observation-definition |
| com.kodality.termx:snomed                 |
| com.kodality.termx:task                   |
| com.kodality.termx:terminology            |
| com.kodality.termx:termx-api              |
| com.kodality.termx:termx-core             |
| com.kodality.termx:ucum                   |
| com.kodality.zmei:zmei-fhir-client        |
| com.kodality.zmei:zmei-fhir-jackson       |
| com.kodality.zmei:zmei-fhir               |
| commons-dbutils:commons-dbutils           |
{.dense}

`./gradlew dependencies | sed -n 's/.*--- \([^ ]*\).*/\1/p' | grep -v "^project$"   | sort | uniq | grep kodality`


### NPM

|-------------------------------------------|
| @kodality-web/core-util                   |
| @kodality-web/marina-markdown             |
| @kodality-web/marina-quill                |
| @kodality-web/marina-ui                   |
| @kodality-web/marina-util                 |
| @kodality-web/structure-definition-viewer |
{.dense}

`npm ls | grep kodality`
+++
