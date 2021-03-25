# XDS-FHIR resources mapping in CH-EPR

This is a draft to map FHIR resources to equivalent XDS resources in CH-EPR. XDS cardinalities incorporate Swiss requirements.

[XDS-FHIR-mapping](https://wiki.ihe.net/index.php/XDS-FHIR-mapping "XDS-FHIR-mapping") by IHE, [Resource DocumentManifest - Mappings](https://www.hl7.org/fhir/documentmanifest-mappings.html "Resource DocumentManifest - Mappings") and [Resource DocumentReference - Mappings](https://www.hl7.org/fhir/documentreference-mappings.html "Resource DocumentReference - Mappings") by HL7.

See also IHE ITI TF MHD, 4.5.1.2.




## Mapping DocumentManifest to SubmissionSet

### masterIdentifier
Mapped to `SubmissionSet.uniqueId`. In XDS, it's an OID (*1.3.6.1.4.1.21367.2005.3.7*); in FHIR, it's an URN-encoded OID (*urn:oid:1.3.6.1.4.1.21367.2005.3.7*) with the system *urn:ietf:rfc:3986*.

### identifier
Mapped to `SubmissionSet.entryUUID`. How?<br>

### status
Mapped to `SubmissionSet.availabilityStatus`.<br>
⚠️ In XDS, the status is always *Approved*; anything else should be rejected.

| DocumentManifest.status | SubmissionSet.availabilityStatus |
| ------------ | ------------ |
| current | urn:oasis:names:tc:ebxml-regrep:StatusType:Approved |
| superseded | urn:oasis:names:tc:ebxml-regrep:StatusType:Deprecated |
| entered-in-error | urn:oasis:names:tc:ebxml-regrep:StatusType:Withdrawn ? |

### type
Mapped to `SubmissionSet.contentTypeCode`.<br> There is a CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.contentTypeCode.html).

### subject
Mapped to `SubmissionSet.patientId`. How?<br>
⚠️ IHE ITI MHD 4.5.1.2 says: 
> IHE constraint Reference(Patient). Not a contained resource. URL Points to an existing Patient Resource representing Affinity Domain Patient.

### created
Mapped to `SubmissionSet.submissionTime`. See dates mapping below.

### author
Aweful mapping.<br>
Swiss modification: SubmissionSet.Author.AuthorRole is required. See CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.Author.AuthorRole.html).<br>
- If author is Reference(Practitioner):
  - authorPerson is
  - authorInstitution: "Organization" in "contained". XON.1 is Organization.name, XON.6.2, XON.6.3 and XON.10 are Organization.identifier. Other XON fields are forbidden. Other Organization fields are forbidden, ignored or stored?
  - authorRole is "Healthcare professional"
  - authorSpecialty is
  - authorTelecommunication is
- If author is Reference(Patient):
  - authorPerson is
  - authorInstitution is
  - authorRole is "Patient"
  - authorSpecialty is
  - authorTelecommunication is
- If author is Reference(RelatedPerson):
  - authorPerson is
  - authorInstitution is
  - authorRole is "Representative"
  - authorSpecialty is
  - authorTelecommunication is
- If author is Reference(PractitionerRole): forbidden?
- If author is Reference(Organization): forbidden?
- If author is Reference(Device): forbidden?
- Missing Assistant and Technical user.

### recipient
Aweful mapping.

### source
Mapped to `SubmissionSet.sourceId`.

### description
Mapped to `SubmissionSet.title`.

### text
Mapped to `SubmissionSet.comments`. Raw text in XDS, XHTML in FHIR.<br>
⚠️ Tags should be forbidden in FHIR.<br>
⚠️ Narrative.status is required in FHIR, absent from XDS.

### content
Contains other resources.

### meta.profile
Mapped to `SubmissionSet.limitedMetadata`. Will probably not be used in CH-EPR.

### others
`SubmissionSet.homeCommunityId` is not mapped (it's probably not needed). Other FHIR properties are unmapped and will be lost.

### cardinalities

| Property | FHIR | XDS sending | XDS responding | Comment |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| masterIdentifier | 0..1 | R | R | ⚠️ Incompatible for sending actor |
| identifier | 0..*	| R | R | ⚠️ Incompatible for sending actor |
| status | 1..1 | O | R | ✔️ OK |
| type | 0..1 | R | R | ⚠️ Incompatible for sending actor |
| subject | 0..1 | R | R | ⚠️ Incompatible for sending actor |
| created | 0..1 | R | R | ⚠️ Incompatible for sending actor |
| author | 0..*	| R | R | ⚠️ Incompatible for sending actor |
| recipient | 0..* | O | O | ✔️ OK |
| source | 0..1 | R | R | ⚠️ Incompatible for sending actor |
| description | 0..1 | O | O | ✔️ OK |
| text | 0..1 | O | O | ✔️ OK |




## Mapping DocumentReference to DocumentEntry

### masterIdentifier
Mapped to `DocumentEntry.uniqueId`. In XDS, it's an OID (*1.3.6.1.4.1.21367.2005.3.7*); in FHIR, it's an URN-encoded OID (*urn:oid:1.3.6.1.4.1.21367.2005.3.7*) with the system *urn:ietf:rfc:3986*.

### identifier
Mapped to `DocumentEntry.entryUUID`. How?<br>

### status
Mapped to `DocumentEntry.availabilityStatus`.

| DocumentReference.status | DocumentEntry.availabilityStatus |
| ------------ | ------------ |
| current | urn:oasis:names:tc:ebxml-regrep:StatusType:Approved |
| superseded | urn:oasis:names:tc:ebxml-regrep:StatusType:Deprecated |
| entered-in-error | ⚠️ |

### type
Mapped to `DocumentEntry.typeCode`. See below for the code system mapping.
typeCode = type.coding.code, display name = type.coding.display, coding scheme = type.coding.system.

### category
Mapped to `DocumentEntry.classCode`. See below for the code system mapping.
classCode = category.coding.code, display name = category.coding.display, coding scheme = category.coding.system.

### subject
Mapped to `DocumentEntry.patientId`. How?<br>
⚠️ IHE ITI MHD 4.5.1.2 says: 
> IHE constraint Reference(Patient). Not a contained resource. URL Points to an existing Patient Resource representing Affinity Domain Patient.

### author
Aweful mapping. Mapped to `DocumentEntry.author`.

### authenticator
Mapped to `DocumentEntry.legalAuthenticator`. XCN (HL7 V2.5 Extended Person Name) to Reference(Practitioner | PractitionerRole | Organization)

## description
Mapped to `DocumentEntry.comments`.

## securityLabel
Mapped to `DocumentEntry.confidentialityCode`. See below for the code system mapping.
confidentialityCode = securityLabel.coding.code, display name = securityLabel.coding.display, coding scheme = securityLabel.coding.system.

## content.attachment.contentType
Mapped to `DocumentEntry.mimeType`. 

## content.attachment.language
Mapped to `DocumentEntry.languageCode`. 

## content.attachment.size
Mapped to `DocumentEntry.size`. The size is calculated on the data prior to base64 encoding, if the data is base64 encoded.

## content.attachment.hash
Mapped to `DocumentEntry.hash`. The IHE Document Sharing metadata element hash holds the SHA1 hash of the document. The hash is encoded in FHIR in base64Binary, whereas in XDS hexbinary is used. The hash is calculated on the data prior to base64 encoding, if the data is base64 encoded.

## content.attachment.title
Mapped to `DocumentEntry.title`. 

## content.attachment.creation
Mapped to `DocumentEntry.creationTime`. See dates mapping below.

## content.format
Mapped to `DocumentEntry.formatCode`. See below for the code system mapping.
formatCode = content.format.code, coding scheme = content.format.system.

## context.event
Mapped to `DocumentEntry.eventCodeList`.

## context.period
Mapped to `DocumentEntry.serviceStartTime` and `DocumentEntry.serviceStopTime`.

## context.facilityType
Mapped to `DocumentEntry.healthcareFacilityTypeCode`.

## context.practiceSetting
Mapped to `DocumentEntry.practiceSettingCode`.

## context.sourcePatientInfo
Mapped to `DocumentEntry.patientId`, `DocumentEntry.sourcePatientInfo` and `DocumentEntry.sourcePatientId`. In FHIR, it's a contained Patient resource. The sourcePatientInfo attribute should not include values for PID-2 (patient id), PID-4 (alternate patient id), PID-12 (country code), or PID-19 (social security number). ⚠️ Way too many PID segments to map.

| XDS | FHIR | Comment |
| ------------ | ------------ | ------------ |
| patientId.CX.1 | Patient.identifier.value[@value] |  |
| patientId.CX.6.HD.2 | Patient.identifier.system[@value] | Code system mapping |
| sourcePatientId.CX.1 | Patient.identifier[use[@value="usual"]].value[@value] |  |
| sourcePatientId.CX.6.HD.2 | Patient.identifier[use[@value="usual"]].system[@value] | Code system mapping |
| sourcePatientInfo.PID.3. | ? |  |
| sourcePatientInfo.PID.5.1 | Patient.name.family[@value] |  |
| sourcePatientInfo.PID.5.2 | Patient.name.given[@value] |  |
| sourcePatientInfo.PID.5.4 | Patient.name.suffix[@value] |  |
| sourcePatientInfo.PID.5.5 | Patient.name.prefix[@value] | ⚠️ |
| sourcePatientInfo.PID.5.6 | Patient.name.prefix[@value] | ⚠️ |
| sourcePatientInfo.PID.5.12 | Patient.name.period | Date mapping |
| sourcePatientInfo.PID.5.13 | Patient.name.period | Date mapping |
| sourcePatientInfo.PID.7 | Patient.birthDate[@value] | Date mapping |
| sourcePatientInfo.PID.8 | Patient.gender | 4 values in each value set |
| sourcePatientInfo.PID.11.1 | Patient.address.line | ⚠️ |
| sourcePatientInfo.PID.11.2 | Patient.address.line | ⚠️ |
| sourcePatientInfo.PID.11.3 | Patient.address.city |  |
| sourcePatientInfo.PID.11.4 | Patient.address.state |  |
| sourcePatientInfo.PID.11.5 | Patient.address.zip |  |
| sourcePatientInfo.PID.11.6 | Patient.address.country |  |
| sourcePatientInfo.PID.11.7 | Patient.address. |  |
| sourcePatientInfo.PID.11.13 | Patient.address.period | Date mapping |
| sourcePatientInfo.PID.11.14 | Patient.address.period | Date mapping |

## context.related / context.encounter
Mapped to `DocumentEntry.referenceIdList`.

### others
`DocumentEntry.repositoryUniqueId` is not mapped. Other FHIR properties are unmapped and will be lost.




## Dates mapping

HL7's DTM shall be encoded in the format `YYYY[MM[DD[HH[MM[SS[.S[S[S[S]]]]]]]]][+/-ZZZZ]`. It allows various precision levels and the choice of time zone.
The dateTime format is different: `YYYY`, `YYYY-MM`, `YYYY-MM-DD`, `YYYY-MM-DDThh:mm:ss+zz:zz` or `YYYY-MM-DDThh:mm:ss.sssZ`. When using the time precision, the timezone is mandatory.<br>
⚠️ Guidance is required to convert e.g. `YYYYMMDDHH` to FHIR dateTime (use the earliest instant covered by the partial date?).




## Code systems mapping

| Name | XDS OID | FHIR URI |
| ------------ | ------------ | ------------ |
| SNOMED Clinical Terms | 2.16.840.1.113883.6.96 | http://snomed.info/sct |
| SNOMED Clinical Terms Swiss Extension | 2.16.756.5.30.1.127.3.4 | urn:oid:2.16.756.5.30.1.127.3.4 |
| ch-ehealth-codesystem-role | 2.16.756.5.30.1.127.3.10.6 | urn:oid:2.16.756.5.30.1.127.3.10.6 |
| ch-ehealth-codesystem-medreg | 2.16.756.5.30.1.127.3.5 | urn:oid:2.16.756.5.30.1.127.3.5 |
| ch-ehealth-codesystem-nareg | 2.16.756.5.30.1.127.3.6 | urn:oid:2.16.756.5.30.1.127.3.6 |
| DICOM Controlled Terminology | 1.2.840.10008.2.16.4 | http://dicom.nema.org/resources/ontology/DCM |
| ch-ehealth-codesystem-format | 2.16.756.5.30.1.127.3.10.10 | urn:oid:2.16.756.5.30.1.127.3.10.10 |
| DICOM UID | 1.2.840.10008.2.6.1 | urn:oid:1.2.840.10008.2.6.1 |
| DocumentReference Format Code Set | 1.3.6.1.4.1.19376.1.2.3 | http://ihe.net/fhir/ValueSet/IHE.FormatCode.codesystem |
| Tags for Identifying Languages - RFC5646 | 2.16.840.1.113883.6.316 | urn:oid:2.16.840.1.113883.6.316 |
| ch-ehealth-codesystem-language | 2.16.756.5.30.1.127.3.10.12 | urn:oid:2.16.756.5.30.1.127.3.10.12 |
| Media Type | 2.16.840.1.113883.5.79 | urn:oid:2.16.840.1.113883.5.79 |
