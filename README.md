# SentimentAnalysisProject
Employee Sentiment Analysis 
Sentiment analysis (or opinion mining) is a natural language processing (NLP) technique used to determine whether data is positive, negative or neutral.

→ performed sentiment analysis on healthcare company’s employee feedback 
→ used R 
→ libraries used for it 
The following packages are used in the examples in this article:
tm for text mining operations like removing numbers, special characters, punctuations and stop words (Stop words in any language are the most commonly occurring words that have very little value for NLP and should be filtered out. Examples of stop words in English are “the”, “is”, “are”.)
snowballc for stemming, which is the process of reducing words to their base or root form. For example, a stemming algorithm would reduce the words “fishing”, “fished” and “fisher” to the stem “fish”.
wordcloud for generating the word cloud plot.
RColorBrewer for color palettes used in various plots
syuzhet for sentiment scores and emotion classification
ggplot2 for plotting graphs
Steps 
Read the data using readlines (The readLines function simply extracts the text from its input source and returns each line as a character string) 
The next step is to load that Vector as a Corpus. In R, a Corpus is a collection of text document(s) to apply text mining or NLP routines on.
Cleaning UP text data 
Cleaning the text data starts with making transformations like removing special characters from the text. This is done using the tm_map() function to replace special characters like /, @ and | with a space. The next step is to remove the unnecessary whitespace and convert the text to lower case.
#Replacing "/", "@" and "|" with space
toSpace <- content_transformer(function (x , pattern ) gsub(pattern, " ", x))
TextDoc <- tm_map(TextDoc, toSpace, "/")
# Remove numbers
TextDoc <- tm_map(TextDoc, removeNumbers)


Remove stop words (words that are unnecessary like and , ‘the’ etc.)
# Remove english common stopwords
TextDoc <- tm_map(TextDoc, removeWords, stopwords("english"))
Stemming using Snowballc package
Changing the words to their root or base form  (Optional step )
# Text stemming - which reduces words to their root form
TextDoc <- tm_map(TextDoc, stemDocument)




Building term document matrix
After cleaning the text data, the next step is to count the occurrence of each word, to identify popular or trending topics. 

Using the function TermDocumentMatrix() from the text mining package, you can build a Document Matrix – a table containing the frequency of words.


Plotted the above table using a bar chart to see the frequency of most used words
Generate the word cloud
#generate word cloud
set.seed(1234)
wordcloud(words = dtm_d$word, freq = dtm_d$freq, min.freq = 5,
          max.words=100, random.order=FALSE, rot.per=0.40,
          colors=brewer.pal(8, "Dark2"))
min.freq – words whose frequency is at or above this threshold value is plotted (in this case, I have set it to 5)
max.words – the maximum number of words to display on the plot (in the code above, I have set it 100)


Getting the sentiment scores using Syzuhet package
This example uses the Syuzhet package for generating sentiment scores, which has four sentiment dictionaries and offers a method for accessing the sentiment extraction tool
4 different methods are 
bing – binary scale with -1 indicating negative and +1 indicating positive sentiment
afinn – integer scale ranging from -5 to +5
Syzuhet- decimal ranging from -1 to 1
nrc


# regular sentiment score using get_sentiment() function and method of your choice
# please note that different methods may have different scales
syuzhet_vector <- get_sentiment(text, method="syuzhet")
# see the first row of the vector
head(syuzhet_vector)
# see summary statistics of the vector
summary(syuzhet_vector)

An inspection of the Syuzhet vector shows the first element has the value of 2.60. It means the sum of the sentiment scores of all meaningful words in the first response(line) in the text file, adds up to 2.60. 
The scale for sentiment scores using the syuzhet method is decimal and ranges from -1(indicating most negative) to +1(indicating most positive). 
Note that the summary statistics of the syuzhet vector show a median value of 1.6, which is above zero and can be interpreted as the overall average sentiment across all the responses is positive.
