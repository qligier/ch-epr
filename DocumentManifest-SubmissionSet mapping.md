# DocumentManifest-SubmissionSet mapping in CH-EPR

A review of [Resource Profile: CH MHD SubmissionSet Comprehensive](http://build.fhir.org/ig/ehealthsuisse/ch-epr-mhealth/StructureDefinition-ch-mhd-submissionset-comprehensive.html) (draft v0.2.0) against releveant XDS specifications.

See also [generic XDS-FHIR mapping](Generic XDS-FHIR mapping.md).

### ihe-designationType
Mapped to `SubmissionSet.contentTypeCode`?<br>
⚠️ Not bound in MHD but restrained in XDS by [SubmissionSet.contentTypeCode](https://art-decor.org/art-decor/decor-valuesets--ch-epr-?id=2.16.756.5.30.1.127.3.10.1.40&effectiveDate=2021-04-01T17:08:39).

### ihe-sourceId
Mapped to `SubmissionSet.sourceId`.<br>
⚠️ _value_ should be required.

### ihe-intendedRecipient
Complex mapping. Optional, so not so important for a first mapping.<br>
⚠️ Bad reference types (not the CH Core types).

### identifier:uniqueId
Mapped to `SubmissionSet.uniqueId`.<br>
⚠️ _value_ should be required and constrained to an UUID.

### identifier:entryUUID
Mapped to `SubmissionSet.entryUUID`.<br>
How to select the entryUUID in multiple identifiers? => If it's implementation dependant, the MHD sender is unable to know what will be the mapped entryUUID, that's bad. Restrict to 1..1 and UUID?<br>
⚠️ _value_ should be required and constrained to an UUID.<br>
⚠️ The cardinality could be adjusted to 1..1.

### status
Mapped to `SubmissionSet.availabilityStatus`. In XDS, the status is always *Approved*.<br>
⚠️ It should be fixed to *current*, as it is in the published version.

### mode
Fixed value, no mapping.

### title
Mapped to `SubmissionSet.title`.

### subject
Mapped to `SubmissionSet.patientId`. The referenced CH Core Patient Profile is provided in the Bundle.

### date
Mapped to `SubmissionSet.submissionTime`? XDS and MHD allow for different date formats.<br>
⚠️ Technically not the same date (creation vs. submission).

### source and ch-ext-author-authorrole
Complex mapping. Must support, so an important mapping?<br>
- If source is Reference(CH Core Practitioner Profile):
  - authorPerson is ?
  - authorInstitution: "Organization" in "contained". XON.1 is Organization.name, XON.6.2, XON.6.3 and XON.10 are Organization.identifier. Other XON fields are forbidden. Other Organization fields are forbidden, ignored or stored?
  - authorRole is "Healthcare professional"?
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If source is Reference(CH Core Patient Profile):
  - authorPerson is ?
  - authorInstitution is ?
  - authorRole is "Patient"?
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If source is Reference(RelatedPerson):
  - authorPerson is ?
  - authorInstitution is ?
  - authorRole is "Representative"?
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If source is Reference(CH Core Practitioner Role Profile): forbidden?
- If source is Reference(CH Core Organization Profile): forbidden?
- If source is Reference(Device): forbidden?
- Missing Assistant and Technical user.
⚠️ What to do if authorRole is e.g. HCP but source is Reference(CH Core Patient Profile)?<br>
⚠️ Bad reference types (not the CH Core types). Also for _ihe-authorOrg_.

### text
Mapped to `SubmissionSet.comments`. Raw text in XDS, XHTML in FHIR.<br>
⚠️ If XHTML text is given in MHD, it will be stripped in the mapping.<br>
⚠️ Narrative.status is mandatory in FHIR, absent from XDS.

### others
`SubmissionSet.homeCommunityId` is not mapped but is optional. Other FHIR properties are unmapped and will be lost.

## cardinalities

| Property | MHD | XDS sending | XDS responding | Comment |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| text                     | 0..1 | O | O | ✔️ OK |
| ihe-designationType      | 1..1 | R | R | ✔️ OK |
| ihe-sourceId             | 1..1 | R | R | ⚠️ Internal cardinality issue |
| ihe-intendedRecipient    | 0..* | O | O | ✔️ OK |
| ch-ext-author-authorrole | 0..1 | O | O | ⚠️ There may be several author roles in a SubmissionSet |
| identifier:uniqueId      | 1..1 | R | R | ⚠️ Internal cardinality issue |
| identifier:entryUUID     | 0..*	| R | R | ⚠️ Cardinality can be improved, internal cardinality issue |
| status                   | 1..1 | O | R | ✔️ OK |
| title                    | 0..1 | O | O | ✔️ OK |
| subject                  | 1..1 | R | R | ✔️ OK |
| date                     | 1..1 | R | R | ✔️ OK |
| source                   | 0..1 | R2 | R2 | ⚠️ There may be several authors in a SubmissionSet |
