#  Art history Data

*CRM modelling & examples*




## Name

A name is an identifier of a person which refers to an agent in a specific moment in time (it is important to remember that it can change over time, see the case of _Prince_). A person's name in CRM can be model differently accordingly to our intention, as well as on the granularity of the information we want to map.

More than one name can be linked to one person, and one person can be known with diverse name, both during its life and after. The very first thing we should ask ourself is if the name we are modelling is the preferred name of that person or not. We will see examples of both.

### Preferred Name

Possible modelling of the preferred name are:

1. **E21 Person → L4 has preferred label → rdfs:Literal**
2. **E21 Person → skos:prefLabel → rdfs:Literal**

The main difference is that the 1 statements is encoded using CIDOC-CRM and CRMDig, while the second use SKOS. The form preferred by I Tatti is the number 2 (_see the code block_). The reason is mainly because it employs SKOS, which we use in other part of the modelling. Moreover SKOS is more used globally.

```xml-dtd
skos:prefLabel "Giovanni da Udine" .
```


### Name

Possible modelling of the name are:

1. **E21 Person → P1 is identified by → E41 Appellation → P3 has note → rdfs:Literal**
2. **E21 Person → rdfs:Label → rdfs:Literal**
3. **E21 Person → P1 is identified by → rdfs:Literal**

The main differences between the three is that example 1 treat the appellation not as a literal but as an entity which has a literal. Thus we can express statements about E41 Appellation, such as when it was used, how it come to be, where this form originate and others of such type.

The form chosen by I Tatti is the number 3 (_see the code block_). The reason is very practical. There was no need to express statements about the name itself, and the use of rdfs:label is preferable as a system level (_metaphacts_) in respect to crm:P1_is_identified_by .


```xml-dtd
a crm:E21_Person ;
rdfs:label "Giovanni da Udine" .
```



## Identifier

A object can has multiple identifiers which we want to link to. An easy example would be Michelangelo, which has an internal identifier, a page on wikidata with his own identifier, as well as another one on ULAN. We can chose to define them and chose a preferred one.

Possible modelling in CRM are:

1. **E21 Person → P48 has preferred identifier → rdfs:Literal** (_see the code block number 1_)
2. **E21 Person → P1 is identified by→ rdfs:Literal** (_see the code block number 2_)

```xml-dtd
crm:P1_is_identified_by "B00000737" ;
```


```xml-dtd
crm:P48_has_preferred_identifier "A00000737" ;
```



## Time

Time is treated quite profoundly in CRM and diverse can be the time representations as well as time expression that the model offer. A possible summarisation of these approaches is outside the scope of the document but the one interested could consult the resource **list resources**

Such a diverse approach allow for a great diversity in the mapping:

1. **E12 Production→ P4 has time-span → E52 Time-Span → P79 beginning is qualified by → xsd:gYear / E12 Production→ P4 has time-span → E52 Time-Span → P80 end is qualified by → xsd:gYear**

```xml-dtd
 a crm:E225_Time-Span ;
crm:P79_beginning_is_qualified_by "1401"^^xsd:gYear ;
crm:P80_end_is_qualified_by  "1700"^^xsd:gYear .
```



## Place

Place is another important feature we should harmonise. Diverse are the possibilities, both inside and outside CRM. The latter offers an extension called CRMGeo that harmonise CRM with GeoSparql and offer a nice representation of the geographical features with a rich semantics. However the adoption is limited and the learning curve is steep. Other option are offered in CRM, which allow the expression of simple statements about place.

The identified mapping options are:

1. **E53 Place → P87 is identified by →  E47 Spatial Coordinates →  rdfs:Label →  rdfs:Literal**
2. **wgs:SpatialThing→ wgs:lat →  rdfs:Literal / wgs:SpatialThing→ wgs:long →  rdfs:Literal**
3. **E53 Place→ P87 is identified by → wgs:Point→ wgs:lat →  rdfs:Literal**

We will see them in more details

1. **E53 Place → P87 is identified by →  E47 Spatial Coordinates →  rdfs:Label →  rdfs:Literal**

   This mapping actually require the specification of the Type of the Spatial Coordinates we are  annotating a place with, such as "Latitude" or "Longitude". Other options could be "WGS84" or other reference system.

| Entity    | Relationship         | Entity                  | Relationship | Entity       |
| --------- | -------------------- | ----------------------- | ------------ | ------------ |
| E53 Place | P87 is identified by | E47 Spatial Coordinates | rdfs:Label   | rdfs:Literal |
|           |                      |                         | P2 has type  | rdfs:Literal |


```xml-dtd
 a crm:E53_Place ;
crm:P87_is_identified_by  ,  .


a crm:E47_Spatial_Coordinates;
rdfs:label "11.8818051" ;
crm:P2_has_type “Longitude" .


a crm:E47_Spatial_Coordinates;
rdfs:label "60.1701801" ;
crm:P2_has_type “Latitude" .
```
The solution is complex and require the use of type for something which should be properly expressed. Moreover, another type would be use if the reference system should be specified.



2. **wgs:SpatialThing→ wgs:lat →  rdfs:Literal / wgs:SpatialThing→ wgs:long →  rdfs:Literal**

   ​

   Another solution is to map a wgs:SpatialThing, which is an entity of the Ontology Geo as a subclass of E53 Place and express the latitude and longitude relative to the place. It is a straightforward solution which, however, does confuse, as it happens in many case, what it is a place and what are the coordinate of that place. The two are very different and should not be mix together.

```xml-dtd
   a wgs:SpatialThing;
   wgs:lat “60.1701801" ;
   wgs:long “24.9419037” .
```


3. **E53 Place→ P87 is identified by → wgs:Point→ wgs:lat →  rdfs:Literal**

This mapping takes in account the declaration, at an ontology level, that wgs:Point, an entity in the ontology Geo, is a subclass of E47 Spatial Coordinates.

```xml-dtd
https://collection.itatti.harvard.edu/resource/ex/place/ a crm:E53_Place ;
crm:P87_is_identified_by https://collection.itatti.harvard.edu/resource/ex/place/point .
a wgs:Point;
wgs:lat "60.1701801" ;
wgs:long “24.9419037" .
```

This solution is, for us, the most satisfying, because it clarify the properties and it is properly mapped to CRM. Moreover Geo is, by far, the most used geographical ontology in the Linked Data  cloud [^1].

## OWL sameAs

A specific paragraph should be dedicated to owl:sameas which is probably one of the most used element in owl, and a way to connected entities between diverse graph.

```xml-dtd
https://collection.itatti.harvard.edu/resource/person/example
owl:sameas "http://vocab.getty.edu/ulan/500004700" .
```

Its popularity has made it particularly misused, and it is better to remind everyone (author included) that for linking entities which are not exactly the same[^2] other properties are available, such as skos:exactMatch which should be considered always as possible alternative.

# Bibliographic Data

We model bibliographic data using [FRBRoo](http://www.cidoc-crm.org/frbroo/), the CIDOC-CRM extension for bibliographic data. We demonstrate the modelling of a printed book, but the approach can be adapted to similar items of expressive matters, such as letters, manuscripts, recordings, etc.

Note: the information here is based on FRBRoo 2.4, which is the latest release at the time of writing. FRBRoo 3.0 is currently under development.

## References

The examples are based on the data of the [Sphaera](http://sphaera.mpiwg-beriln.mpg.de) project. The data model for this was developed following the guidelines in:

 Doerr, M., Gradmann, S., LeBoeuf, P., Aalberg, T., Bailly, R., & Olensky, M. (2013). _Final Report on EDM - FRBRoo Application Profile Task Force_. Europeana. Retrieved from <http://pro.europeana.eu/taskforce/edm-frbroo-application-profile>

A tutorial on FRBRoo is available [here](http://83.212.168.219/FRBR_Tutorial/)

## Book

In FRBRoo, a book is understood as consisting of several (CRM) entities:

- the physical book (F5 Item): an individual material copy of a book
- the manifestation (F3 Manifestation Product Type): the 'prototype' of a particular book, identifiable for example by an ISBN number
- the expression (F22 Self-Contained Expression): the text contained in a book
- the work (F1 Work): the work conveyed by a book

Using this terminology we can talk of my personal copy of Virginia Woolf's _Orlando_ (F5 Item), which is the Oxford World's Classics edition (F3 Manifestation Product Type), that contains the text edited by Michael H. Whitworth (F22 Self-Contained Expression) and conveys Vriginia Woolf's work _Orlando_ (F1 Work).

Note: In FRBRoo 2.4 we distinguish between the _F24 Publication Expression_ that includes the entire content of a book (including page numbers, TOC, etc.) and the _F22 Self-Contained Expression_ that includes the text as written by the author. This discrimination is likely to be abandoned in version 3.0.

The components are modelled as follows:

1. **F5 Item → P128 carries → F24 Publication Expression**
2. **F24 Publication Expression → P165 incorporates → F22 Self-Contained Expression**
3. **F5 Item → R7 is example of → F3 Manifestation Product Type**
4. **F1 Work → R3 is realised in → F22 Self-Contained Expression**

```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book> a frbroo:F5_Item ;
	crm:P128_carries <http://example.org/book/publication> ;
	crm:R7_is_example_of <http://example.org/book/manifestation> .

<http://example.org/book/publication> a frbroo:F24_Publication_Expression ;
	crm:P165_incorporates <http://example.org/book/expression> .

<http://example.org/book/expression> a frbroo:F22_Self-Contained_Expression .

<http://example.org/book/work> a frbroo:F1_Work ;
	frbroo:R3_is_realised_in <http://example.org/book/expression> .
```

### Author

As author we understand the actor(s) responsible for the creation of a F22 Self-Contained Expression. Therefore, the term does not convey a particular idea of authorship and can include different roles, such as translators, copyists, etc.

We model the creation of the Expression as follows:

1. **F22 Self-Contained Expression → R17i was created by → F28 Expression Creation**[^3]

The author is then represented as the actor carrying out the event

1. **F28 Expression Creation → P14 carried out by → E39 Actor**


```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/expression> a frbroo:F22_Self-Contained_Expression ;
	frbroo:R17i_was_created_by <http://example.org/book/expression/event> .

<http://example.org/book/expression/event> a frbroo:F28_Expression_Creation ;
	crm:P14_carried_out_by <http://example.org/actor> .

<http://example.org/actor> a crm:E39_Actor .
```

The typed property PC14 carried out by is used when we need to represent additional information:

1. **F28 Expression Creation → P01 is domain of → PC14 carried out by → P02 has range → E39 Actor**
2. **PC14 carried out by → P14.1 in the role of → E55 Type**


```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/expression> a frbroo:F22_Self-Contained_Expression ;
	frbroo:R17i_was_created_by <http://example.org/book/expression/event> .

<http://example.org/book/expression/event> a frbroo:F28_Expression_Creation ;
	crm:P01_is_domain_of <http://example.org/book/author/1> .

<http://example.org/book/author/1> a crm:PC14_carried_out_by ;
	crm:P02_has_range <http://example.org/actor> ;
	crm:P14.1_in_the_role_of <http://example.org/type/author>

<http://example.org/actor> a crm:E39_Actor .

<http://example.org/type/author> a crm:E55_Type .
```

### Publisher

As publisher we understand the actor(s) responsible for the creation of a F24 Publication Expression. A publisher may be the same as the printer, in which case they are also the actor responsible for carrying out the F32 Carrier Production Event.

We model the creation of the F24 Publication Expression as follows:

1. **F24 Publication Expression → R24i was created through → F30 Publication Event**

The publisher is then represented as the actor carrying out the event

1. **F30 Publication Event  → P14 carried out by → E39 Actor**


```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/publication> a frbroo:F24_Publication_Expression ;
    frbroo:R24i_was_created_through <http://example.org/book/publication/event> .

<http://example.org/book/publication/event> a frbroo:F30_Publication_Event ;
    crm:P14_carried_out_by <http://example.org/actor> .

<http://example.org/actor> a crm:E39_Actor .
```

### Date published
The publishing date is the date the F30 Publication has been carried out.

It is modelled as follows:

1. **F30 Publication Event → P4 has time-span  → E52 Time-Span**

See the section on Time for more detail on how to model time spans. In bibliographic data we typically have a year, which we model using the **P82a** and **P82b** properties. For readability, we also include the year as a rdfs:label to the E52 Time-Span.

See the following example:

```ttl
@prefix frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>.
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/>.

<http://example.org/book/publication/event> a frbroo:F30_Publication_Event ;
    crm:P4_has_time-span <http://example.org/book/publication/date> .

<http://example.org/book/publication/date> a crm:E52_Time-Span ;
	rdfs:label "1980" ;
	crm:P82a_begin_of_the_beginning "1980-01-01"^^xsd:date;
 	crm:P82b_end_of_the_end  "1980-12-31"^^xsd:date .
```



[^1]: Not recognised officialy by W3C, Geo becomes a *standard de facto* for geographical data. However it is not as complex or precise as GEOSparql or CRMGeo

[^2]: See the case of historical place vs actual places.

[^3]: We occasionally prefer to note the inverse CIDOC properties, denoted by _i_, when there is an identifiable 'central' entity.
