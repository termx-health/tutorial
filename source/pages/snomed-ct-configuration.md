# SNOMED CT configuration

SNOMED CT is configured using CodeSystem [snomed-module](cs:snomed-module). Every concept of this CodeSystem is a language module.

## Concept definition

#### Code
Language code

#### Designation

- **display**: language module name

#### Property
- **language**: language of module
- **refsetId**: snomed id of language refset
- **moduleId**: snomed id of language module
- **branchPath**: snomed branch path of module
  
![snomed-module](files/207/Screenshot%202024-06-11%20at%2014.50.15.png)


## Influence on other components


### Snomed translations

Snomed browser -> Snomed translations

Defining language modules is necessary for adding translations. Defined modules will appear in the module selection. Also properties *language*, *refsetId*, *moduleId* are used to create correct SNOMED CT designation.

### Snomed in TermX terminology

TermX has background logic related to the selection of SNOMED CT branch path accordingly with CodeSystem [snomed-ct](cs:snomed-ct) versions.

If [snomed-ct](cs:snomed-ct) version has a defined version URI then TermX would automatically find the correct branch path related to that URI and request SNOMED CT terms from Snowstorm using that path. The branch path will be found by the correspondence of moduleId in version URI and moduleId property in snomed-module CodeSystem.

**Example**

CodeSystem: snomed-ct -> version URI = http://snomed.info/sct/11000181102/version/20231130 
Module ID = 11000181102
Version = 20231130 (version will be converted to correct branch path version 2023-11-30)

CodeSystem: snomed-module -> concept where property moduleId = 11000181102 -> property branchPath = MAIN/SNOMEDCT-EE

Result = MAIN/SNOMEDCT-EE/2023-11-30
