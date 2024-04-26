# SentimentAnalysisProject
Employee Sentiment Analysis 
Sentiment analysis (or opinion mining) is a natural language processing (NLP) technique used to determine whether data is positive, negative or neutral.

→ performed sentiment analysis on healthcare company’s employee feedback 
→ used R 

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



Employed the Syuzhet package to generate sentiment scores, revealing that 'Trust' constitutes 35% of words, while 'Disgust' is only 2%.
