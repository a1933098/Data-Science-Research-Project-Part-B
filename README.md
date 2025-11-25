
## README FILE  
## Emotion and Place in *Around the World in 80 Days*
 
 **Natural Language Processing Research Project (MDS 7097B)**
 
 **Author:** Ann Mary Anish

 **Supervisor:** Ashley Grace Dennis-Henderson
 
 **University of Adelaide**


#
### **PROJECT OVERVIEW**

 This project uses Natural Language Processing (NLP) to analyse
 Jules Verne’s novel "*Around the World in 80 Days*".

 It answers two key research questions:
 
 **1. How does the emotional tone of the novel change across chapters?**
 
 **2. How do place names mentioned throughout the text align with Phileas Fogg’s journey?**

 To explore these questions, the project combines:
- Dictionary-based sentiment analysis (SenticNet)
- Named Entity Recognition (NER) using spaCy
- N-gram analysis (bi-grams and tri-grams)
- Visualisations including the emotional arc, event-annotated arc, top locations, and journey heatmap)

#
  **REPOSITORY STRUCTURE**
 
 | File Name                        | Description                          |
|----------------------------------|--------------------------------------|
| `Nlp_Project.Rmd`                | Main analysis file                   |
| `around_world_80_days.csv`       | Cleaned chapter text                 |
| `tokens_for_sentiment.csv`       | Tokenised & lemmatised word list     |
| `sentiment_lexicon_coverage.csv` | Lexicon coverage results             |
| `NER_Top_Locations.csv`          | Most frequently mentioned locations  |
| `NER_Locations_By_Chapter.csv`   | Location–chapter matrix (heatmap)    |
| `bigrams_clean.csv`              | Cleaned bi-grams                     |
| `trigrams_clean.csv`             | Cleaned tri-grams                    |
| `README.Rmd` / `README.md`       | Project documentation                |


#
### **GETTING STARTED**

 **1. DEPENDENCIES**
 
 To run the analysis you will need:
 - R (version 4.2 or later)
 - RStudio (recommended)
 - Required R packages, installed as shown below
 
**2. REQUIRED R PACKAGES**
 
Install pacman if not already installed
- install.packages("pacman")

 Load and install all required packages
-  pacman::p_load(tidyverse, textdata, tidytext, syuzhet, gutenbergr, textstem, stringr,  ggrepel, dplyr, ggplot2, lexicon, grid, zoo, spacyr)


**3. Initialising spaCy (R-Only)**
 
 In R:
 - library(spacyr)
 - spacy_initialize(model = "en_core_web_sm")

 **4. Download the novel**
  
  book <- gutenberg_download(103, mirror = "https://www.gutenberg.org")

### **HOW TO RUN THE ANALYSIS**

 1. Open Nlp_Project.Rmd in RStudio
 2. Ensure all packages & spaCy are installed
 3. Knit Nlp_Project.Rmd to HTML

 Knitting will:
 - Download the novel from Project Gutenberg (ID 103)
 - Clean and split the text into chapters
 - Tokenise, remove stopwords, and lemmatise words
 - Compute sentiment per chapter using SenticNet
 - Smooth the emotional arc
 - Extract and standardise locations using NER
 - Build graphs (emotional arc, events arc, locations, heatmap, n-grams)
 - Save multiple CSV files


### **METHODS SUMMARY**

 **1. TEXT PREPARATION** 

The novel undergoes several cleaning steps before analysis:

- **Download** the text from Project Gutenberg (ID 103)
- **Remove headers/footers** added by Gutenberg
- **Split into chapters** using chapter headings
- **Remove blank lines and extra chapter titles**
- **Combine lines** so each chapter becomes a single row
- **Convert to lowercase**
- **Remove punctuation and digits**
- **Tokenise** into individual words
- **Remove stopwords** (“the”, “and”, “was”)
- **Lemmatise** words (e.g., “running”, “ran” → “run”)

The cleaned chapters are saved to `around_world_80_days.csv`  
and the cleaned tokens are saved to `tokens_for_sentiment.csv`.


**2. SENTIMENT ANALYSIS**
 
 This section measures how the emotional tone of the novel changes across chapters.
 
**Lexicon Selection**
- Five sentiment dictionaries were tested: AFINN, Bing, NRC, SentiWordNet, and SenticNet.
- Coverage (the percentage of novel words appearing in each lexicon) was compared.
- SenticNet had the highest coverage (~72.57%), so it was used for the final analysis.

**Calculating Sentiment**
 
 For each chapter:
- Words were matched to SenticNet
- Each matched word contributed a sentiment value
- The chapter sentiment score = average of all matched word values
- A centred rolling mean was applied to create a smoother emotional pattern across the novel.
 
 **Visualisations**
 - Emotional Arc (SenticNet sentiment per chapter, smoothed with a rolling mean)
 - Emotional Arc with Key Events (major plot moments labelled A–H to show where emotion rises and falls)

**3. NAMED ENTITY RECOGNITION (NER)**
 
 This part of the project identifies all geographical references in the novel, such as   cities, countries, and regions.

 **What was done**
 - spaCy used through spacyr
 - Extract GPE (geo-political) and LOC (location) entities
 - Standardise old spellings (Bombay -> Mumbai)
 - Remove demonyms (Englishman, Frenchwoman, American)
 - Fix spacing/hyphens

 **Two key outputs**
 - NER_Top_Locations.csv : total mentions of each location
 - NER_Locations_By_Chapter.csv : how often each place appears per chapter

 **Visualisations**
 - Top 10 most frequent locations (bar plot)
 - Heatmap of mentions across chapters (ordered by travel route)

 **4. N-GRAM ANALYSIS** 
 
 This part of the project identifies the most common short word-patterns in the novel.
 N-grams help reveal repeated descriptions, actions, and story elements.

**What was done**
 - Created Bi-grams (2-word phrases) and Tri-grams (3-word phrases)
 - Remove stopword combinations
 - Counted the frequency of each n-gram
 - Saved results as:
    - bigrams_clean.csv
    - trigrams_clean.csv

 **Visualisations** 
- Top Bi-grams (most common 2-word phrases)
- Top Tri-grams (most common 3-word phrases)

**Highlights**
- Frequent character names (e.g., “phileas fogg”)
- Major locations (e.g., “hong kong”)
- Time-related expressions (e.g., “half past eleven”)
- Key stakes or plot elements (e.g., “twenty thousand pounds”)


### **REPRODUCIBILITY**

 - All data is included or downloaded automatically
 - All steps are contained in Nlp_Project.Rmd
 - CSV outputs included for transparency
 - Anyone can reproduce the analysis by knitting the file


### **WHY THIS PROJECT IS USEFUL**

 - Fully reproducible NLP project
 - Combines sentiment analysis, NER, and n-grams
 - Provides clear visualisations for emotional + geographical patterns
 - Easily adaptable to other books and texts



### **ACKNOWLEDGMENTS**

 I would like to thank:
 - Ashley Grace Dennis-Henderson for supervision and guidance
 - The University of Adelaide
 - Project Gutenberg for the text
 - Developers of tidyverse, tidytext, spacyr, spaCy, and other open-source tools

