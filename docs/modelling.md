# CORDH Data Harmonisation Group  


  
  
----

*CRM modelling & examples*

The document outlines modelling choices in relation to diverse entities described in the Consortium's data. The modelling is provided, whenever possible, using CIDOC-CRM. However, in certain cases, other ontologies are used. 
For each of the described entities diverse encoding choices were outlined, and a decision about each of them was made. The scope of this document is not to provide a track of this process, but just to present its results. The documentation of the possible modelling and encoding of an entity in RDF can be found in the [Annex A](#).

For each entity, as an example of its use, an RDF encoding of a real-world statement is provided. The syntax of the encoding is Turtle, and all the prefix used can be found on [http://prefix.cc/](http://prefix.cc/).

## Appellation 

#### Note

An appellation is a sign, usually in the form of a string of characters, used for referencing an entity (person object, place, an event etc.). More appellations can refer, in the same language, to the same entity, as in the case of:

> Mona Lisa or La Gioconda. which both refers to the same painting

Appellations can also be tied to a specific time interval, such as in the case of:

> Prince, Alexander Nevermind, The Artist, The Artist Formerly Known as Prince (TAFKAP), Camille, Christopher, Tracy Jamie Starr, Joey Coco, Tora Tora, The Kid — which refers to Prince Rogers Nelson in diverse moments of his career

However, If more appellations can coexist, a chosen one should be defined using a different property which identifies it with our <u>preferred</u> appellation.

### Modelling the Appellation

A simple appellation can be modelled in CRM as

**E21 Person → P1 is identified by → E41 Appellation**

The encoding of the string of characters of the appellation can be done in multiple ways. The form chosen by the consortium is:

**crm:E21 Person → P1 is identified by → E41 Appellation → rdfs:Label → rdfs:Literal**

```turtle
<http://vocab.getty.edu/ulan/500020981> 
a  crm:E21_Person ;
crm:P1_is_identified_by <https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9> .
<https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9>
a  crm:E41_Appellation ;
rdfs:label "Gian Francesco de Maineri" .
```

*<small>Codebox 1  - Appellation</small>*

**Modelling Note**

> *The use of crm:P3_has_note for encoding the value of the appellation is discouraged because of its confusing nature. Appellations can have both notes and values, which are part of two different information domains, and should not overlap. Additionally, the property rdf:value, which can be also be used to encode the appellation, is to be set aside because of its lack of explicit semantics.*


## Identifier

#### Note

An identifier is a string assigned to an entity that uniquely identifies it in a system (usually, a digital system). Examples of identifiers comprise inventory numbers, databases keys or other strings used for retrieval and identification purpose. As for the appellation, an entity can have multiple identifiers from the same or different system, and a preferred one can be chosen.

### Modelling the Identifier

A simple identifier can be modelled in CRM as

**E21 Person → P1 is identified by → E42 Identifier**

The encoding of the string of characters of the identifier can be done in multiple ways. The form chosen by the consortium is:

**E21 Person → P1 is identified by → E42 Identifier → rdfs:Label → rdfs:Literal**

```turtle
<http://vocab.getty.edu/ulan/500020981> 
a  crm:E21_Person ;
crm:P1_is_identified_by <https://collection.itatti.harvard.edu/resource/identifier/6c5a29fd8> .
<https://collection.itatti.harvard.edu/resource/identifier/6c5a29fd8>
a  crm:E42_Identifier ;
rdfs:label "6c5a29fd8" .
```


*<small>Codebox 2  - Identifier</small>*


## Preferred Appellations & Identifiers

The preference for a specific sign or string for identifying an entity will be modelled using type. 
In respect to the encoding of the [appellation](#appellation) or the [identifier](#identifier), the initial part of the modelling remain the same:

**E21 Person → P1 is identified by → E41 Appellation**
**E21 Person → P1 is identified by → E42 Identifier**

The preference towards a specific appellation or identifier has to be defined using a E55 Type:

**E21 Person → P1 is identified by → E41 Appellation → P2 has type → E55 Type**

It is advisable to use a standard vocabulary to express the preferred type, such as the one from the Art and Architecture Thesaurus: http://vocab.getty.edu/page/aat/300404670.

```turtle
<http://vocab.getty.edu/ulan/500020981> a crm:E21_Person ; 
crm:P1_is_identified_by <https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9> . 

<https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9> a crm:E41_Appellation ; 
rdfs:label "Gian Francesco de Maineri" .
crm:P2_has_type "http://vocab.getty.edu/page/aat/300404670"^^xsd:anyURI .
```
*<small>Codebox 3  - Preferred Appellation</small>*

If one appellation is represented using diverse alphabets (Latin, Chinese, Greek, Russian..), it would be treated as the same E41 Appellation, and the different strings encoded with rdfs:label. In this case only,skos:prefLabel can be used to define the preferred textual version of the same appellation.


```turtle
<http://vocab.getty.edu/ulan/500318548> a crm:E21_Person ; 
crm:P1_is_identified_by <https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9> . 

<https://collection.itatti.harvard.edu/resource/appellation/6c5a29fd81f9> a crm:E41_Appellation ; 
skos:prefLabel "Gogol, Nikolay" ,
rdfs:label "Гоголь, Николай Васильевич" ,
rdfs:label "果戈里" .
```

*<small>Codebox 4  - Alphabetic differences</small>*

**Modelling Note**

> *CRM does provide a property for encoding a preferred identifier, P48 has preferred identifier, but the range is E42 Identifier, a subclass of E41 Appellation, therefore it cannot be used for encoding the preferred appellation. While it can be used at least for the preferred we opted for a standard solution for both classes*
> While the use of skos:prefLabel was considered, it was easily rejected because if a person has multiple appellations, skos:prefLabel would not identify the preferred one, but only the preferred string of text of that specific appellation.

## Language for Literals
*To be completed*


## Dimension/Quantity

#### Note

Dimension are measurable data which results from an observation. The outcome of an observation is always to be seen as a couple value/system where the meaning of the value is dependant on the system used.

Dimensions can be modelled in CRM as:
**E54 Dimension → P91 has unit →  E58 Measurement Unit**
**E54 Dimension → P90 has value →  E60 Number**

The modelling above is based on the differentiation between the value and the system which provide meaning to the value. 

### Modelling Dimension

When talking about Dimension it is paramount to use an external recognisable authority to define the system which provides meaning to the value.
In order to define the measurement units and the quantity kind (length, weight) we rely on E55 Type and the vocabulary provided with the Ontology of units of measure (OM). 

```turtle
<https://collection.itatti.harvard.edu/resource/dimension/width> a crm:E54_Dimension ;
crm:P90_has_value    "6"^^xsd:decimal ;
crm:P91_has_unit <http://www.ontology-of-units-of-measure.org/resource/om-2/centimetre> ;
crm:P2_has_type <http://www.ontology-of-units-of-measure.org/resource/om-2/Width> .

<https://collection.itatti.harvard.edu/resource/dimension/lenght> a crm:E54_Dimension ;
crm:P90_has_value    "12"^^xsd:decimal ;
crm:P91_has_unit <http://www.ontology-of-units-of-measure.org/resource/om-2/centimetre> ;
crm:P2_has_type <http://www.ontology-of-units-of-measure.org/resource/om-2/Length> .
```

OM includes all the necessary dimensions required by our use cases. It is easy to browse, and well developed.

Other ontologies were considered, specifically QUDT, which is "Quantity, Unit, Dimension and Type" schema used for modelling physical measurable properties in a machine processable form. While QUDT was the initial candidate, it was not chosen because the browsing of the units was quite difficult and, moreover, it is still under development and not yet complete or stable.   

OM can be easily browsed here:
[http://www.ontology-of-units-of-measure.org/]([http://www.ontology-of-units-of-measure.org)

## Time-span

#### Note

CIDOC-CRM assume the existence of a SpaceTime dimension, where each temporal entity can be mapped to. This system allows the computing of diversity, closeness or equality in time between events. In doing so, it treats temporal extents in their physical sense, characterising each time interval with a beginning, an end and a duration. Additionally, CIDOC-CRM assumes that our knowledge of the time intervals of a historical event has a diverse degree of certainty, and it allows us to record such certainty/uncertainty with specific properties. A useful summarization of the treatment of time in CIDOC-CRM is available in the document [Time](time.html).  

Following CIDOC-CRM we will model time-related assertions as certain or uncertain, where the certain statements are going to be model using the property P81, while the uncertain statements using the property P82. The level of certainty has to be determined by the researcher carrying out the work. 

We can have two diverse modelling scenario:

1. Fuzzy time statements
2. Precise time statements

In both cases the date is formatted following the [ISO 8601:2004](https://www.iso.org/standard/40874.html) recommendations that prescribe the composition of the date using the format:

> YYYY-MM-DD

### Fuzzy Time Statements

Fuzzy time statements are modelled using CIDOC-CRM as:

- **E52 Time-Span → P82a begin of the begin → xsd:dateTime**
- **E52 Time-Span → P82b end of the end → xsd:dateTime**

<!---
Change to dateTime
-->

The date needs to be expressed with its datatype (date or dateTime). See the [W3C reccomandations](https://www.w3.org/TR/xmlschema-2/#isoformats) for more details.

```turtle
<https://collection.itatti.harvard.edu/resource/work/W00060289/production/timespan>
a  crm:E52_Time-Span ;
crm:P82a_begin_of_the_begin  "1401-01-01"^^xsd:date ;
crm:P82b_end_of_the_end      "1700-12-31"^^xsd:date .
```

*<small>Codebox 5 - Time Statements</small>*

**Modelling Note**

> *The production event recorded here has a time interval of almost 3 centuries. The choice of using a fuzzy time statement is, in this case, quite clear. *

### Precise time statements

Precise time statements are modelled using CIDOC-CRM as:

- **E52 Time-Span → P81a end of the begin → xsd:dateTime**
- **E52 Time-Span → P81b begin of the end → xsd:dateTime**

```turtle
<https://collection.itatti.harvard.edu/resource/work/W00060289/production/timespan>
a  crm:E52_Time-Span ;
crm:P81a_begin_of_the_begin  "2001-10-21T21:32:52"^^xsd:dateTime;
crm:P81b_end_of_the_end      "2001-10-26T00:00:00"^^xsd:dateTime.
```

*<small>Codebox 6 - Precise Time Statements</small>*

**Modelling Note**

> *The precision of the date is a clear sign we are dealing with a more precise time statement.*



## Spatial Coordinate

#### Note

A place is a section of space identified independently from its temporal status. They are usually referenced using absolute (WGS, EPSG) or relative reference systems. Absolute reference system uses point-based coordinates to identify the representation of a space.



### Longitude & Latitude

The spatial coordinates of a place can be modelled in CIDOC-CRM as 

**E53 Place → P168 place is defined by → E94 Space Primitive**

The encoding of the Space Primitive can be done using GML/WKT and other ways. WKT is the form chosen by the consortium because of its compactness, case insensitivity and cross-domain support. The codebox below is an example of the encoding of a Place  using wktLiteral:

```turtle
<https://collection.itatti.harvard.edu/resource/ex/place>
        a crm:E53_Place ;
        crm:P168_place_is_defined_by  "POINT (9.1232696 45.2503146)"^^geo:wktLiteral ;
```
*<small>Codebox 7</small>*

However, sometimes WKT is not accepted and providing separate attribute for latitude and longitude is necessary. In this case we rely on locn:geometry property from the [ISA Programme Location Core Vocabulary](https://www.w3.org/ns/locn#locn:Geometry) for defining the geometry of the place and [W3C-Basic-Geo](https://www.w3.org/2003/01/geo/) for specifying the latitude and longitude.


```turtle
<https://collection.itatti.harvard.edu/resource/ex/place>
        a crm:E53_Place ;
        crm:P168_place_is_defined_by  "POINT (9.1232696 45.2503146)"^^geo:wktLiteral ;
         locn:geometry [
      geo:lat "51.477811" ;
      geo:long "-0.001475"
    ] .
```
*<small>Codebox 8</small>*






