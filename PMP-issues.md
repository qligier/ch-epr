# Current issues encountered by the primary eMedication aggregator

## ITI-43 audit message

For the document repository, the [audit message](https://ihe.github.io/publications/ITI/TF/Volume2/ITI-43.html#3.43.6) does not describe the human requestor(s). But the EPDV Annex 5.1 says:

> Whenever a transaction was secured by XUA, the corresponding ATNA record **MUST** include the following set of <ActiveParticipant> elements related to involved users

Is this incompatible? Are we allowed to add content to an audit message? Should we fix ITI-43 specification?

## ITI-92 cannot update the deletionStatus attribute

ITI-57 (Update Document Set) does not allow cross-community calls, so we have to use ITI-92 (Restricted Update Document Set). But its specification specifically says (§48.4.1):

> In order to maintain interoperability among participating communities, certain metadata attributes are restricted from modification as they describe the current state of DocumentEntry object, or the stored physical document. The RMU Profile permits updating the following metadata attributes: [...] Other IHE Profiles, XDS Affinity Domain policies, or community policies may impose additional restrictions on the metadata attributes they allow to be changed.

The attribute _deletionStatus_ cannot be updated by an ITI-92 query, it means a document cannot be deleted in a cross-community transaction.

## Usage of deletionStatus attribute

The usage of the attribute _deletionStatus_ is not described by EPDV Annex 5.1. Who's allowed to set it/update it? On which documents? What is the use case for _deletionProhibited_? What is the by law determinate time period?

## Content of the medication plan

What goes into the medication plan? Which drug categories (i.e. dangerous/protected drugs, drug trials)? Are prescrivable non-drugs allowed (e.g. crutches)?

## Drug database

Do we need to connect the aggregator to a drug database (compendium)? If it shall reject 'protected' drugs or non-drugs, then surely yes.

## Patient rights

What can a patient do? Can they create a PRE or DIS? Is there limitations depending on the drug type? (e.g. preventing patients from creating MTPs for anything else than over-the-counter drugs). Can they specify a drug that has no GTIN (i.e. not in the compendium)?

## Medication card

A template has to be created, with the rules to generate it (order, grouping, etc.). A special effort has to be made on dosing forms.

## XUA

Is there requirements for the XML signature (i.e. CanonicalizationMethod, SignatureMethod, Transforms, DigestMethod)?

Is there requirements for AuthnContextClassRef? How to process AuthnStatement/@SessionNotOnOrAfter with Conditions/@NotOnOrAfter? How to process Audience when it's not _urn:e-health-suisse:token-audience:all-communities_?

## Document IDs management

How to deal with document IDs in respect to DocumentEntry vs DocumentReference, CDA vs FHIR/XML vs FHIR/JSON? Same ID for the three? Three IDs? Two IDs (CDA and FHIR)? No good solution, issues with references between documents (a CDA that references a FHIR by example), issues with translating between CDA and FHIR (keep or translate the IDs?).

E.g. to choose document type, need to use differents IDs in ITI-43 but should use the same ID in ITI-68.

Whatever the choice, there will be multiple issues and none of the solutions seems to be better than the others.

## POC scope

What's the scope of the POC?

## Other issues

[CDA-CH-EMED](https://art-decor.org/art-decor/decor-issues--cdachemed-), [CH-PHARM](https://art-decor.org/art-decor/decor-issues--ch-pharm-), [CH-EPR](https://art-decor.org/art-decor/decor-issues--ch-epr-), [CH-EMED](https://github.com/ehealthsuisse/ch-emed/issues), [CH-EPR-mHealth](https://github.com/ehealthsuisse/ch-epr-mhealth/issues)
