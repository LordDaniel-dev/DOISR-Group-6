For the following command to evaluate the data:

pyshacl -s shapes.ttl correcto.jsonld -df json-ld

We recieve a positive evaluation result:

Validation Report
Conforms: True

For the incorrect data in incorrecto.jsonld in which we have that a directors last movie has been released before his first movie (and has no actor or actress credits) we get the following negative evaluation result:

pyshacl -s shapes.ttl incorrecto.jsonld -df json-ld
Validation Report
Conforms: False
Results (3):
Constraint Violation in LessThanConstraintComponent (http://www.w3.org/ns/shacl#LessThanConstraintComponent):
        Severity: sh:Violation
        Source Shape: [ sh:lessThan :hasLastMovieYear ; sh:path :hasFirstMovieYear ]
        Focus Node: ex:director1
        Value Node: Literal("2020", datatype=xsd:integer)
        Result Path: :hasFirstMovieYear
        Message: Value of ex:director1->:hasLastMovieYear <= Literal("2020", datatype=xsd:integer)
Constraint Violation in MinCountConstraintComponent (http://www.w3.org/ns/shacl#MinCountConstraintComponent):
        Severity: sh:Violation
        Source Shape: [ sh:datatype xsd:boolean ; sh:maxCount Literal("1", datatype=xsd:integer) ; sh:minCount Literal("1", datatype=xsd:integer) ; sh:path :hasActorCredits ]
        Focus Node: ex:director1
        Result Path: :hasActorCredits
        Message: Less than 1 values on ex:director1->:hasActorCredits
Constraint Violation in MinCountConstraintComponent (http://www.w3.org/ns/shacl#MinCountConstraintComponent):
        Severity: sh:Violation
        Source Shape: [ sh:datatype xsd:boolean ; sh:maxCount Literal("1", datatype=xsd:integer) ; sh:minCount Literal("1", datatype=xsd:integer) ; sh:path :hasActressCredits ]
        Focus Node: ex:director1
        Result Path: :hasActressCredits
        Message: Less than 1 values on ex:director1->:hasActressCredits

And when we make the necessary changes to the data file (giving boolean values to hasActorCredits and hasActressCredits and making hasLastMovieYear >= hasFirstMovieYear) we get:

pyshacl -s shapes.ttl incorrecto2.ttl
Validation Report
Conforms: True
