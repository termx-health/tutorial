TermX offers tight integration with [Snowstorm server](page:snowstorm) using its internal [API](https://snowstorm.kodality.dev/swagger-ui.html).

## Editions
SNOMED CT is managed and distributed by various organizations and national bodies in different countries, and they may create their editions of SNOMED CT to meet their specific needs. 
These editions are typically based on the international version of SNOMED CT. Still, they may include additional language translations, extensions, or customizations to suit the requirements of a particular region or healthcare system.

Using TermX [SNOMED CT management](/integration/snomed/management) you can get an overview of existing editions on your local Snowstorm server, including edition versions and create a new edition. 
![editions](files/109/snomed-edition-list.png)

### Setup editions
For a new installation, you should install the SNOMED International package first. You can download packages through [SNOMED Member Licensing and Distribution Service (MLDS)](https://mlds.ihtsdotools.org). The code of the SNOMED International Edition is `MAIN`.
After that, you can create a country-specific edition. You should specify the two alpha characters from the country code. For example, `EE` is for Estonia, and `LT` is for Lithuania.

### Published branches
You can import the published branches to the edition. Select proper edition -> ... -> Import from RF2. Specify the RF2 archive file and SNAPSHOT as a type of import.
> NB! The import of the published branches is not revertable. The only way to delete the published branch is to delete the edition and import it again.

![](files/109/SNOMEDCT-EE-with-modal.png){width=500}

### Daily build branches
Each SNOMED CT edition will have its own specific release cycle and updates. Daily build branches allow to manage editions: add new descriptions, inactivate and reactivate synonyms. A list of daily build branches can be found in [SNOMED CT management](/integration/snomed/management) component.

The daily build branch will appear in the list if the edition has a `Daily build available` property. This value can be changed in edition edit form.
![Daily build list](files/109/snomed-daily-build-list.png)


### Working branches
Working branches are another way to make changes. This process of developing SNOMED terminology is less automatic compared to daily build branches. A list of working branches can be found in [SNOMED CT management](/integration/snomed/management) component.

The working branch does not create a new version in the edition. You can create an unlimited number of versions and can delete them. You should specify the parent to your edition branch while creating the working branch. For the International edition branch is a `MAIN`; for the Estonian edition branch is `MAIN/SNOMEDCT-EE`.
The branch will appear in the working branch list if it is based on the edition branch and not on the version branch (an example of a version branch is `MAIN/SNOMEDCT-EE/2023-05-30`).
![Working branch list](files/109/working-branch-list.png)

### Authoring stats
Each daily build or working branch has information about authoring stats (new changes compared to the latest version of the edition). 

Different places influence authoring stats result:
- New descriptions: all approved translations connected to the current branch. New translations can be added in [SNOMED CT browser](page:snomed-ct-browser). Removing the new description from the authoring stats list is possible if the wrong description was approved. After that, it will appear in the `Ulinked translations` list. All unliked translations can be added to authoring stats again.  
- New synonyms: translations with type synonyms connected to the current branch.
- Inactivated synonyms: can be defined from the management component. Clicking on the dropdown modal form will appear for inactive synonym description number insertion.
- Reactivated synonyms: same process as synonym inactivation. Inactivated synonyms can be reverted using this function.

![Autoring stats](files/109/authoring-stats.png)

When changes are collected and need to be versioned, the way to upload them from the working branch and the daily build branch is different.

Daily build branch allow more automatic way of new release creation. Clicking on 'Version changes' will appear in a modal form, where the edition name and new release effective date should be specified. After the new release creation, authoring stats will be empty.

![Version changes modal](files/109/version-chages-modal.png){width=500}.

Working branch changes can be uploaded using the RF2 file. 
1. RF2 file export on a working branch (using tye SNAPSHOT)
2. Check `effectiveDate` fields in the exported file. In the next step, import `effectiveDate` will impact the release version name.
3. RF2 file import on the edition (using type SNAPSHOT)


