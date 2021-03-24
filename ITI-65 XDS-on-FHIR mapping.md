# XDS on FHIR mapping in CH-EPR

This is a draft to map FHIR resources to equivalent XDS resources in the context of a transaction with XDS on FHIR option, in CH-EPR. XDS cardinalities incorporate Swiss requirements.

[XDS-FHIR-mapping](https://wiki.ihe.net/index.php/XDS-FHIR-mapping "XDS-FHIR-mapping") by IHE, [Resource DocumentManifest - Mappings](https://www.hl7.org/fhir/documentmanifest-mappings.html "Resource DocumentManifest - Mappings") and [Resource DocumentReference - Mappings](https://www.hl7.org/fhir/documentreference-mappings.html "Resource DocumentReference - Mappings") by HL7.

## Mapping DocumentManifest to SubmissionSet

– **masterIdentifier**<br>
Mapped to `SubmissionSet.uniqueId`. In XDS, it's an OID (*1.3.6.1.4.1.21367.2005.3.7*); in FHIR, it's an URN-encoded OID (*urn:oid:1.3.6.1.4.1.21367.2005.3.7*) with the system *urn:ietf:rfc:3986*.<br>
:warning: Cardinalities are not aligned.

– **identifier**<br>
Mapped to `SubmissionSet.entryUUID`. How?<br>
:warning: Cardinalities are not aligned.

– **status**<br>
Mapped to `SubmissionSet.availabilityStatus`.<br>
:warning: In XDS, the status is always *Approved*; anything else should be rejected.

| DocumentManifest.status | SubmissionSet.availabilityStatus |
| ------------ | ------------ |
| current | urn:oasis:names:tc:ebxml-regrep:StatusType:Approved |
| superseded | urn:oasis:names:tc:ebxml-regrep:StatusType:Deprecated |
| entered-in-error | urn:oasis:names:tc:ebxml-regrep:StatusType:Withdrawn ? |

– **type**<br>
Mapped to `SubmissionSet.contentTypeCode`.<br> In CH-EPR, a [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.contentTypeCode.html) is used to restrict this property.
:warning: Cardinalities are not aligned.

– **subject**<br>

– **created**<br>

– **author**<br>

– **recipient**<br>

– **source**<br>

– **description**<br>

– **content**<br>
Mapped to `SubmissionSet.title`.

– **others**<br>
`SubmissionSet.comments` and `SubmissionSet.homeCommunityId` are not mapped.

– **cardinalities**<br>

| Property | FHIR | XDS sending | XDS responding | Comment |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| masterIdentifier | 0..1 | R | R | :warning: Incompatible for sending actor |
| identifier | 0..*	| R | R | :warning: Incompatible for sending actor |
| status | 1..1 | O | R | :heavy_check_mark: OK |
| type | 0..1 | R | R | :warning: Incompatible for sending actor |
| subject | 0..1 | R | R | :warning: Incompatible for sending actor |
| created | 0..1 | R | R | :warning: Incompatible for sending actor |
| author | 0..*	| R | R | :warning: Incompatible for sending actor |
| recipient | 0..* | O | O | :heavy_check_mark: OK |
| source | 0..1 | R | R | :warning: Incompatible for sending actor |
| description | 0..1 | O | O | :heavy_check_mark: OK |

## Mapping DocumentReference to DocumentEntry
