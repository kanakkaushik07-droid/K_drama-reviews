# 📺 K DRAMA REVIEWS USING R PROGRAMMING
(main objective: learning text functions in R)
## 📌 K-Drama Dataset Generation & Text Analysis in R

This repository presents an R programming project that builds a synthetic dataset of popular K-Dramas and demonstrates a wide range of text manipulation and data wrangling techniques. The project generates a dataframe containing drama titles along with randomly created positive and negative user reviews, and enriches it with additional attributes such as reviewer gender, location, streaming platform, genre, and review headlines. It also applies multiple string-processing operations using base R and the stringr package, including case conversion, text combination, splitting, replacement, pattern detection, substring extraction, position detection, and formatting. The dataset is further enhanced with derived features like combined headlines, tokenized words, keyword counts, and masked text, making it a practical resource for learning data cleaning, feature engineering, and text analytics in R.

---

## 📂 K-Drama Dataset Creation

```r
kdrama_names <- data.frame(
  Drama = c(
    "Crash Landing on You",
    "Itaewon Class",
    "Vincenzo",
    "Descendants of the Sun",
    "Goblin",
    "Reply 1988"
    # (list continues...)
  )
)

kdrama_names
View(kdrama_names)
```
using the kdrama_names dataframe generating random comments one badand one good comment for each kdrama entered above

## 💬 Generating Random Good & Bad Comments
```r
set.seed(123)
length(kdrama_names)
class(kdrama_names)

good_templates <- c(
  "A beautifully written drama with strong performances and emotional depth.",
  "I absolutely loved the storytelling and character development.",
  "An engaging and memorable series that kept me hooked.",
  "Excellent direction and outstanding acting throughout.",
  "A heartwarming drama that exceeded my expectations."
)

bad_templates <- c(
  "The plot felt weak and somewhat predictable.",
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
streaming_platform <- c(
  "Netflix",       # Crash Landing on You
  "Netflix / Viki",   #Itaewon Class
  "Netflix / Prime Video", # Vincenzo (widely available on Netflix)
  "Netflix / Prime Video", # Descendants of the Sun
  "Netflix / Viki", # Goblin
  "Netflix / Viki", # Reply 1988
  "Netflix", # Hospital Playlist
  "Netflix", # Signal (widely listed on Netflix guides)
  "Netflix", # Kingdom (available on Netflix)
  "Viki", # Moon Lovers: Scarlet Heart Ryeo (commonly on Viki)
  "Netflix", # Sky Castle (commonly on Netflix listings)
  "Netflix / Viki", # The World of the Married (commonly Netflix, Viki) "Netflix", # Mr. Sunshine (listed on Netflix guides)  "Netflix / Viki", # My Love from the Star (classic available on Netflix/Viki) "Netflix", # Weightlifting Fairy Kim Bok Joo (Netflix)  "Netflix / Viki", # Strong Woman Do Bong Soon  "Netflix", # Extraordinary Attorney Woo (widely on Netflix)  "tvN / Netflix", # Twenty-Five Twenty-One (tvN original, also on Netflix)  "Netflix", # Business Proposal (Netflix in many regions)  "Netflix", # True Beauty (appears on Netflix in many regions)  "Netflix / iQiyi / Viki", # Flower of Evil (international on Netflix, iQiyi, Viki) "Netflix", # All of Us Are Dead (Netflix)  "Netflix" # Squid Game (Netflix))

kdrama_df$streaming_platform <- sample(streaming_platform, size = nrow(kdrama_df), replace = TRUE)
```

## 📝 Adding Review Headlines
```r
review_headline <- c(
  "A Heart-felt Cross-Border Romance Drama",
  "A Gritty Tale of Ambition and Revenge",
  "A Stylish Mafia-Infused Courtroom Drama",
  "A Classic Military Romance Adventure",
  "A Magical Romance with Mythic Depth","A Nostalgic Slice-of-Life Drama", "A Warm, Character-Driven Medical Ensemble", "A Mind-Bending Crime Thriller", "A Historical Zombie Thriller Masterpiece", "A Bittersweet Historical Romance", "A Sharp Satire on Elite Education", "A Scorching Tale of Betrayal and Romance", "An Epic Historical Romance", "An Interstellar Romance with Heart", "A Joyful Coming-of-Age Romance")

kdrama_df$review_headline <- sample(review_headline, size = nrow(kdrama_df), replace = TRUE)
```

## 🎭 Adding Genre Column
```r
genre <- c("romance", # Crash Landing on You "drama", # Itaewon Class "crime", # Vincenzo "romance", # Descendants of the Sun "fantasy", # Goblin "slice_of_life", # Reply 1988 "medical", # Hospital Playlist "thriller", # Signal "historical", # Kingdom "historical", # Moon Lovers: Scarlet Heart Ryeo "satire", # Sky Castle "melodrama", # The World of the Married "historical", # Mr. Sunshine "fantasy", # My Love from the Star "romance", # Weightlifting Fairy Kim Bok Joo "fantasy", # Strong Woman Do Bong Soon "legal", # Extraordinary Attorney Woo "romance", # Twenty-Five Twenty-One "romcom", # Business Proposal "romance", # True Beauty "thriller", # Flower of Evil "horror", # All of Us Are Dead "thriller", # Squid Game "horror", # Sweet Home "romance", # Hometown Cha-Cha-Cha "business", # Start-Up "romance", # Her Private Life "fantasy", # While You Were Sleeping "romcom", # What's Wrong with Secretary Kim "romance", # The Heirs "romance", # Boys Over Flowers "fantasy", # Secret Garden "slice_of_life", # My Mister "office", # Misaeng "fantasy", # Hotel Del Luna "fantasy", # The King: Eternal Monarch "fantasy", # Alchemy of Souls "superhero", # Moving "revenge", # The Glory "romance", # Our Beloved Summer "romcom", # Fight for My Way "romance", # Romance Is a Bonus Book "action", # Healer "fantasy", # W "thriller", # Pinocchio "action", # City Hunter "medical", # Doctor Stranger "romance", # Because This Is My First Life "historical", # Love in the Moonlight "historical", # Moon Embracing the Sun "historical", # The Red Sleeve "fantasy", # Extraordinary You "fantasy", # Tomorrow "action", # Taxi Driver "revenge", # Reborn Rich "sci_fi", # Memories of the Alhambra "fantasy", # The Uncanny Counter "romance", # Snowdrop "thriller", # Big Mouth "fantasy", # Arthdal Chronicles "comedy", # Chief Kim "black_comedy", # Prison Playbook "romance", # My ID is Gangnam Beauty "romance", # Nevertheless "fantasy", # Doom at Your Service "fantasy", # Angel's Last Mission: Love "medical", # Dr. Romantic "medical", # Dr. Romantic 2 "medical", # Dr. Romantic 3 "melodrama", # The Penthouse "melodrama", # The Penthouse 2 "melodrama", # The Penthouse 3 "action", # Vagabond "action", # The K2 "romcom", # Personal Taste "fantasy", # Legend of the Blue Sea "romance", # Love Alarm "crime", # My Name "slice_of_life", # Navillera "fantasy", # 18 Again "melodrama", # The Smile Has Left Your Eyes "romance", # Coffee Prince "melodrama", # Kill Me Heal Me "comedy", # The Producers "romance", # Sh**ting Stars "romance", # Run On "romcom", # Touch Your Heart "romance", # Encounter "romcom", # Suspicious Partner "thriller", # Beyond Evil "sci_fi", # Life on Mars "thriller", # Voice "thriller", # Tunnel "thriller", # Mouse "thriller", # Stranger "thriller", # Stranger 2 "dystopian", # The Devil Judge "legal", # Law School "medical", # Hospital Playlist 2 "slice_of_life", # Reply 1994 "slice_of_life", # Reply 1997 "fantasy", # The Sound of Magic "fantasy", # Bulgasal "historical", # Rookie Historian Goo Hae Ryung "historical", # Queen for Seven Days "historical", # Empress Ki "historical", # Mr. Queen "historical", # 100 Days My Prince "romcom", # Love to Hate You "romance", # Forecasting Love and Weather "slice_of_life", # My Liberation Notes "romance", # One Spring Night "romance", # Something in the Rain "romcom", # Mad for Each Other "slice_of_life", # Hello My Twenties "slice_of_life", # Hello My Twenties 2 "medical", # Good Doctor "family", # The Good Bad Mother "historical", # Under the Queen's Umbrella "fantasy", # Island "fantasy", # Island Part 2 "crime", # Money Heist Korea "romance", # A Time Called You "fantasy", # See You in My 19th Life "melodrama", # The Interest of Love "historical", # The Forbidden Marriage "fantasy", # Link: Eat Love Kill "romance", # Yumi's Cells "romance", # Yumi's Cells 2 "fantasy", # My Demon "fantasy", # Destined with You "romcom", # King the Land "thriller", # Celebrity "crime", # The Worst of Evil "medical", # Daily Dose of Sunshine "romance", # Castaway Diva "revenge", # Marry My Husband "romcom", # Doctor Slump "melodrama", # Queen of Tears "action", # A Shop for Killers "horror", # Gyeongseong Creature "horror", # Gyeongseong Creature 2 "fantasy", # The Midnight Studio "thriller", # Hierarchy "romance", # Lovely Runner "thriller", # The Frog "romcom", # No Gain No Love "fantasy" # Judge from Hell )  

kdrama_df$genre <- sample(genre, size = nrow(kdrama_df), replace = TRUE)
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
  kdrama_df$review_headline,
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

Data frame creation in R
Random data generation
Text mining & preprocessing
String manipulation using base R + stringr
Feature engineering techniques
