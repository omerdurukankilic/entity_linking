# Entity Linking and Normalization
Linking recipe ingredients to their USDA portions dataset counterparts using cosine similarity. Normalizing items' units to grams. Finding the main ingredients of recipes.

## 1. Methodology and Process
Prior to entity linking, the most accurate method of matching two strings were evaluated between 5 alternatives; namely cosine similarity, Levenshtein Distance, Jaccard Similarity, Euclidean Distance and Manhattan Distance. The methods' performances were evaluated via comparison with a preset ground truth. Cosine similarity outperformed the rest by scoring 17/20 where the next best alternative -Levenshtein Distance- scored 13/20. Thus, cosine similarity was chosen for linking operations. However, the performance of cosine similarity is is not guaranteed for different datasets.

The initial process requires two datasets. The first one is the USDA portions dataset, available online. Columns useful for this process were selected from the original dataset, the new dataset was renamed to "grams.csv". The second dataset is the parsed outputs from a NER model trained by "TASTESET.csv", available online. The NER model is able to read recipe datasets and parse recipes to their ingredients, ingredients' units and quantities. The parsed outputs are named "ingr.csv".

"grams.csv" and "ingr.csv" are inputted to "match_set.py" where the output is "link.csv". "link.csv" includes every unique occurence of an ingredient, its best match to USDA dataset and the similarity score.

"link.csv" is inputted into "link_set.Rmd" where columns of "link.csv" are labeled. The new dataset is named "tlink.csv".

"grams.csv", "ingr.csv" and "tlink.csv" are inputted to "main_ingr.csv" where the output is "grammage.csv". "grammage.csv" is the same as "ingr.csv" except a new column is added to show the grammage of matched ingredients for each recipe.

## 2. data folder
There are 5 csv files in the data folder. grams.csv and ingr.csv are the initial input files for this process. Other three are output files.

### 2.1 ingr.csv
A NER model was trained using "TASTESET.csv". Then another dataset was inputted into the model to generate a parsed output. The columns represent title of the recipe, ingredients of the recipe, quantity of the ingredient and the unit of the quantity; respectively. There are incorrect categorizations albeit not frequent. Some of these incorrect categorizations are handled during the matching operation. Also, there are many special ingredient names that do not exist within USDA portions dataset.

### 2.2 grams.csv
Selected USDA portions dataset columns form this dataset. Columns are Food_Code (USDA code of the food item), Main_Food_Description (name of the food item), Subcode_Description (alternative name for the food item), WWEIA_Category_Description (type category of the food item), Portion_Description (unit), Portion_Weight_(g) (weight of the given unit in grams) in order.

### 2.3 link.csv and tlink.csv


