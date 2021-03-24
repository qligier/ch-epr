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
:warning: In XDS, the status is always *Approved*; anything else should be rejected.

| DocumentManifest.status | SubmissionSet.availabilityStatus |
| ------------ | ------------ |
| current | urn:oasis:names:tc:ebxml-regrep:StatusType:Approved |
| superseded | urn:oasis:names:tc:ebxml-regrep:StatusType:Deprecated |
| entered-in-error | urn:oasis:names:tc:ebxml-regrep:StatusType:Withdrawn ? |

### type
Mapped to `SubmissionSet.contentTypeCode`.<br> There is a CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.contentTypeCode.html).

### subject
Mapped to `SubmissionSet.patientId`. How?<br>
:warning: IHE ITI MHD 4.5.1.2 says: 
> IHE constraint Reference(Patient). Not a contained resource. URL Points to an existing Patient Resource representing Affinity Domain Patient.

### created
Mapped to `SubmissionSet.submissionTime`. See dates mapping below.

### author
Aweful mapping.<br>
Swiss modification: SubmissionSet.Author.AuthorRole is required. See CH-EPR [value set](http://fhir.ch/ig/ch-epr-term/ValueSet-SubmissionSet.Author.AuthorRole.html).

### recipient
Aweful mapping.

### source
Mapped to `SubmissionSet.sourceId`.

### description
Mapped to `SubmissionSet.title`.

### text
Mapped to `SubmissionSet.comments`. Raw text in XDS, XHTML in FHIR. <br>
:warning: Tags should be forbidden in FHIR.<br>
:warning: Narrative.status is required in FHIR, absent from XDS.

### content
Contains other resources.

### meta.profile
Mapped to `SubmissionSet.limitedMetadata`. Will probably not be used in CH-EPR.

### others
`SubmissionSet.homeCommunityId` is not mapped (it's probably not needed). Other FHIR properties are unmapped and will be lost.

### cardinalities

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
| text | 0..1 | O | O | :heavy_check_mark: OK |

## Mapping DocumentReference to DocumentEntry

## Dates mapping

HL7's DTM shall be encoded in the format `YYYY[MM[DD[HH[MM[SS[.S[S[S[S]]]]]]]]][+/-ZZZZ]`. It allows various precision levels and the choice of time zone.
The dateTime format is different: `YYYY`, `YYYY-MM`, `YYYY-MM-DD`, `YYYY-MM-DDThh:mm:ss+zz:zz` or `YYYY-MM-DDThh:mm:ss.sssZ`. When using the time precision, the timezone is mandatory.
Guidance is required to map e.g. `YYYYMMDDHH` to FHIR dateTime (use the earliest instant covered by the partial date?).
