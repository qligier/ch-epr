# XDS-FHIR resources mapping in CH-EPR

See [DocumentManifest-SubmissionSet mapping](DocumentManifest-SubmissionSet mapping.md) and [DocumentReference-DocumentEntry mapping](DocumentReference-DocumentEntry mapping.md).

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
