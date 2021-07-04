# Current issues encountered by the primary eMedication aggregator

## ITI-43 audit message

For the document repository, the [audit message](https://ihe.github.io/publications/ITI/TF/Volume2/ITI-43.html#3.43.6) does not describe the human requestor(s). But the EPDV Annex 5.1 says:

> Whenever a transaction was secured by XUA, the corresponding ATNA record **MUST** include the following set of <ActiveParticipant> elements related to involved users

Is this incompatible? Are we allowed to add content to an audit message? Should we fix ITI-43 specification?

## ITI-92 cannot update the deletionStatus attribute

ITI-57 (Update Document Set) does not allow cross-community calls, so we have to use ITI-92 (Restricted Update Document Set). But its specification specifically says (§48.4.1):
  
> In order to maintain interoperability among participating communities, certain metadata attributes are restricted from modification as they describe the current state of DocumentEntry object, or the stored physical document. The RMU Profile permits updating the following metadata attributes: [...] Other IHE Profiles, XDS Affinity Domain policies, or community policies may impose additional restrictions on the metadata attributes they allow to be changed.
  
The attribute _deletionStatus_ cannot be updated by an ITI-92 query.
