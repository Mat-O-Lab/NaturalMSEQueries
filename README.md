# NaturalMSEQueries - A natural way to query Material Sciences Engineering data experiments

**Main Research Question**: How to query semantic Material Science Engineering(MSE) data easier than using SPARQL queries? 

**Methodology**: Applying the State-of-Art(SOA) of Visual SPARQL queries aka SPARNATURAL to facilitate querying 
semantic MSE data instead of SPARQL queries.

**Evaluation**: Include comparisons with SOA, and use the [SUS questionaries](http://www.measuringu.com/sus.php) 
to assess the usability level. 

Suggestions are always welcome!

# The approach 
The inspiration for this work comes from [Sparnatural](https://github.com/sparna-git/Sparnatural).

It supports the creation of basic graph patterns with the selection of values with autocomplete search or dropdown lists. It can be configured through a JSON-LD or OWL configuration file (that can be edited in Protégé) that defines the classes and properties to be presented in the component.

![](documentation/screencast-sparnatural-dbpedia-v3-en.gif)

# Getting Started

## How to run
- clone the repository if you have not done it already.
### (Developers) On the repository directory, execute the following terminal commands:
- ``npm i`` to install dependencies
- ``npm run start`` to launch the development server with auto-refresh
- ``npm run build`` to compile the module using webpack

### With Docker
1. Run ``docker-compose build``
2. Run ``docker-compose up``
3. Open your browser: http://127.0.0.1:8080

## Tools and IDEs used
- Visual Studio Code

## Installing your own triple store (Apache Fuseki) and adapt `index.html` to your SPARQL endpoint URL
- Download the [apache Fuseki](https://dlcdn.apache.org/jena/binaries/apache-jena-fuseki-4.4.0.zip)
- Extract the content and execute the following command in a terminal ``./fuseki-server``
- The server should be running on ``http://localhost:3030/`` now you need to add your data
- Go to the index.html and replace the information about the endpoint. Example ``<a id="endpoint" href="http://localhost:3030/testmechanics/sparql">http://localhost:3030/testmechanics/sparql</a>``
- (**TODO**) One of the main first steps is to use our own data, e.g. [TeamMechanics data](https://raw.githubusercontent.com/Mat-O-Lab/RDFConverter/development/resources/franhoferTeamMechanik/data.ttl) - Mainly configuring the Left side of the screen to use our own ontology. Also present on the issue #[1199200364](https://github.com/Mat-O-Lab/NaturalMSEQueries/issues/2#issue-1199200364)

## Read the documentation
1. Read [the Sparnatural documentation](https://docs.sparnatural.eu)
2. Look at how things work in file `index.html`; 

# Features

## Query Structure

### Basic query pattern

Select the type of entity to search...

![](documentation/1-screenshot-class-selection.png)

... then select the type of the related entity.

![](documentation/2-screenshot-object-type-selection.png)

In this case there is only one possible type of relation that can connect the 2 entities, so it gets selected automatically. Then select a value for the related entity, in this case in a dropdown list :

![](documentation/3-screenshot-value-selection.png)

Congratulations, your first SPARQL query criteria is complete !

![](documentation/4-screenshot-criteria.png)

Now you can fetch the generated SPARQL query :

![](documentation/5-screenshot-sparql.png)

### "WHERE"

This enables to navigate the graph :

![](documentation/6-where.png)

### "AND"

Combine criterias :

![](documentation/7-and.png)

### "OR"

Select multiple values for a criteria :

![](documentation/8-or.png)

## Values selection

Sparnatural offers currently 6 ways of selecting a value for a criteria : autocomplete field, dropdown list, simple string value, date range (year or date precision), date range with a search in a period name (e.g. "bronze age"), or no selection at all.

### Autocomplete field

![](documentation/9-autocomplete.png)

### Dropdown list

![](documentation/10-list.png)

### Tree selector

![](documentation/17-tree.png)

### String value (text search)

![](documentation/11-search.png)

### Date range (year or date precision)

![](documentation/12-time-date.png)

### Date range with search in period name (chronocultural periods)

![](documentation/14-chronocultural-period.png)

(this requires data from [Perio.do](https://perio.do), a gazeeter of periods for linking and visualizing data)

### Boolean selection

![](documentation/15-boolean.png)

### No value selection

This is useful when a type a of entity is used only to navigate the graph, but without the ability to select an instance of these entities.

![](documentation/13-no-value.png)


## Multilingual

Sparnatural is multilingual and supports displaying labels of classes and properties in multiple languages.

## Support for OPTIONAL and FILTER NOT EXISTS

Sparnatural supports the `OPTIONAL` and `FILTER NOT EXISTS {}` keywords applied to a whole "branch" of the query.
See here how to search for French Museums and the name of Italian painters they display, _if any_ :

![](documentation/16-optional.gif)


## Limitations

### No UNION or BIND, etc.

Sparnatural does not support the creation of UNION, SERVICE, BIND, etc...

### SPARQL endpoint needs to be CORS-enabled

To send SPARQL queries to a service that is not hosted on the same domain name as the web page in which Sparnatural is included, the SPARQL endpoint needs to allow [Cross-Origin Resource Sharing (CORS)](https://enable-cors.org/). But we have SPARQL proxies for those who are not, don't worry ;-)

# Integration

## Specification of classes and properties

The component is configurable using a an [OWL configuration file](https://docs.sparnatural.eu/OWL-based-configuration) editable in Protégé.. Look at the specification files of [the demos](https://github.com/sparna-git/sparnatural.eu/tree/main/demos) to get an idea. 

Alternatively one can also use a [JSON(-LD) ontology file](https://docs.sparnatural.eu/JSON-based-configuration). A JSON(-LD) configuration file contains :

### Class definition

```json
    {
      "@id" : "http://dbpedia.org/ontology/Museum",
      "@type" : "Class",
      "label": [
        {"@value" : "Museum", "@language" : "en"},
        {"@value" : "Musée","@language" : "fr"}
      ],
      "faIcon":  "fas fa-university"
    },
```

### Property definitions with domains and ranges

```json
    {
      "@id" : "http://dbpedia.org/ontology/museum",
      "@type" : "ObjectProperty",
      "subPropertyOf" : "sparnatural:AutocompleteProperty",
      "label": [
        {"@value" : "displayed at","@language" : "en"},
        {"@value" : "exposée à","@language" : "fr"}
      ],
      "domain": "http://dbpedia.org/ontology/Artwork",
      "range": "http://dbpedia.org/ontology/Museum",
      "datasource" : "datasources:search_rdfslabel_bifcontains"
    },
```

### Using font-awesome icons

It is possible to directly use font-awesome icons in place of icons embedded in your application :

```json
"faIcon":  "fas fa-user",
```

## How to integrate Sparnatural in a webpage

Look at [this page in the documentation](https://docs.sparnatural.eu/Javascript-integration).


## Map the query structure to a different graph structure

Map classes or properties in the config to a corresponding SPARQL property path or a corresponding class URI, using the `sparqlString` JSON key, e.g. :

```
    {
      "@id" : "http://labs.sparna.fr/sparnatural-demo-dbpedia/onto#bornIn",
      "@type" : "ObjectProperty",
      ...
      "sparqlString": "<http://dbpedia.org/ontology/birthPlace>/<http://dbpedia.org/ontology/country>",
    },
```

Then call `expandSparql` on the `sparnatural` instance by passing the original SPARQL query, to replace all mentions of original classes and properties URI with the corresponding SPARQL string :

```
queryString = sparnatural.expandSparql(queryString);
```
