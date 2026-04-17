# 📺 K DRAMA REVIEWS USING R PROGRAMMING
(main objective: learning text functions in R)

## 📌 Table of Contents
- [K-Drama Dataset & Text Analysis](#k-drama-dataset--text-analysis-in-r)
- [Dataset Creation](#k-drama-dataset-creation)
- [Generating Comments](#generating-random-good--bad-comments)
- [Adding Gender Column](#adding-gender-column)
- [Adding Location Column](#adding-location-column)
- [Adding Streaming Platform](#adding-streaming-platform)
- [Adding Review Headlines](#adding-review-headlines)
- [Adding Genre Column](#adding-genre-column)
- [Text Manipulation](#text-manipulation-operations)
- [Project Outcome](#project-outcome)

---

## 📌 K-Drama Dataset Generation & Text Analysis in R

This repository presents an R programming project that builds a synthetic dataset of popular K-Dramas and demonstrates a wide range of text manipulation and data wrangling techniques. The project generates a dataframe containing drama titles along with randomly created positive and negative user reviews, and enriches it with additional attributes such as reviewer gender, location, streaming platform, genre, and review headlines. It also applies multiple string-processing operations using base R and the stringr package, including case conversion, text combination, splitting, replacement, pattern detection, substring extraction, position detection, and formatting. The dataset is further enhanced with derived features like combined headlines, tokenized words, keyword counts, and masked text, making it a practical resource for learning data cleaning, feature engineering, and text analytics in R.

---

## 📂 K-Drama Dataset Creation
- [Dataset Creation](#dataset-creation)
```r
kdrama_names <- data.frame(
  Drama = c(
    "Crash Landing on You", "Itaewon Class", "Vincenzo",
    "Descendants of the Sun", "Goblin", "Reply 1988",
    "Hospital Playlist", "Signal", "Kingdom"
    # (truncated for readability)
  )
)

View(kdrama_names)
```

using the kdrama_names dataframe generating random comments one badand one good comment for each kdrama entered above

## 💬 Generating Random Good & Bad Comments
```r
set.seed(123)

good_templates <- c("A beautifully written drama with strong performances and emotional depth.",
  "I absolutely loved the storytelling and character development.",
  "An engaging and memorable series that kept me hooked.",
  "Excellent direction and outstanding acting throughout.",
  "A heartwarming drama that exceeded my expectations."
)

bad_templates <- c( "The plot felt weak and somewhat predictable.",
  "I struggled to connect with the characters.",
  "The pacing was slow and occasionally boring.",
  "It had potential but failed to deliver properly.",
  "Not as impressive as I expected."
)

# generating 2 columns
kdrama_df <- data.frame(
  Drama = kdrama_names$Drama,
  Good_Comment = sapply(kdrama_names$Drama, function(x)
    paste(x, "-", sample(good_templates, 1))),
  Bad_Comment = sapply(kdrama_names$Drama, function(x)
    paste(x, "-", sample(bad_templates, 1))),
  stringsAsFactors = FALSE
)

View(kdrama_df)
fix(kdrama_df)   #if any quick changes are needed
```

## 👤 Adding Gender Column
```r
# adding gender of person who commented define the gender categories
genders <- c("male", "female", "non-binary")

# add the new column to kdrama_df
kdrama_df$gender <- sample(genders, size = nrow(kdrama_df), replace = TRUE)
View(kdrama_df)
```

## 🌍 Adding Location Column
```r
# user location define location
location <- c("South Korea", "Indonesia", "Philippines", "Thailand", "India", "USA", "Brazil", "Mexico")

kdrama_df$location <- sample(location, size = nrow(kdrama_df), replace = TRUE)
```

## 🎬 Adding Streaming Platform
```r
streaming_platform <- c("Netflix", "Netflix / Viki", "Netflix / Prime Video", "Viki")
kdrama_df$Streaming_Platform <- sample(streaming_platform, nrow(kdrama_df), replace = TRUE)
```

## 📝 Adding Review Headlines
```r
review_headline <- c(
  "A Heart-felt Cross-Border Romance Drama",
  "A Gritty Tale of Ambition and Revenge",
  "A Stylish Mafia-Infused Courtroom Drama"
)

kdrama_df$Review_Headline <- sample(review_headline, nrow(kdrama_df), replace = TRUE)
```

## 🎭 Adding Genre Column
```r
genre <- c("romance","drama","crime","fantasy","thriller","historical")
kdrama_df$Genre <- sample(genre, nrow(kdrama_df), replace = TRUE)
```

## 🧠 Text Manipulation Operations
### 🔤 Case Conversion
```r
tolower(kdrama_df$Good_Comment)
toupper(kdrama_df$Good_Comment)
```

## 🔗 Combine Columns
```r
kdrama_df$Drama_Headline <- paste(
  kdrama_df$Drama,
  kdrama_df$Review_Headline,
  sep = " - "
)
kdrama_df$Drama_Headline
```

## ✂️ Split Text into Words
```r
kdrama_df$Good_Words <- strsplit(kdrama_df$Good_Comment, " ")
kdrama_df$Good_Words
```

## 🔁 Replace Words
```r
kdrama_df$Bad_Comment_Modified <- gsub(
  "slow",
  "dragging",
  kdrama_df$Bad_Comment,
  ignore.case = TRUE
)
```

## 🔍 Pattern Matching
```r
romance_rows <- grep("Romance", kdrama_df$review_headline, ignore.case = TRUE)
kdrama_df[romance_rows, ]
```

## 🔢 Count Occurrences
```r
kdrama_df$Romance_Count <- str_count(kdrama_df$review_headline, "Romance")
View(kdrama_df)

# Count Number of Characters (Including Spaces)
kdrama_df$Headline_Char_Count <- nchar(kdrama_df$review_headline)
kdrama_df$Headline_Char_Count
```

## 📍 Find Position
```r
# Find rows where "Romance" appears in review_headline:
romance_rows <- grep("Romance",kdrama_df$review_headline,ignore.case = TRUE)
romance_rows kdrama_df[romance_rows, ]
length(romance_rows)

kdrama_df$Romance_Position <- str_locate(
  kdrama_df$review_headline,
  "Romance"
)
```

## ✂️ Extract Words
```r
# Extract first 10 characters from Drama name:
kdrama_df$Drama_Substr <- substr(kdrama_df$Drama, start = 1, stop = 10)
kdrama_df$Drama_Substr

kdrama_df$Second_Word <- word(kdrama_df$review_headline, 2)
kdrama_df$Second_Word

#Extract First Word from Good Comment
kdrama_df$First_Word_Good <- word(kdrama_df$Good Comment, 1)

#Extract Word "Romance" If Present
kdrama_df$Extract_Romance <- str_extract(kdrama_df$review_headline, "Romance")
kdrama_df$Extract_Romance
```

## 📏 Character Count
```r
#Count how many times "Romance" appears in review_headline:
kdrama_df$Romance_Count <- str_count(kdrama_df$review_headline,"Romance")
kdrama_df$Romance_Count

kdrama_df$Headline_Char_Count <- nchar(kdrama_df$review_headline)
```

## 🎨 Replace Characters
```r
# Replace first occurrence of "Romance":
kdrama_df$Headline_Replace_First <- str_replace( kdrama_df$review_headline, "Romance", "Love Story" )
kdrama_df$Headline_Replace_First

kdrama_df$Drama_Vowel_Masked <- chartr(
  "aeiouAEIOU",
  "**********",
  kdrama_df$Drama
)
kdrama_df$Drama_Vowel_Masked
```
## 📌 String Functions (stringr)
```r
#Detect if "Romance" Exists in Headline
str_detect(kdrama_df$review_headline,regex("Romance", ignore_case = TRUE))

#convert Genre to Title Case
str_to_title(kdrama_df$genre)

#Count Length Using str_length()
str_length(kdrama_df$Drama)

str_replace(kdrama_df$review_headline, "Romance", "Love Story")

#Remove Word "Romance" from Headline
str_remove(kdrama_df$review_headline,"Romance")

str_extract(kdrama_df$review_headline, "Romance")

#Pad Drama Name to Fixed Width
str_pad(kdrama_df$Drama, width = 35,side = "right",pad = ".")

#Combine Location + Streaming Platform
str_c(kdrama_df$location,kdrama_df$streaming_platform,sep = " - ")
```

## 🔗 Combine Strings
```r
str_c(kdrama_df$location, kdrama_df$streaming_platform, sep = " - ")
```

## 🚀 Project Outcome
This project helps understand:

- Data frame creation in R  
- Random data generation  
- Text mining & preprocessing  
- String manipulation using base R + stringr  
- Feature engineering techniques
