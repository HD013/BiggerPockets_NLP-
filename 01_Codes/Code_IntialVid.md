## This code will inialize and scape data from a single video and cover a few analyses

<br>

```
# Import libraries
import itertools 
from youtube_comment_downloader import * #you can find out more about this library here Tutorial: https://github.com/egbertbouman/youtube-comment-downloader
import re
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"

#NLP packages
from textblob import TextBlob
from nltk.corpus import stopwords
from wordcloud import WordCloud, STOPWORDS
import spacy
import nltk


```

<br> 

```
# Test download comments from a Youtube link
downloader = YoutubeCommentDownloader()
comments = downloader.get_comments_from_url('https://www.youtube.com/watch?v=IVK5vQg1UvY', sort_by=SORT_BY_POPULAR)
# This is a video from about a year old.

# empty data frame
comment_df = pd.DataFrame()

# show first 10 comments by getting the first 10th items from generator object
comments_top10 = itertools.islice(comments, 10)

# convert generator object to pd.dataframe
comments_top10df= pd.DataFrame(comments_top10)
```

<br>

```
# Func to count # of comments for this video 
def ilen(it):
    return len(list(it))

ilen(comments)
print (comments_top10df)

```

<br>


```
# The first 500 comments
comments = downloader.get_comments_from_url('https://www.youtube.com/watch?v=IVK5vQg1UvY', sort_by=SORT_BY_POPULAR)

comments_top500 = itertools.islice(comments, 500)
comments_top500df= pd.DataFrame(comments_top500)

comments_top500df.head()

```

<br>

```
# Save comments to CSV file 
comments_top500df.to_csv("02_Results/bp_IVK5vQg1UvY_ytb500.csv")

```

<br>

### CLEAN DATA ###

<br>

```
import warnings
warnings.filterwarnings('ignore')

# remove punctuation:
comments_top500df['text'] = comments_top500df['text'].str.replace('[^\w\s]','')
comments_top500df['text'].head()

```

<br>

```
# remove any emojis :(
# REFERENCE : https://gist.github.com/slowkow/7a7f61f495e3dbb7e3d767f97bd7304b

def remove_emoji(text):
    emoji_pattern = re.compile("["
                           u"\U0001F600-\U0001F64F"  # emoticons
                           u"\U0001F300-\U0001F5FF"  # symbols & pictographs
                           u"\U0001F680-\U0001F6FF"  # transport & map symbols
                           u"\U0001F1E0-\U0001F1FF"  # flags 
                           u"\U00002702-\U000027B0"
                           u"\U000024C2-\U0001F251"
                           "]+", flags=re.UNICODE)
    return emoji_pattern.sub(r'', text)

comments_top500df['text'] = comments_top500df['text'].apply(lambda x: remove_emoji(x))
comments_top500df['text'].head()

```

<br>

```
# Remove stopwords - commonly used words (i.e. “the”, “a”, “an”) that do not add meaning to a sentence 
nltk.download('stopwords')

stop = stopwords.words('english')
comments_top500df['text'] = comments_top500df['text'].apply(lambda x: " ".join(x for x in x.split() if x not in stop))
comments_top500df['text'].head()

```

<br>

```
# Lemmatization using Spacy so that we can count the appearance of each word.

#initialize Spacy ‘en’ model, keeping only the component need for lemmatization and creating an engine:
# need to download the model in cmd by: spacy download en_core_web_sm 

nlp = spacy.load("en_core_web_sm", disable=['parser', 'ner'])

def space(comment):
    doc = nlp(comment)
    return " ".join([token.lemma_ for token in doc])

comments_top500df['text'] = comments_top500df['text'].apply(space)
comments_top500df['text'].head()

```

<br>

```
# remove textless rows  
comments_top500df['text'].replace('', np.nan, inplace=True)
comments_top500df.dropna(subset=['text'], inplace=True)
comments_top500df.shape # 125 rows 

```

<br>

```
# Save updated comments to CSV file 
comments_top500df.to_csv("02_Results/bp_IVK5vQg1UvY_ytb500.csv")

```

<br>

### WORDCLOUD VIZ ###
#### See: https://www.geeksforgeeks.org/generating-word-cloud-python/

<br>

```
comment_words = ''
stopwords = set(STOPWORDS)

for val in comments_top500df['text']:
     
    # typecaste each val to string
    val = str(val)
 
    # split the value
    tokens = val.split()
     
    # Converts each token into lowercase
    for i in range(len(tokens)):
        tokens[i] = tokens[i].lower()
     
    comment_words += " ".join(tokens)+" "
 
wordcloud = WordCloud(width = 800, height = 800,
                background_color ='white',
                stopwords = stopwords,
                min_font_size = 10).generate(comment_words)
 
# plot the WordCloud image   
beingsaved = plt.figure(figsize = (8, 8), facecolor = None)

plt.imshow(wordcloud)
plt.axis("off")
plt.tight_layout(pad = 0)
 
plt.show()

```

<br>

### Sentiment analysis $$$
#### https://www.kaggle.com/code/adepvenugopal/sentiment-analysis-of-youtube-comments

<br>

```
#Testing NLP - Sentiment Analysis using TextBlob
testsent= TextBlob("The movie is good").sentiment

testsent
# To interpret the result:
# TextBlob’s output for a polarity task is a float within the range [-1.0, 1.0] where -1.0 is a negative polarity and 1.0 is positive. This score can also be equal to 0, which stands for a neutral evaluation of a statement as it doesn’t contain any words from the training set.
# Whereas, a subjectivity/objectivity identification task reports a float within the range [0.0, 1.0]

```

<br>

```
#Test NLP - Sentiment Analysis using one of the video comments
comments_top500df['text'][1]

TextBlob(comments_top500df['text'][1]).sentiment

```

<br>

```
# Calculate Sentiment polarity for each comment

pol=[] # list which will contain the polarity of the comments
for i in comments_top500df['text']:
    try:
        analysis =TextBlob(i)
        pol.append(analysis.sentiment.polarity)
        
    except:
        pol.append(0)
        
# adding polarity to df
comments_top500df['polarity']=pol
comments_top500df.head()

```

<br>

```
comments_top500df.tail()
# just check the tail 

```

<br>

```
# Make plot of polarity 
plt.hist(comments_top500df['polarity'], bins=100)
plt.gca().set(title='Comment Polarity Histogram', ylabel='Frequency')

```

<br>

```
# get a few summary statistics 
comments_top500df['polarity'].describe()

```

<br>


<br>


<br>

