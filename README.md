# Project 1: MI3 Group5
MI3 Data Analysis using Word Clouds for Project 1 Group 5 for DS 4002

Team Members:
David Bergman(dtb9de): Group Leader,
Sally Sydnor(srs8yy),
Minh Nguyen(hvn9qwn)

## Repository Contents

This repository contains 2 markdown files: README.md and LICENSE.md, as well as 3 folders: SRC, DATA, and Figures. The README.md file contains information about the contents of the repo as well as explanations for the src, data, and figures folders. LICENSE.md contains an MIT license for our work. The SRC folder contains the main code file for our project. More information about how the code works will be provided in the next section of this document. The data folder contains instructions on how to download the data file used for this project. A data dictionary is provided in the data section of this readme. The figures folder will contain all of the graphics generated from this project. A description of each figure is provided in the figures section of the readme. Lastly, all of our references will be displayed in the references section of this readme.

## SRC

### Installation/Building of Code

#### Implement the following steps to install and build the code:

1. Importing Packages
```{r}
#install.packages("wordcloud")
#install.packages("wordcloud2")
#install.packages("tm")
library(tm)
library(wordcloud)
library(RColorBrewer)
library(wordcloud2)
```

2. Importing Dataset
```{r}
library(readr)
JEOPARDY<- read_csv("/Users/davidbergman/Downloads/JEOPARDY_CSV.csv")
```

3. Data Cleaning
```{r}
JEOPARDY$`Air Date` <- substr(JEOPARDY$`Air Date`, 1, 4)
JEOPARDY$`Air Date` <- as.integer(JEOPARDY$`Air Date`)
#View(JEOPARDY)

text_data <- JEOPARDY$Question


corpus <- Corpus(VectorSource(text_data))

corpus <- tm_map(corpus, content_transformer(tolower))  # Convert to lowercase
corpus <- tm_map(corpus, removePunctuation)            # Remove punctuation
corpus <- tm_map(corpus, removeNumbers)                # Remove numbers
corpus <- tm_map(corpus, removeWords, stopwords("en")) # Remove English stopwords
corpus <- tm_map(corpus, stripWhitespace)              # Strip whitespace

```

#### Utilize tm and word cloud packages for generating word clouds

### Code Usage

Producing a word cloud:

1. Compile specific years data for analyzing 
```{r}
#1984-1991
years_1_data <- JEOPARDY[JEOPARDY$`Air Date` >= 1984 & JEOPARDY$`Air Date` <= 1990, ]
years_1_data_text <- years_1_data$Question
```

2. Subset data and compile into a corpus
```{r}
subset_corpus <- Corpus(VectorSource(years_1_data_text))
```

3. Convert textual data to lowercase, remove unnecessary letters or punction, remove common stopwords and whitespace
```{r}
subset_corpus <- tm_map(subset_corpus, content_transformer(tolower))  # Convert to lowercase
subset_corpus <- tm_map(subset_corpus, removePunctuation)            # Remove punctuation
subset_corpus <- tm_map(subset_corpus, removeNumbers)                # Remove numbers
subset_corpus <- tm_map(subset_corpus, removeWords, stopwords("en")) # Remove English stopwords
subset_corpus <- tm_map(subset_corpus, stripWhitespace)              # Strip whitespace
```

4. Remove words that are not significant or helpful in data analysis
```{r}
words_to_remove <- c("one","first","used","also","like","can","dont","name","named","called","new","said","may","known","became","made","clue","man", "said","type","part","last","seen", "whose","now","people","crew")
subset_corpus <- tm_map(subset_corpus, removeWords, words_to_remove)
```
5. Produce the wordcloud
```{r}
wordcloud(words = subset_corpus, max.words =20, random.order = FALSE, colors = brewer.pal(8,"Dark2"))
```

## Data

| Variable    | Variable Type | Description                               |
| ----------- | ------------- | ------------------------------------------|
| Show Number | int           | Episode number that question was asked    |
| Air Date    | int           | Year that question was asked              |  
| Round       | string        | Round of Jeopardy that question was asked |                      
| Category    | string        | Category of question                      |
| Value       | string        | Dollar value of question                  |
| Question    | string        | Question asked on Jeopardy                |
| Answer      | string        | Answer to the question                    |

Data file can be downloaded through this google drive link:
https://drive.google.com/file/d/0BwT5wj_P7BKXUl9tOUJWYzVvUjA/view?resourcekey=0-uFrn8bQkUfSCvJlmtKGCdQ


## Figures

### Project 1 Figures Table of Contents
| Figure Name      | Description |
| ----------- | ----------- |
| Allwords      | This wordcloud contains the 20 most common words in Jeopardy questions from 1984-2012, without any "useless" words removed (stopwords have already been removed)       |
| Allyears   | This wordcloud contains the 20 most common words in Jeopardy questions from 1984-2012 with words we deemed useless/informationless removed (all subsequent wordclouds also have these words removed)        |
| 1984-1991   | This wordcloud contains the 20 most common words in Jeopardy questions from 1984-1991      |
| 1992-1997   | This wordcloud contains the 20 most common words in Jeopardy questions from 1992-1997      |
| 1998-2004   | This wordcloud contains the 20 most common words in Jeopardy questions from 1998-2004      |
| 2005-2012   | This wordcloud contains the 20 most common words in Jeopardy questions from 2005-2012      |

View figures here: https://github.com/dtb9de/DS4002P1/tree/13b4944ca0e1a8570ac7ea63c1a0afa8d277f23e/Figures

## References

[1] D. Forward, “How to watch Alex Trebek’s first-ever episode of ‘jeopardy!,’” Parade,
https://parade.com/tv/alex-trebek-first-ever-jeopardy-episode-premiere-march-30-nearly-
40-years-later (accessed Sep. 15, 2023).

[2] "Jeopardy (American television game show)." Britannica,
https://www.britannica.com/topic/Jeopardy-American-television-game-show. Accessed:
September 13, 2023.

### Previous Submissions
MI1: https://myuva-my.sharepoint.com/:w:/g/personal/srs8yy_virginia_edu/EUod7bAndmhPnW-_wb9_UuEBdv3Dl_-yY4szrFnZHyG8UA?e=skkBzt
MI2: https://myuva-my.sharepoint.com/:w:/g/personal/dtb9de_virginia_edu/EbOE_-26VtpGhSplty2E6joBd7crYj7pRcOFT0oxlzT4Bg?e=7aiQYW

