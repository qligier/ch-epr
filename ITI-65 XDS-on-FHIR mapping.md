# XDS on FHIR mapping for ITI-65 transaction in CH-EPR (MHD Provide Document Bundle)

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

## Mapping DocumentReference to DocumentEntry
