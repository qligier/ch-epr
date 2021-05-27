# DocumentReference-DocumentEntry mapping in CH-EPR

A review of [StructureDefinition: IHE_MHD_Provide_Comprehensive_DocumentReference_CH](http://fhir.ch/ig/ch-epr-mhealth/StructureDefinition-ch-mhd-provide-comprehensive-documentreference.html)
and [StructureDefinition: IHE_MHD_Query_Comprehensive_DocumentReference_CH](http://fhir.ch/ig/ch-epr-mhealth/StructureDefinition-ch-mhd-query-comprehensive-documentreference.html)
against releveant XDS specifications.

See also [generic XDS-FHIR mapping](Generic XDS-FHIR mapping.md).

### masterIdentifier
Mapped to `DocumentEntry.uniqueId`. In XDS, it's an OID (*1.3.6.1.4.1.21367.2005.3.7*); in FHIR, it's an URN-encoded OID (*urn:oid:1.3.6.1.4.1.21367.2005.3.7*) with the system *urn:ietf:rfc:3986*.

### identifier
Mapped to `DocumentEntry.entryUUID`. How?<br>
The entryUUID is mapped to Identifier.value.
Do we need a type/system to specify it's the entryUUID (the identifier is 1..\*? How to select the entryUUID in multiple identifiers?
=> If it's implementation dependant, the MHD sender is unable to know what will be the mapped entryUUID, that's bad.
Restrict to 1..1 and UUID?

### status
Mapped to `DocumentEntry.availabilityStatus`.

| DocumentReference.status | DocumentEntry.availabilityStatus |
| ------------ | ------------ |
| current | urn:oasis:names:tc:ebxml-regrep:StatusType:Approved |
| superseded | urn:oasis:names:tc:ebxml-regrep:StatusType:Deprecated |

### type
Mapped to `DocumentEntry.typeCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.

### category
Mapped to `DocumentEntry.classCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.

### subject
Mapped to `DocumentEntry.patientId`. The referenced CH Core Patient Profile is provided in the Bundle.

### author
Aweful mapping. Mapped to `DocumentEntry.author`.

### authenticator
Mapped to `DocumentEntry.legalAuthenticator`. XCN (HL7 V2.5 Extended Person Name) to Reference(CH Core Practitioner Profile | CH Core Practitioner Role Profile). Optional, so not so important for a first mapping.

## description
Mapped to `DocumentEntry.comments`.

## securityLabel
Mapped to `DocumentEntry.confidentialityCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.

## ch-ext-deletionstatus
Mapped to `DocumentEntry.deletionStatus`. Value set on both sides.<br>

## originalProviderRole
⚠️ Absent from the current MHD profiles.

## date
⚠️ Absent from XDS

## content.attachment.contentType
Mapped to `DocumentEntry.mimeType`. 

## content.attachment.language
Mapped to `DocumentEntry.languageCode`. Value set on both sides.<br>

## content.attachment.size
Mapped to `DocumentEntry.size`. The size is calculated on the data prior to base64 encoding, if the data is base64 encoded.

## content.attachment.hash
Mapped to `DocumentEntry.hash`. The hash is encoded in FHIR in base64Binary, whereas in XDS hexbinary is used. The hash is calculated on the data prior to base64 encoding, if the data is base64 encoded.

## content.attachment.title
Mapped to `DocumentEntry.title`. 

## content.attachment.creation
Mapped to `DocumentEntry.creationTime`.<br>
⚠️ XDS and MHD allow for different date formats.

## content.format
Mapped to `DocumentEntry.formatCode`. Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.

## context.event
Mapped to `DocumentEntry.eventCodeList`.

## context.period
Mapped to `DocumentEntry.serviceStartTime` and `DocumentEntry.serviceStopTime`.

## context.facilityType
Mapped to `DocumentEntry.healthcareFacilityTypeCode`.Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.

## context.practiceSetting
Mapped to `DocumentEntry.practiceSettingCode`.Value set on both sides.<br>
⚠️ XDS requires the value, display name and coding scheme. Those are optional in MHD.

## context.sourcePatientInfo
Mapped to `DocumentEntry.sourcePatientInfo` and `DocumentEntry.sourcePatientId`.
In FHIR, it's a contained CH Core Patient Profile resource.<br>
⚠️ `DocumentEntry.sourcePatientInfo` is way too hard to map, but it's optional.

| XDS | FHIR | Comment |
| ------------ | ------------ | ------------ |
| sourcePatientId.CX.1 | Patient.identifier[use[@value="usual"]].value[@value] |  |
| sourcePatientId.CX.6.HD.2 | Patient.identifier[use[@value="usual"]].system[@value] | Code system mapping |

## context.encounter
> When referenceIdList contains an encounter, and a FHIR Encounter is available, it may be referenced.
> 
Mapped to `DocumentEntry.referenceIdList`. Not used in CH-EPR?

## context.related
> Use .identifier element to hold referenceIdList identifier(s).
> May use .reference element when the identifier has a known FHIR resource id.

Mapped to `DocumentEntry.referenceIdList`.


### others
`DocumentEntry.repositoryUniqueId` is not mapped. Other FHIR properties are unmapped and will be lost.


## cardinalities

| Property | MHD Provide | MHD Query | XDS sending | XDS responding | Comment |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| masterIdentifier | 1..1 | 1..1 | R | R | ✔️ OK |
| author | 0..*	| 0..*	| R | R | ⚠️ Incompatible |
