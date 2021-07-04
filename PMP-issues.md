# Current issues encountered by the primary eMedication aggregator

# ITI-43 audit message

For the document repository, the [audit message](https://ihe.github.io/publications/ITI/TF/Volume2/ITI-43.html#3.43.6) cannot contain the human requestor(s). But the EPDV Annex 5.1 says:

> Whenever a transaction was secured by XUA, the corresponding ATNA record **MUST** include the following set of <ActiveParticipant> elements related to involved users
  
This is incompatible and should be fixed in the ITI-43 specification.
