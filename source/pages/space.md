## Space

The space refers to an area restricted or reserved for an individual or a group, providing a sense of exclusivity and control. 

The TermX space is used for several purposes. Space allows you:
- organize notes and documentation in the Wiki.
- to organize logically interconnected TermX objects, such as code systems, value sets, structure definitions, etc.
- to synchronise the TermX objects with GitHub.
- to synchronise the terminology with the external terminology server through FHIR API.
- to publish the space content as ImplementationGuide (IG).
- to generate static sites (SSG).
- to specify access permission to the space.

#### Permissions
- Add `Space.$space-id.$action` for users who should manage your space, including publishing functionality (GitHub, IG, SSG).
- Add `Wiki.$space-id.$action` for users who should work with Wiki pages.

#### Wiki
Every space may be used for documentation. No additional actions are needed.

#### TermX objects
You can create object groups and add TermX objects to the groups.
- Find section "packages" and press "Add package"
![](files/66/space_objects.png){width=400}
- Add code systems, value sets and concept maps to the "package".
- All wiki pages will be added automatically.
- Once objects are added, they will be visible on the preview page.
![](files/66/space_list.png){width=400}

#### Synchronization with external server
- Add external terminology of FHIR server first
- Add server into the space config
- Goto to the list of objects
- Select compare icon
- In the comparison view, select the external server
![](files/66/compare.png){width=400}
- Press "Sync" on the under proper server. 

#### GitHub
You can synchronize Wiki pages and TermX objects with GitHub.
Read more on the [TermX GitHub page](page:github).

#### Static site generation (SSD)
Configure [GitHub](page:github) first.
Configure [pipeline](page:static-site-generation) for SSD.

