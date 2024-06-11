# The publication standard

The future publication standard (occasionally referred to as "Standard 2.0") can be considered as the centrepiece of the official gazette portal.

> [!TIP]
> To get a better understanding of the interaction between the individual artifacts, it is recommended to read ["Big Picture"](https://github.com/officialgazette/big-picture) first.

> [!IMPORTANT]
> The existing concepts and artifacts should be challenged and revised. An optimal standard is to be drafted as part of the public tender. In particular, it should
>- ensure the interoperability of the future system
>- be scalable (e.g. with regard to the inclusion of new tenants or new publication types)
>- be developer-friendly 



## Why a standard?

The growing number of official publications in the official gazette portal shows that the introduction of a standard that applies to all possible types of publications is unavoidable. A standardised exchange structure offers a number of advantages, such as:

- A uniform standard ensures the interoperability of the system.  It furthermore ensures that the publications remain readable and processable at a future time.
- The import and export of publications is simplified and each publication type can be processed in the same way.
- The schemes are easier to read once the basic principles have been understood.
- The new publication structure should follow universal semantics that apply to all publication types. Wherever possible, the schema structure should be based on existing eCH standards.

## Considerations regarding the existing schema

In recent years, various preparatory work has been carried out for the introduction of a new, holistic standard. In particular

* A minimum set of metadata common to all publications was defined. This set enables (and should enable in a future solution) a standardised search of both published and archived publications. Information regarding the existing meta data schema can be found [here](https://amtsblattportal.ch/docs/api/#_api_reference).

* a generic structure for message content was created. This makes it possible to deliver and obtain publications of very different types and from several public authorities in a standardised structure. More Information regarding the generic content structure can be found [here](https://amtsblattportal.ch/docs/api/#_publication_schema_2_0).



## The scope of the Standard
The field of action with regard to standardization comprises various aspects. The following overview shows which artifacts are expected for the future standard.

```mermaid

%%{
  init: {
    'theme': 'neutral'
  }
}%%

block-beta

block:scope["Scope"]

columns 3
block:standard["Standard
of publication"]:1
columns 1
Meta
space
space
Content
end
Meta space Terms["Tenant
specific configuration"]
space:3
Content space termDB[("
Term
Database")]

end



Terms --> Content
termDB --> Terms
```
The individual artifacts are described in detail as follows.

### The general structure of a publication

> [!TIP]
> In the existing solution, the structure schema is available in XSD format, [see here](https://github.com/officialgazette/publication-standard/blob/main/publication_schema.xsd).

The structure of the publication describes how a publication is organized and can be requested and submitted via the API. A distinction must be made between

* The meta data: Meta data should be as identical as possible for each publication type, and they should be used for both expired (possibly archived) and future publications.

* The content data: Content data varies significantly depending on the type of publication. Therefore, a solution had to be found to keep the basic structure stable but adapt the actual content data for each publication type.

### The meta data
The structure and elements of the existing meta data schema is documented [here](https://amtsblattportal.ch/docs/api/#_api_reference). For reasons of downward compatibility, it may be a good approach to keep this structure in a future solution/standardisation.

### The content data

**Semantic structuring**

The current content schema consists of four elements that give the publication a semantically usable structure:

* controls

Contains the configuration controls as boolean values (e.g.: "apiImport" must be set "true" when importing a publication via API)

* location

Describes the assignment of the publication to a municipality. If a message cannot be assigned to a municipality, this element remains empty.

* action

Describes the nature of a publication

* businessCase

Describes the business case on which the publication is based.

* reaction

Describes the possible reactions to a publication


**The content structure**

The following figure shows the content structure of a publication:

![Schema](https://amtsblattportal.ch/docs/api/images/publication_schema_overview.png)

Each element has an element key and a term in German or French (a future solution should provide Italian and English as well, see "general terms catalogue"). 
The element is also typified by means of a "valueType" element. The following schema excerpt shows the possible elements.

**key:** Contains the key of the term. This uniquely identifies the term, even if the term is renamed on a client-specific basis (for example, a "building application" may be called a "building project", depending on the publication organ).

**type:** Describes the type of term from a technical perspective:
- action
- businessCase
- businessTerm
- enumValue
- reaction
- municipalityId

**valueType:** Describes the type of term from a technical/structural perspective:
- text
- textNeutral
- richtext
- int
- enum
- date
- dateFromTo
- datetime
- address
- legalPerson
- naturalPerson
- naturalPersonLight
- naturalLegalPerson
- deceasedPerson
- url
- attachment

These types are mapped in the XML schema, further information on the structure can be found in the [API doc of the official gazette portal](https://amtsblattportal.ch/docs/api).
  
### The terms catalogue 
The collection of general terms (also referred to as the term database) includes all terms that can be used in publication types. These terms are maintained in German, French, Italian and English and have a unique key.

**Structure of a term**
The terms catalogue can currently be accessed in JSON format at the following URL: https://amtsblattportal.ch/terms 
A term object ("term") contains various information and is structured as follows:
```
{
"key" : "constructionProject",
"valueType" : "text",
"term" : {
  "de" : "Planning application",
  "fr" : "Demande d'autorisation de construire"
  }
}
```


> [!TIP]
> In the existing solution, the terms catalog is structured in a publication type-centered manner. This concept should be challenged. The catalog structure can and should be revised as part of the new concept.

### Tenant-specific configurations
Publishers of an official gazette should be able to configure their publication types autonomously. The exchange of this configuration should also be standardized.
These configurations include in particular
  - Assignment and editing of preconfigured default publication types from the term database
  - Individualization of publication type naming
  - Individualization of term naming
  - Legal remedies of the specific publication types
  - Publication period
  - Archiving requirements

An example of a current client-specific configuration (Canton Valais) can be found (here)[https://amtsblattportal.ch/terms/kabvs]


