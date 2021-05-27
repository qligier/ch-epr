# DocumentReference-DocumentEntry mapping in CH-EPR

A review of [Resource Profile: CH MHD DocumentReference Comprehensive](http://build.fhir.org/ig/ehealthsuisse/ch-epr-mhealth/StructureDefinition-ch-mhd-documentreference-comprehensive.html) (draft v0.2.0)
against releveant XDS specifications.

See also [generic XDS-FHIR mapping](Generic XDS-FHIR mapping.md).

### masterIdentifier
Mapped to `DocumentEntry.uniqueId`. In XDS, it's an OID (*1.3.6.1.4.1.21367.2005.3.7*); in FHIR, it's an URN-encoded OID (*urn:oid:1.3.6.1.4.1.21367.2005.3.7*) with the system *urn:ietf:rfc:3986*.<br>
⚠️ _value_ should be required and constrained to an UUID.

### identifier
Mapped to `DocumentEntry.entryUUID`. How?<br>
The entryUUID is mapped to Identifier.value.
Do we need a type/system to specify it's the entryUUID (the identifier is 1..\*? How to select the entryUUID in multiple identifiers?
=> If it's implementation dependant, the MHD sender is unable to know what will be the mapped entryUUID, that's bad.
Restrict to 1..1 and UUID?<br>
⚠️ _value_ should be required and constrained to an UUID.

### status
Mapped to `DocumentEntry.availabilityStatus`. Sending actor: required in MHD but not in XDS: not an issue, the default (and only sensible) value is `current`.

| DocumentReference.status | DocumentEntry.availabilityStatus |
| ------------ | ------------ |
| current | urn:oasis:names:tc:ebxml-regrep:StatusType:Approved |
| superseded | urn:oasis:names:tc:ebxml-regrep:StatusType:Deprecated |

### type
Mapped to `DocumentEntry.typeCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.<br>
⚠️ MHD binding is only preferred, not required.<br>
⚠️ No type-category mapping, as per [DocumentEntry.classCode_DocumentEntry.typeCode_mapping](https://art-decor.org/art-decor/decor-valuesets--ch-epr-?id=2.16.756.5.30.1.127.3.10.1.1.4&effectiveDate=2021-04-01T10:38:06).

### category
Mapped to `DocumentEntry.classCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.<br>
⚠️ MHD binding is only preferred, not required.<br>
⚠️ No type-category mapping, as per [DocumentEntry.classCode_DocumentEntry.typeCode_mapping](https://art-decor.org/art-decor/decor-valuesets--ch-epr-?id=2.16.756.5.30.1.127.3.10.1.1.4&effectiveDate=2021-04-01T10:38:06).

### subject
Mapped to `DocumentEntry.patientId`. The referenced CH Core Patient Profile is provided in the Bundle.

### author
Complex mapping. Mapped to `DocumentEntry.author`.<br>
⚠️ No MHD binding but XDS constrained by value set [DocumentEntry.authorSpeciality](https://art-decor.org/art-decor/decor-valuesets--ch-epr-?id=2.16.756.5.30.1.127.3.10.1.1.4&effectiveDate=2021-04-01T10:38:06).<br>
⚠️ No MHD binding but XDS constrained by value set [DocumentEntry.author.authorRole](https://art-decor.org/art-decor/decor-valuesets--ch-epr-?id=2.16.756.5.30.1.127.3.10.1.1.3&effectiveDate=2021-04-01T08:55:32).<br>
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

### ch-ext-author-authorrole
Mapped to `DocumentEntry.author.authorRole`. Value set on both sides.<br>
⚠️ It's 0..* in XDS and 1..1 in MHD.

### authenticator
Mapped to `DocumentEntry.legalAuthenticator`. XCN (HL7 V2.5 Extended Person Name) to Reference(CH Core Practitioner Profile | CH Core Practitioner Role Profile). Optional, so not so important for a first mapping.

## description
Mapped to `DocumentEntry.comments`.

## securityLabel
Mapped to `DocumentEntry.confidentialityCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.<br>
⚠️ MHD binding is only preferred, not required.

## ch-ext-deletionstatus
Mapped to `DocumentEntry.deletionStatus`. Value set on both sides.<br>

## originalProviderRole
⚠️ Absent from the current MHD profiles.

## date
⚠️ Absent from XDS.

## content.attachment.contentType
Mapped to `DocumentEntry.mimeType`.<br>
⚠️ No MHD binding but XDS constrained by value set [DocumentEntry.mimeType](https://art-decor.org/art-decor/decor-valuesets--ch-epr-?id=2.16.756.5.30.1.127.3.10.1.16&effectiveDate=2021-04-01T17:07:34).

## content.attachment.language
Mapped to `DocumentEntry.languageCode`. Value set on both sides.<br>
⚠️ MHD binding is only preferred, not required.

## content.attachment.size
Mapped to `DocumentEntry.size`. The size is calculated on the data prior to base64 encoding, if the data is base64 encoded.

## content.attachment.hash
Mapped to `DocumentEntry.hash`. The hash is encoded in FHIR in base64Binary, whereas in XDS hexbinary is used. The hash is calculated on the data prior to base64 encoding, if the data is base64 encoded.

## content.attachment.title
Mapped to `DocumentEntry.title`. 

## content.attachment.creation
Mapped to `DocumentEntry.creationTime`.<br>
⚠️ Absent for On-Demand document but always required in MHD.<br>
⚠️ XDS and MHD allow for different date formats.

## content.format
Mapped to `DocumentEntry.formatCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.<br>
⚠️ MHD binding is only preferred, not required.

## context.event
Mapped to `DocumentEntry.eventCodeList`.<br>
⚠️ MHD binding [HL7 v3 Value Set ActCode](http://hl7.org/fhir/R4/v3/ActCode/vs.html) is incompatible with [DocumentEntry.eventCodeList](https://art-decor.org/art-decor/decor-valuesets--ch-epr-?id=2.16.756.5.30.1.127.3.10.1.8&effectiveDate=2021-04-01T17:00:07) value set.

## context.period
Mapped to `DocumentEntry.serviceStartTime` and `DocumentEntry.serviceStopTime`.

## context.facilityType
Mapped to `DocumentEntry.healthcareFacilityTypeCode`.Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.<br>
⚠️ MHD binding is only preferred, not required.

## context.practiceSetting
Mapped to `DocumentEntry.practiceSettingCode`.Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.<br>
⚠️ MHD binding is only preferred, not required.

## context.sourcePatientInfo
Mapped to `DocumentEntry.sourcePatientInfo` and `DocumentEntry.sourcePatientId`.
In FHIR, it's a contained CH Core Patient Profile resource.<br>
There a value set for `DocumentEntry.sourcePatientInfo.PID-8`.<br>
`DocumentEntry.sourcePatientInfo` is way too hard to map, but it's optional.

| XDS | FHIR | Comment |
| ------------ | ------------ | ------------ |
| sourcePatientId.CX.1 | Patient.identifier[use[@value="usual"]].value[@value] |  |
| sourcePatientId.CX.6.HD.2 | Patient.identifier[use[@value="usual"]].system[@value] | Code system mapping |

## context.encounter
> When referenceIdList contains an encounter, and a FHIR Encounter is available, it may be referenced.

Mapped to `DocumentEntry.referenceIdList`. Not used in CH-EPR?

## context.related
> Use .identifier element to hold referenceIdList identifier(s).
> May use .reference element when the identifier has a known FHIR resource id.

Mapped to `DocumentEntry.referenceIdList`.

## objectType
⚠️ Absent from MHD but required in XDS. It should not be an issue for the PMP or the MobileAccessGateway, but others may encounter it.

## relatesTo
Is storred as an association in an XDS Registry. It means it will be stripped from the resource in an ITI-18 response that doesn't include Associations if it goes through the MobileAccessGateway, even if it was provided as a FHIR resource. Not sure how the PMP will manage that in a native MHD response.

### others
`DocumentEntry.homeCommunityId`, `documentAvailability`, `logicalID`, `version`, `legalAuthenticator`, `URI`, and `repositoryUniqueId` are not mapped. They're all optional (except for repositoryUniqueId, mandatory for XDS responding actor).

Other FHIR properties are not mapped and will be lost.


## cardinalities

| Property                       | MHD  | XDS sending | XDS responding | Comment |
| ------------                   | ---- | - | - | ------------ |
| deletionstatus	               | 0..1 | O | O | ✔️ OK |
| masterIdentifier               | 1..1 | R | R | ⚠️ Internal cardinality issue |
| identifier                     | 1..*	| R | R | ⚠️ External and internal cardinality issues |
| status                         | 1..1 | O | R | ✔️ OK (see comment) |
| type                           | 1..1 | R | R | ✔️ OK |
| category                       | 1..1 | R | R | ✔️ OK |
| subject                        | 1..1 | R | R | ✔️ OK |
| date                           | 1..1	| X | X | ⚠️ Incompatible |
| author                         | 0..* | O | O | ✔️ OK |
| authorRole                     | 1..1 | O | O | ⚠️ Incompatible |
| description                    | 0..1 | O | O | ✔️ OK |
| securityLabel                  | 1..* | R | R | ⚠️ Internal cardinality issue |
| content.attachment.contentType | 1..1 | R | R | ✔️ OK |
| content.attachment.language    | 1..1 | R | R | ✔️ OK |
| content.attachment.size        | 0..1 | O | R3 | ✔️ OK, may have to calculate it |
| content.attachment.hash        | 0..1 | O | R3 | ✔️ OK, may have to calculate it |
| content.attachment.title       | 0..1 | R | R | ⚠️ Incompatible |
| content.attachment.creation    | 1..1 | R | R/X | ⚠️ Incompatible |
| content.format                 | 1..1 | R | R | ⚠️ Internal cardinality issue |
| context.event                  | 0..* | O | O | ✔️ OK |
| context.period                 | 0..1 | O | O | ✔️ OK |
| context.facilityType           | 1..1 | R | R | ⚠️ Internal cardinality issue |
| context.practiceSetting        | 1..1 | R | R | ⚠️ Internal cardinality issue |
| context.sourcePatientInfo      | 1..1 | R | R | ✔️ OK |
| context.encounter              | 0..* | O | O | ✔️ OK |
| objectType                     | X    | R | R | ⚠️ Incompatible in rare cases |
| originalProviderRole           | ?    | R | R | Not sure about the XDS cardinality, it's absent from the Metadata Optionality table in Annex 5.1 |
| repositoryUniqueId             | X    | O | R | ⚠️ Incompatible in rare cases |
