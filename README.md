# Entity Linking and Normalization
Linking recipe ingredients to their USDA portions dataset counterparts using cosine similarity. Normalizing items' units to grams. Finding the main ingredients of recipes.

## 1. Methodology and Process
Prior to entity linking, the most accurate method of matching two strings were evaluated between 5 alternatives; namely cosine similarity, Levenshtein Distance, Jaccard Similarity, Euclidean Distance and Manhattan Distance. The methods' performances were evaluated via comparison with a preset ground truth. Cosine similarity outperformed the rest by scoring 17/20 where the next best alternative -Levenshtein Distance- scored 13/20. Thus, cosine similarity was chosen for linking operations. However, the performance of cosine similarity is is not guaranteed for different datasets.

The initial process requires two datasets. The first one is the USDA portions dataset, available online. Columns useful for this process were selected from the original dataset, the new dataset was renamed to "grams.csv". The second dataset is the parsed outputs from a NER model trained by "TASTESET.csv", available online. The NER model is able to read recipe datasets and parse recipes to their ingredients, ingredients' units and quantities. The parsed outputs are named "ingr.csv".

"grams.csv" and "ingr.csv" are inputted to "match_set.ipynb" where the output is "link.csv". "link.csv" includes every unique occurence of an ingredient, its best match to USDA dataset and the similarity score.

"link.csv" is inputted into "link_set.Rmd" where columns of "link.csv" are labeled. The new dataset is named "tlink.csv".

"grams.csv", "ingr.csv" and "tlink.csv" are inputted to "main_ingr.ipynb" where the output is "grammage.csv". "grammage.csv" is the same as "ingr.csv" except a new column is added to show the grammage of matched ingredients for each recipe.

## 2. data folder
There are five csv files in the data folder. grams.csv and ingr.csv are the initial input files for this process. Other three are output files.

### 2.1. ingr.csv
A NER model was trained using "TASTESET.csv". Then another dataset was inputted into the model to generate a parsed output. The columns represent title of the recipe, ingredients of the recipe, quantity of the ingredient and the unit of the quantity; respectively. There are incorrect categorizations albeit not frequent. Some of these incorrect categorizations are handled during the matching operation. Also, there are many special ingredient names that do not exist within USDA portions dataset. Total of 30926 rows and 4 columns are present.

### 2.2. grams.csv
Selected USDA portions dataset columns form this dataset. Columns are Food_Code (USDA code of the food item), Main_Food_Description (name of the food item), Subcode_Description (alternative name for the food item), WWEIA_Category_Description (type category of the food item), Portion_Description (unit), Portion_Weight_(g) (weight of the given unit in grams) in order. Total of 32615 rows and 6 columns are present.

### 2.3. link.csv and tlink.csv
"link.csv" includes unique occurrences of food items from "ingr.csv" dataset, their best matches with food items in "grams.csv", and those matches' similarity scores. "link.csv" is the output of "match_set.ipynb". "tlink.csv" is the same as "link.csv" except the columns are labeled. "tlink.csv" is the output of "link_set.Rmd". Total of 16586 rows and 3 columns are present.

### 2.4. grammage.csv
"grammage.csv" is the final output of the process. It includes the recipe information of "ingr.csv" plus the grammage information of each ingredient on and added column. Total of 30926 columns and 5 columns are present.

## 3. notebook folder
There are two Jupyter notebook files (.ipynb) in the notebook folder. Files include comments for detailed explanations of the code.

### 3.1. match_set.ipynb
This script finds the best match for each "ingr.csv" ingredient and "grams.csv" food item. The file takes approximately 15 minutes to run on Apple M1 Macbook with 8GB RAM. "link.csv" is the output of this script.

### 3.2. main_ingr.ipynb
This script handles inconsistencies with the matching process and normalizes units to grams for each recipe ingredient that is matched. The file takes approximately 22 minutes to run on Apple M1 Macbook with 8GB RAM. "grammage.csv" is the output of this script.

## 4. Rmd folder
There is one R Markdown (.Rmd) file in the notebook folder. The file does not include comments as its functionality is singular.

### 4.1. link_set.Rmd
This script adds labels to "link.csv" columns. "tlink.csv" is the output.

## 5. Results
Even though cosine similarity gave the best result when compared with the ground truth, it does not work adequately when special names for ingredients are presented. As the method is unable to match specific names with any food item in USDA portions dataset, they are defaulted to one unrelated result -in this case "lamb chop, ns as to cut, cooked, lean only eaten"-. This results in incorrect matching of possible main ingredients. This problem is handled via unit matching in "main_ingr.ipynb", however the data in "grammage.csv" suggests that given quantity in grams are not wrong but most ingredients are missing due to mismatch from cosine similarity or unit matching.

## 6. Further Improvements
The main problem is the initial matching in this process. Since the cosine similarity method just looks at how close two vectors are to each other, the context of these items being food items is missing. Thus, two suggestions for further research is given: Initial NER model should be trained with a larger array of recipes in order to avoid incorrect parsing; a new machine learning model (likely a neural network or an active learning model) should be defined in order to provide more accurate entity linking.
