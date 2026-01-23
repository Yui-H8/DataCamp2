# Natural Language Processing (NLP) in Python
---
**Description**

Unlock the power of Natural Language Processing (NLP) and take your text analysis skills to the next level! This course equips you with essential tools to process, analyze, and extract insights from text data. Start with the fundamentals of text processing, such as tokenization, lemmatization, and stemming, to extracting meaningful numerical features from text using bag-of-words (BoW), TF-IDF, and embeddings. Finally, harness the power of state-of-the-art transformer models using Hugging Face. Perform sentiment analysis, classify content, analyze question-answer relationships, assess grammatical acceptability, and generate text using the latest models.

---
### Sentence and word tokenization
Tokenization is an important first step in NLP. It involves breaking text into smaller units called tokens, which is key to working with language data. Your task is to tokenize a snippet of a news article into both sentences and words.
* Import the nltk library.
* Download the punkt_tab package.
* Tokenize the text into sentences.
```
Hint
The punkt_tab package can be downloaded using the nltk.download() function.
Sentence tokenization can be performed through the sent_tokenize() function from nltk.
```
```python
# Import nltk
import nltk
# Download the punkt_tab package 
nltk.download('punkt_tab')

text = """
The stock market saw a significant dip today. Experts believe the downturn may continue.
However, many investors are optimistic about future growth.
"""

# Tokenize the text into sentences
sentences = nltk.sent_tokenize(text)
print(sentences)
```
```
['\nThe stock market saw a significant dip today.', 'Experts believe the downturn may continue.', 'However, many investors are optimistic about future growth.']
```
* Tokenize the first sentence you obtained into words.
```
Hint
The first element of a list s can be accessed by s[0].
Word tokenization can be performed through the word_tokenize() function from nltk.
```
```python
import nltk
nltk.download('punkt_tab')

text = """
The stock market saw a significant dip today. Experts believe the downturn may continue.
However, many investors are optimistic about future growth.
"""

sentences = nltk.sent_tokenize(text)

# Tokenize the first sentence into words
words = nltk.word_tokenize(sentences[0])
print(words)
```
```
['The', 'stock', 'market', 'saw', 'a', 'significant', 'dip', 'today', '.']
```
*Bravo! You've successfully tokenized the text into sentences and words. Now you're ready to continue your preprocessing journey!*

### NLP workflow
NLP enables computers to understand, interpret, and generate human language. This process involves several key steps that transform raw text into meaningful insights.

Arrange the steps of the NLP workflow in the correct order to understand how machines process language.
* Arrange the components of the NLP workflow in the correct order.

*Great job! You've successfully arranged the steps of the NLP workflow. Keep up the good work!*

---
### Removing stop words
You're working on a project where the goal is to classify feedback from users into different categories like "product issues", "service issues", and "suggestions". Often, stop words don't carry much meaning in distinguishing between categories. Your task is to remove these stop words to focus on the important words that will help a machine later on categorize the feedback into the correct topics.

The functions word_tokenize from nltk.tokenize and stopwords.words from nltk.corpus have been imported for you. Additionally, the NLTK resources punkt_tab and stopwords have already been downloaded.
* Tokenize the provided feedback into words.
* Get the list of English stopwords.
* Remove English stop words and save the result in filtered_tokens.
```
Hint
Theword_tokenize() function can be used to perform word tokenization.
Use stopwords.words() to get the stop words list, and don't forget to pass the language name.
Use a list comprehension to loop through tokens, convert each of them to lowercase with .lower(), and include it in filtered_tokens only if it's not in stop_words.
```
```python
feedback = "I reached out to support and got a helpful response within minutes!!! Very #impressed"

# Tokenize the text
tokens = word_tokenize(feedback)

# Get the list of English stop words
stop_words = stopwords.words('english')

# Remove stop words 
filtered_tokens = [word for word in tokens if word.lower() not in stop_words]

print(filtered_tokens)
```
```
['reached', 'support', 'got', 'helpful', 'response', 'within', 'minutes', '!', '!', '!', '#', 'impressed']
```
*Excellent! You've successfully removed stop words from the feedback. Now you have a simplified list of tokens to give for a machine! But before doing that, let's go a step further and deal with punctuation.*

### Removing punctuation
Now that you've removed stop words from the feedback text, it's time to handle punctuation. The tokens you obtained in the previous exercise still contain punctuation marks, which are often unnecessary when categorizing feedback.

Your task is to remove punctuation from the list of tokens provided, helping to clean up the data even further.
* Clean the filtered_tokens list by removing all punctuation.
```
Hint
You can filter out punctuation by checking if each token is not in string.punctuation.
```
```python
import string

filtered_tokens = ['reached', 'support', 'got', 'helpful', 'response', 'within', 'minutes', '!', '!', '!', '#', 'impressed']

# Remove punctuation
clean_tokens = [word for word in filtered_tokens if word.lower() not in string.punctuation]

print(clean_tokens)
```
```
['reached', 'support', 'got', 'helpful', 'response', 'within', 'minutes', 'impressed']
```
*Great job! You've successfully removed the punctuation from the list of tokens, leaving the most meaningful words that a machine can use to categorize the content.*

---
### Lowercasing
You're analyzing user reviews for a travel website. These reviews often include inconsistent capitalization like "TRAVEL" and "travel". To prepare the text for sentiment analysis and topic extraction, you'll first convert all words to lowercase, then tokenize them and clean them from stop words and punctuation.

The word_tokenize() function, a stop_words list have been provided. NLTK resources are already downloaded.
* Convert the provided review into lowercase.
* Tokenize the lower_text into words.
* Use list comprehension to remove stop words and punctuation using the lists of stop_words and string.punctuation.
```
Hint
Using .lower() is the right way to transform a text into lowercase.
In a list comprehension, you can add multiple conditions as follows [item for item in collection if condition1 and condition2].
Use not in to check if a word is not in the stop_words list.
```
```python
review = "I have been FLYING a lot lately and the Flights just keep getting DELAYED. Honestly, traveling for WORK gets exhausting with endless delays, but every trip teaches you something new!"

# Lowercase the review
lower_text = review.lower()

# Tokenize the lower_text into words
tokens = word_tokenize(lower_text)

# Remove stop words and punctuation
clean_tokens = [word for word in tokens if word not in stop_words and word not in string.punctuation]

print(clean_tokens)
```
```
['flying', 'lot', 'lately', 'flights', 'keep', 'getting', 'delayed', 'honestly', 'traveling', 'work', 'gets', 'exhausting', 'endless', 'delays', 'every', 'trip', 'teaches', 'something', 'new']
```
*Great start! You've successfully converted the review to lowercase and removed stop words and punctuation. Next, you'll stem the cleaned tokens to make the analysis more efficient.*

### Stemming
Now that you've cleaned the review text and removed stop words and punctuation, you're ready to normalize the remaining words using stemming to reduce words to their root form. This helps group similar words together, making your analysis more consistent and efficient.

The PorterStemmer class has been provided, along with a list of clean_tokens.
* Initialize the PorterStemmer().
* Use a list comprehension to stem each token from the clean_tokens list.
```
To apply stemming, use stemmer.stem() and pass the token you want to stem.
```
```python
clean_tokens = ['flying', 'lot', 'lately', 'flights', 'keep', 'getting', 'delayed', 'honestly', 'traveling', 'work', 'gets', 'exhausting', 'endless', 'delays', 'every', 'travel', 'teaches', 'something', 'new']

# Create stemmer
stemmer = PorterStemmer()

# Stem each token
stemmed_tokens = [stemmer.stem(word) for word in clean_tokens]

print(stemmed_tokens)
```
```
['fli', 'lot', 'late', 'flight', 'keep', 'get', 'delay', 'honestli', 'travel', 'work', 'get', 'exhaust', 'endless', 'delay', 'everi', 'travel', 'teach', 'someth', 'new']
```
*Well done! Notice how words like 'traveling', 'travel', 'delays', and 'delayed' were reduced to a common root form. While some results may not be complete English words, this simplification helps group similar terms together, making it easier to extract insights from the reviews.*

### Lemmatization
While continuing your analysis of user reviews, you noticed that stemming sometimes produces non-standard words like "fli" from "flying", which can reduce interpretability. To address this, you'll now use lemmatization, which returns actual words and helps improve the clarity and accuracy of your analysis.

WordNetLemmatizer has been imported, stop_words has been defined, and the necessary NLTK resources have been downloaded.
* Create an instance lemmatizer of the WordNetLemmatizer() class.
* Use the lemmatizer to lemmatize the lower_tokens.
