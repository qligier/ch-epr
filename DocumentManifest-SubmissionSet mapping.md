# DocumentManifest-SubmissionSet mapping in CH-EPR

A review of [StructureDefinition: IHE_MHD_Comprehensive_DocumentManifest](http://fhir.ch/ig/ch-epr-mhealth/StructureDefinition-IHE.MHD.Comprehensive.DocumentManifest.html) against releveant XDS specifications.

### masterIdentifier
Mapped to `SubmissionSet.uniqueId`. In XDS, it's an OID (*1.3.6.1.4.1.21367.2005.3.7*); in FHIR, it's an URN-encoded OID (*urn:oid:1.3.6.1.4.1.21367.2005.3.7*) with the system *urn:ietf:rfc:3986*.

### identifier
Mapped to `SubmissionSet.entryUUID`. How?<br>
The entryUUID is mapped to Identifier.value. Do we need a type/system to specify it's the entryUUID (the identifier is 1..\*? How to select the entryUUID in multiple identifiers?

### status
Mapped to `SubmissionSet.availabilityStatus`.<br>
⚠️ In XDS, the status is always *Approved*; anything else will be rejected. The MHD status is unrestricted.

| DocumentManifest.status | SubmissionSet.availabilityStatus |
| ------------ | ------------ |
| current | urn:oasis:names:tc:ebxml-regrep:StatusType:Approved |
| superseded | urn:oasis:names:tc:ebxml-regrep:StatusType:Deprecated |
| entered-in-error | urn:oasis:names:tc:ebxml-regrep:StatusType:Withdrawn ? |

### type
Mapped to `SubmissionSet.contentTypeCode`.<br>
⚠️ There is a CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.contentTypeCode.html) but MHD is bound to another value set: [HL7 v3 Value Set ActCode](http://hl7.org/fhir/R4/v3/ActCode/vs.html)

### subject
Mapped to `SubmissionSet.patientId`. How?<br>
⚠️ IHE ITI MHD 4.5.1.2 says: 
> IHE constraint Reference(Patient). Not a contained resource. URL Points to an existing Patient Resource representing Affinity Domain Patient.

This is incompatible with adding the Patient resource in the Bundle.

### created
Mapped to `SubmissionSet.submissionTime`.<br>
⚠️ XDS and MHD allow for different date formats.

### author
Aweful mapping. Must support, so an important mapping.<br>
Swiss modification: SubmissionSet.Author.AuthorRole is required. See CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.Author.AuthorRole.html).<br>
- If author is Reference(Practitioner):
  - authorPerson is ?
  - authorInstitution: "Organization" in "contained". XON.1 is Organization.name, XON.6.2, XON.6.3 and XON.10 are Organization.identifier. Other XON fields are forbidden. Other Organization fields are forbidden, ignored or stored?
  - authorRole is "Healthcare professional"
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If author is Reference(Patient):
  - authorPerson is ?
  - authorInstitution is ?
  - authorRole is "Patient"
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If author is Reference(RelatedPerson):
  - authorPerson is ?
  - authorInstitution is ?
  - authorRole is "Representative"
  - authorSpecialty is ?
  - authorTelecommunication is ?
- If author is Reference(PractitionerRole): forbidden?
- If author is Reference(Organization): forbidden?
- If author is Reference(Device): forbidden?
- Missing Assistant and Technical user.

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
`SubmissionSet.homeCommunityId` is not mapped. Other FHIR properties are unmapped and will be lost.

### cardinalities

| Property | MHD | XDS sending | XDS responding | Comment |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| masterIdentifier | 1..1 | R | R | ✔️ OK |
| identifier | 1..*	| R | R | ✔️ OK |
| status | 1..1 | O | R | ✔️ OK |
| type | 1..1 | R | R | ✔️ OK |
| subject | 1..1 | R | R | ✔️ OK |
| created | 1..1 | R | R | ✔️ OK |
| author | 0..*	| R | R | ⚠️ Incompatible. (R because of authorRole, initially R2) |
| recipient | 0..* | O | O | ✔️ OK |
| source | 1..1 | R | R | ✔️ OK |
| description | 0..1 | O | O | ✔️ OK |
| text | 0..1 | O | O | ✔️ OK |
