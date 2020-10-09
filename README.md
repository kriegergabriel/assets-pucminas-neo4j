# assets-pucminas-neo4j

Execute this code in your Cypher console Node4j
```cypher
LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/kriegergabriel/assets-pucminas-neo4j/main/products.csv" AS row
CREATE (n:Product)
SET n = row,
n.unitPrice = toFloat(row.unitPrice),
n.unitsInStock = toInteger(row.unitsInStock), n.unitsOnOrder =
toInteger(row.unitsOnOrder),
n.reorderLevel = toInteger(row.reorderLevel), n.discontinued =
(row.discontinued <> "0")

LOAD CSV WITH HEADERS FROM
"https://raw.githubusercontent.com/kriegergabriel/assets-pucminas-neo4j/main/categories.csv" AS row
CREATE (n:Category)
SET n = row

LOAD CSV WITH HEADERS FROM
"https://raw.githubusercontent.com/kriegergabriel/assets-pucminas-neo4j/main/suppliers.csv" AS row
CREATE (n:Supplier)
SET n = row

CREATE INDEX ON :Product(productID)
CREATE INDEX ON :Category(categoryID)
CREATE INDEX ON :Supplier(supplierID)

MATCH (p:Product), (c:Category) 
WHERE p.categoryID = c.categoryID 
CREATE (p)-[:PART_OF]->(c)

MATCH (p:Product),(s:Supplier)
WHERE p.supplierID = s.supplierID
CREATE (s)-[:SUPPLIES]->(p)
```
