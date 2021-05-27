# DocumentManifest-SubmissionSet mapping in CH-EPR

A review of [StructureDefinition: IHE_MHD_Comprehensive_DocumentManifest_CH](http://fhir.ch/ig/ch-epr-mhealth/StructureDefinition-ch-mhd-comprehensive-documentmanifest.html) against releveant XDS specifications.

See also [generic XDS-FHIR mapping](Generic XDS-FHIR mapping.md).

### masterIdentifier
Mapped to `SubmissionSet.uniqueId`. In XDS, it's an OID (*1.3.6.1.4.1.21367.2005.3.7*); in FHIR, it's an URN-encoded OID (*urn:oid:1.3.6.1.4.1.21367.2005.3.7*) with the system *urn:ietf:rfc:3986*.

### identifier
Mapped to `SubmissionSet.entryUUID`. How?<br>
The entryUUID is mapped to Identifier.value. Do we need a type/system to specify it's the entryUUID (the identifier is 1..\*? How to select the entryUUID in multiple identifiers? => If it's implementation dependant, the MHD sender is unable to know what will be the mapped entryUUID, that's bad. Restrict to 1..1 and UUID?

### status
Mapped to `SubmissionSet.availabilityStatus`.<br>
In XDS, the status is always *Approved*; in MHD, the status is always *current*.

### type
Mapped to `SubmissionSet.contentTypeCode`. There is a CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.contentTypeCode.html) (currently a fixed value).

### subject
Mapped to `SubmissionSet.patientId`. The referenced CH Core Patient Profile is provided in the Bundle.

### created
Mapped to `SubmissionSet.submissionTime`.<br>
⚠️ XDS and MHD allow for different date formats.

### author and ch-ext-author-authorrole
Aweful mapping. Must support, so an important mapping?<br>
Swiss modification: SubmissionSet.Author.AuthorRole is required and mapped to ch-ext-author-authorrole. See CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.Author.AuthorRole.html).<br>
- If author is Reference(CH Core Practitioner Profile):
  - authorPerson is ?
  - authorInstitution: "Organization" in "contained". XON.1 is Organization.name, XON.6.2, XON.6.3 and XON.10 are Organization.identifier. Other XON fields are forbidden. Other Organization fields are forbidden, ignored or stored?
  - authorRole is "Healthcare professional"?
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If author is Reference(CH Core Patient Profile):
  - authorPerson is ?
  - authorInstitution is ?
  - authorRole is "Patient"?
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If author is Reference(RelatedPerson):
  - authorPerson is ?
  - authorInstitution is ?
  - authorRole is "Representative"?
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If author is Reference(CH Core Practitioner Role Profile): forbidden?
- If author is Reference(CH Core Organization Profile): forbidden?
- If author is Reference(Device): forbidden?
- Missing Assistant and Technical user.
⚠️ What to do if authorRole is e.g. HCP but author is Reference(CH Core Patient Profile)?

### recipient
Aweful mapping. Optional, so not so important for a first mapping.

### source
Mapped to `SubmissionSet.sourceId`. OID in XDS, urn-encoded OID in MHD.

### description
Mapped to `SubmissionSet.title`.

### text
Mapped to `SubmissionSet.comments`. Raw text in XDS, XHTML in FHIR.<br>
⚠️ If XHTML text is given in MHD, it will be stripped in the mapping.<br>
⚠️ Narrative.status is mandatory in FHIR, absent from XDS.

### content
Contains other resources.

### meta.profile
Reference to [StructureDefinition: IHE_MHD_Comprehensive_DocumentManifest](http://fhir.ch/ig/ch-epr-mhealth/StructureDefinition-IHE.MHD.Comprehensive.DocumentManifest.html).<br>
Mapped to `SubmissionSet.limitedMetadata`, which is forbidden in XDS, so no mapping needed.<br>
This could be restricted in MHD, other profiles should not be supported.

### others
`SubmissionSet.homeCommunityId` is not mapped but is optional. Other FHIR properties are unmapped and will be lost.

## cardinalities

| Property | MHD | XDS sending | XDS responding | Comment |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| masterIdentifier | 1..1 | R | R | ✔️ OK |
| identifier       | 1..*	| R | R | ✔️ OK |
| status           | 1..1 | O | R | ✔️ OK |
| type             | 1..1 | R | R | ✔️ OK |
| subject          | 1..1 | R | R | ✔️ OK |
| created          | 1..1 | R | R | ✔️ OK |
| author           | 0..*	| R | R | ⚠️ Incompatible |
| authorRole       | 1..1	| O | O | ⚠️ Incompatible |
| recipient        | 0..* | O | O | ✔️ OK |
| source           | 1..1 | R | R | ✔️ OK |
| description      | 0..1 | O | O | ✔️ OK |
| text             | 0..1 | O | O | ✔️ OK |
