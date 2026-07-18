TermX utilizes several open-source projects created by TermX. The built NPM packages, Java libraries, and Docker images are published to GitHub Packages (github.com/orgs/termx-health/packages).


## Source code

### NPM

* https://github.com/termx-health/web-commons

### Gradle

FHIR

* https://github.com/termx-health/kefhir

TermX

* https://github.com/termx-health/termx-server
* https://github.com/termx-health/termx-web
* https://github.com/termx-health/termx-chef
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
| org.termx:edition-est            |
| org.termx:edition-int            |
| org.termx:implementation-guide   |
| org.termx:modeler                |
| org.termx:observation-definition |
| org.termx:snomed                 |
| org.termx:task                   |
| org.termx:task-taskforge         |
| org.termx:terminology            |
| org.termx:termx-api              |
| org.termx:termx-core             |
| org.termx:uam                    |
| org.termx:ucum                   |
| org.termx:wiki                   |
| org.termx:wiki-pdf               |
| commons-dbutils:commons-dbutils           |
{.dense}

`./gradlew dependencies | sed -n 's/.*--- \([^ ]*\).*/\1/p' | grep -v "^project$"   | sort | uniq | grep termx`


### NPM

|-------------------------------------------|
| @termx-health/core-util                   |
| @termx-health/markdown                    |
| @termx-health/markdown-parser             |
| @termx-health/quill                       |
| @termx-health/ui                          |
| @termx-health/util                        |
| @termx-health/structure-definition-viewer |
{.dense}

`npm ls | grep termx`
+++
