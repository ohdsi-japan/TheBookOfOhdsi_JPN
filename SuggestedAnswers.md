# Suggested Answers {#SuggestedAnswers}

This Appendix contains suggested answers for the exercises in the book.

## SQL and R

**Exercise \@ref(exr:exerciseCelecoxibUsers)**

To compute the number of people with at least one prescription of celecoxib, we need to query the DRUG_EXPOSURE table. To find all drugs containing the ingredient celecoxib, we join to the CONCEPT_ANCESTOR and CONCEPT tables:


```r
library(DatabaseConnector)
connection <- connect(connectionDetails)
sql <- "SELECT COUNT(DISTINCT(person_id)) AS person_count
FROM @cdm.drug_exposure
INNER JOIN @cdm.concept_ancestor
  ON drug_concept_id = descendant_concept_id
INNER JOIN @cdm.concept ingredient
  ON ancestor_concept_id = ingredient.concept_id
WHERE LOWER(ingredient.concept_name) = 'celecoxib'
  AND ingredient.concept_class_id = 'Ingredient'
  AND ingredient.standard_concept = 'S';"

renderTranslateQuerySql(connection, sql, cdm = "main")
```

```
##   PERSON_COUNT
## 1         1844
```

Note that we use `COUNT(DISTINCT(person_id))` to find the number of distinct persons, considering that a person might have more than one prescription. Also note that we use the `LOWER` function to make our search for "celecoxib" case-insensitive.

