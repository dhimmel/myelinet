# Unsupervised prediction of candidate compounds for remyelination

Multiple sclerosis destroys the myelin coating neurons leading to disability. Here, we aim to find small molecules compounds that will promote remyelination. We base our search on our drug repurposing hetnet -- a network with multiple node and relationship types -- which provides a comprehensive framework for data integration. The hetnet is stored in a graph database called neo4j, which we use to compute network features.

## Queries for visualization

These queries should be run in the neo4j browser. They were chosen to produce
illustrative and appropriately sized visualizations.

```cypher
// Find paths between Marimastat and myelination
MATCH paths =
  (s1:Compound)-[:BINDS_CbG]-(n1)-[:INTERACTS_GiG]-()
               -[:PARTICIPATES_GpBP]-(t1:BiologicalProcess)
WHERE s1.identifier = 'DB00786' // Marimastat
  AND t1.identifier = 'GO:0042552' // myelination
  // Expressed in the central nervous system
  AND exists((:Anatomy {identifier: 'UBERON:0001017'})-[:EXPRESSES_AeG]-(n1))
RETURN paths
UNION ALL
MATCH paths = (s2:Compound)-[:BINDS_CbG]-(n1)-[:INTERACTS_GiG]-()
             -[:REGULATES_BPrG]-(t2:BiologicalProcess)
WHERE s2.identifier = 'DB00786' // Marimastat
  AND t2.identifier = 'GO:0048709' // oligodendrocyte differentiation
RETURN paths
```

## Installation

This repository assumes a neo4j server containing the hetio-ind database is
running on port 7474.

## License

All original content in this repository is released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/ "Creative Commons · Public Domain Dedication").
