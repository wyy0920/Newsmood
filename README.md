
News Mood 


```python
#dependencies
import tweepy
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import json
import time
import csv
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
```


```python
#Import Twitter API Keys and setup API Authentication
from TwitterAPIkey import consumer_key,consumer_secret,access_token,access_token_secret
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())

```

News Moond Analysis 


```python
#target search terms
target_terms=("@BBCWorld","@CBSNews","@CNN","@FoxNews","@nytimes")

#Array to hold sentiment
sentiment_array=[]
#Variable for holding the oldest tweet
oldest_tweet=""
i=0
counter=1

    
```


```python
#loop through all target terms
for target in target_terms:
    #variables for holding sentiments
    compound_list=[]
    positive_list=[]
    negative_list=[]
    neutral_list=[]
    i+=1
    public_tweets=api.user_timeline(target,count=100)
#print(public_tweets)
#loop through all tweets
    for tweet in public_tweets:
        text=tweet["text"]
    #run vader on each tweet
        compound=analyzer.polarity_scores(text)["compound"]
        pos=analyzer.polarity_scores(text)["pos"]
        neu=analyzer.polarity_scores(text)["neu"]
        neg=analyzer.polarity_scores(text)["neg"]
    #add each value to the array
        sentiment_array.append({"Media":target,"Tweet text":text,"Date":tweet["created_at"],"Compound Score":compound,"Positive Score":pos,"Neutral Score":neu,"Negative Score":neg,"Tweet Ago":counter})

    counter+=1

```


```python
#convert sentiments_array to Daraframe
sentiments_pd=pd.DataFrame.from_dict(sentiment_array)
sentiments_pd
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Compound Score</th>
      <th>Date</th>
      <th>Media</th>
      <th>Negative Score</th>
      <th>Neutral Score</th>
      <th>Positive Score</th>
      <th>Tweet Ago</th>
      <th>Tweet text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.6908</td>
      <td>Thu Mar 22 22:44:19 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.487</td>
      <td>0.513</td>
      <td>0.000</td>
      <td>1</td>
      <td>Syria war: Eastern Ghouta rebels announce ceas...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0000</td>
      <td>Thu Mar 22 22:34:29 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Trump replaces McMaster with Bolton https://t....</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.6705</td>
      <td>Thu Mar 22 22:18:10 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.224</td>
      <td>0.776</td>
      <td>0.000</td>
      <td>1</td>
      <td>At least half a million children are being aff...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.2500</td>
      <td>Thu Mar 22 21:46:45 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.099</td>
      <td>0.755</td>
      <td>0.146</td>
      <td>1</td>
      <td>The Islamic State group destroyed their librar...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.1280</td>
      <td>Thu Mar 22 21:03:07 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>0.842</td>
      <td>0.158</td>
      <td>1</td>
      <td>Sex doll 'brothel': Xdolls escapes Paris counc...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.0000</td>
      <td>Thu Mar 22 19:15:59 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Kenya payout for mother made to deliver on hos...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.4973</td>
      <td>Thu Mar 22 19:15:59 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>0.624</td>
      <td>0.376</td>
      <td>1</td>
      <td>Dapchi kidnap: Government 'will not abandon' l...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.0000</td>
      <td>Thu Mar 22 18:20:44 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Meet SoFi - the soft robot fish developed by M...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.5106</td>
      <td>Thu Mar 22 18:17:09 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>0.602</td>
      <td>0.398</td>
      <td>1</td>
      <td>Viral teacher's inspirational chalkboard PC ht...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.0000</td>
      <td>Thu Mar 22 18:15:23 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>RT @bbclaurak: Tusk - EU moving trade debate t...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.0000</td>
      <td>Thu Mar 22 17:26:48 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Israeli man fined for urinating on memorial at...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.3400</td>
      <td>Thu Mar 22 17:20:30 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>0.893</td>
      <td>0.107</td>
      <td>1</td>
      <td>RT @BBCSport: Ireland have been handed a big 2...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>-0.5423</td>
      <td>Thu Mar 22 16:59:39 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.280</td>
      <td>0.720</td>
      <td>0.000</td>
      <td>1</td>
      <td>Police video shows fatal shooting of unarmed b...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>0.0000</td>
      <td>Thu Mar 22 16:15:47 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>EU and six other countries exempted from US me...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>-0.1280</td>
      <td>Thu Mar 22 15:50:31 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.207</td>
      <td>0.631</td>
      <td>0.162</td>
      <td>1</td>
      <td>Donald Trump's top Russia lawyer John Dowd res...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.0000</td>
      <td>Thu Mar 22 15:40:24 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Skating Lake Baikal, the world's deepest lake ...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0.0000</td>
      <td>Thu Mar 22 14:55:08 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Cambridge Analytica taken to court over data s...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>-0.6705</td>
      <td>Thu Mar 22 14:54:46 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.290</td>
      <td>0.539</td>
      <td>0.172</td>
      <td>1</td>
      <td>Serving six years for killing her abusive ex-p...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>0.1779</td>
      <td>Thu Mar 22 14:33:39 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.215</td>
      <td>0.519</td>
      <td>0.267</td>
      <td>1</td>
      <td>Ukraine arrests pilot hero Savchenko over 'cou...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>-0.5106</td>
      <td>Thu Mar 22 14:27:54 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.248</td>
      <td>0.752</td>
      <td>0.000</td>
      <td>1</td>
      <td>Dapchi kidnap: Nigerian father's pain as daugh...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>-0.7184</td>
      <td>Thu Mar 22 14:22:27 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.500</td>
      <td>0.500</td>
      <td>0.000</td>
      <td>1</td>
      <td>YouTube gun ban drives bloggers to PornHub htt...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>0.0000</td>
      <td>Thu Mar 22 14:17:41 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Dutch parliament halted as man jumps from publ...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.0000</td>
      <td>Thu Mar 22 14:17:41 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Cynthia Nixon and 10 other celebrities who ent...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0.0000</td>
      <td>Thu Mar 22 13:49:56 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Nigerian girls tell of kidnap ordeal https://t...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>-0.6249</td>
      <td>Thu Mar 22 13:37:39 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.254</td>
      <td>0.746</td>
      <td>0.000</td>
      <td>1</td>
      <td>Meet Louis - the gorilla who walks upright to ...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>-0.1531</td>
      <td>Thu Mar 22 13:37:10 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.167</td>
      <td>0.833</td>
      <td>0.000</td>
      <td>1</td>
      <td>Miss Venezuela to close temporarily over corru...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>0.0000</td>
      <td>Thu Mar 22 13:24:26 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>1</td>
      <td>Austria recalls diplomat from Israel over Nazi...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>0.4854</td>
      <td>Thu Mar 22 13:21:44 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>0.813</td>
      <td>0.187</td>
      <td>1</td>
      <td>RT @BBC_HaveYourSay: Iran Supreme Leader Ali K...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>-0.3182</td>
      <td>Thu Mar 22 13:16:07 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.247</td>
      <td>0.753</td>
      <td>0.000</td>
      <td>1</td>
      <td>Iranian officials mocked for using foreign pro...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.2263</td>
      <td>Thu Mar 22 12:44:28 +0000 2018</td>
      <td>@BBCWorld</td>
      <td>0.000</td>
      <td>0.808</td>
      <td>0.192</td>
      <td>1</td>
      <td>Tennessee grants $1 million to wrongly convict...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>470</th>
      <td>-0.5574</td>
      <td>Thu Mar 22 09:00:10 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.399</td>
      <td>0.413</td>
      <td>0.188</td>
      <td>5</td>
      <td>2 American tourists were killed in a helicopte...</td>
    </tr>
    <tr>
      <th>471</th>
      <td>0.6343</td>
      <td>Thu Mar 22 08:44:06 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.057</td>
      <td>0.694</td>
      <td>0.250</td>
      <td>5</td>
      <td>A new agreement signals warmer ties between Ir...</td>
    </tr>
    <tr>
      <th>472</th>
      <td>0.0243</td>
      <td>Thu Mar 22 08:30:13 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.142</td>
      <td>0.710</td>
      <td>0.148</td>
      <td>5</td>
      <td>Untangling yourself from a site like Facebook ...</td>
    </tr>
    <tr>
      <th>473</th>
      <td>0.7351</td>
      <td>Thu Mar 22 08:15:10 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.581</td>
      <td>0.419</td>
      <td>5</td>
      <td>RT @bxchen: New Tech Fix: Want to #deletefaceb...</td>
    </tr>
    <tr>
      <th>474</th>
      <td>0.5994</td>
      <td>Thu Mar 22 08:02:04 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.726</td>
      <td>0.274</td>
      <td>5</td>
      <td>"People with dementia also need adventures. Th...</td>
    </tr>
    <tr>
      <th>475</th>
      <td>0.4798</td>
      <td>Thu Mar 22 07:32:03 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.822</td>
      <td>0.178</td>
      <td>5</td>
      <td>Emmanuel Macron has made clear that he's not a...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>0.0000</td>
      <td>Thu Mar 22 07:17:06 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>Our technology reporter in Mumbai, @vindugoel,...</td>
    </tr>
    <tr>
      <th>477</th>
      <td>-0.3182</td>
      <td>Thu Mar 22 07:10:13 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.204</td>
      <td>0.796</td>
      <td>0.000</td>
      <td>5</td>
      <td>For Heart Disease Patients, Think Exercise, No...</td>
    </tr>
    <tr>
      <th>478</th>
      <td>0.3612</td>
      <td>Thu Mar 22 07:02:03 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.872</td>
      <td>0.128</td>
      <td>5</td>
      <td>Stephen Hawking's ashes will be interred at We...</td>
    </tr>
    <tr>
      <th>479</th>
      <td>-0.7269</td>
      <td>Thu Mar 22 06:35:01 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.243</td>
      <td>0.757</td>
      <td>0.000</td>
      <td>5</td>
      <td>"When I deployed to Iraq in 2003, there was no...</td>
    </tr>
    <tr>
      <th>480</th>
      <td>0.6705</td>
      <td>Thu Mar 22 06:18:02 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.776</td>
      <td>0.224</td>
      <td>5</td>
      <td>RT @nytopinion: .@tomfriedman: The G.O.P. is a...</td>
    </tr>
    <tr>
      <th>481</th>
      <td>0.6124</td>
      <td>Thu Mar 22 05:56:21 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.615</td>
      <td>0.385</td>
      <td>5</td>
      <td>How to enjoy fine dining on a fast food budget...</td>
    </tr>
    <tr>
      <th>482</th>
      <td>0.3434</td>
      <td>Thu Mar 22 05:39:00 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.100</td>
      <td>0.734</td>
      <td>0.165</td>
      <td>5</td>
      <td>Many counties, including rich ones, are aging ...</td>
    </tr>
    <tr>
      <th>483</th>
      <td>0.0000</td>
      <td>Thu Mar 22 05:21:04 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>Facebook chief executive Mark Zuckerberg spoke...</td>
    </tr>
    <tr>
      <th>484</th>
      <td>-0.5106</td>
      <td>Thu Mar 22 05:02:03 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.155</td>
      <td>0.845</td>
      <td>0.000</td>
      <td>5</td>
      <td>RT @sangerkatz: Getting sick can be really exp...</td>
    </tr>
    <tr>
      <th>485</th>
      <td>-0.2960</td>
      <td>Thu Mar 22 04:32:05 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.104</td>
      <td>0.896</td>
      <td>0.000</td>
      <td>5</td>
      <td>President Trump met with the Saudis on Tuesday...</td>
    </tr>
    <tr>
      <th>486</th>
      <td>0.0000</td>
      <td>Thu Mar 22 04:17:01 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>When Bill de Blasio sealed his come-from-behin...</td>
    </tr>
    <tr>
      <th>487</th>
      <td>-0.4404</td>
      <td>Thu Mar 22 04:06:05 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.153</td>
      <td>0.847</td>
      <td>0.000</td>
      <td>5</td>
      <td>The FBI investigated Jeff Sessions for possibl...</td>
    </tr>
    <tr>
      <th>488</th>
      <td>0.0000</td>
      <td>Thu Mar 22 03:56:10 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>Is Facebook too big to quit? Here are answers ...</td>
    </tr>
    <tr>
      <th>489</th>
      <td>0.0000</td>
      <td>Thu Mar 22 03:42:01 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>RT @NYTHealth: Some 400 to 500 women worldwide...</td>
    </tr>
    <tr>
      <th>490</th>
      <td>-0.5267</td>
      <td>Thu Mar 22 03:32:04 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.167</td>
      <td>0.833</td>
      <td>0.000</td>
      <td>5</td>
      <td>Robotic fish could be essential to understandi...</td>
    </tr>
    <tr>
      <th>491</th>
      <td>0.3818</td>
      <td>Thu Mar 22 03:17:04 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.867</td>
      <td>0.133</td>
      <td>5</td>
      <td>Marjory Stoneman Douglas High School in Florid...</td>
    </tr>
    <tr>
      <th>492</th>
      <td>0.6808</td>
      <td>Thu Mar 22 03:02:04 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.694</td>
      <td>0.306</td>
      <td>5</td>
      <td>A Memphis megachurch pastor whose congregation...</td>
    </tr>
    <tr>
      <th>493</th>
      <td>0.5719</td>
      <td>Thu Mar 22 02:46:02 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.810</td>
      <td>0.190</td>
      <td>5</td>
      <td>RT @adamgoldmanNYT: Mr. Nader has been granted...</td>
    </tr>
    <tr>
      <th>494</th>
      <td>0.4939</td>
      <td>Thu Mar 22 02:32:05 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>0.842</td>
      <td>0.158</td>
      <td>5</td>
      <td>With government funding set to run out this we...</td>
    </tr>
    <tr>
      <th>495</th>
      <td>0.0000</td>
      <td>Thu Mar 22 02:17:03 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>Facebook chief executive Mark Zuckerberg spoke...</td>
    </tr>
    <tr>
      <th>496</th>
      <td>0.0000</td>
      <td>Thu Mar 22 02:02:04 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>At a time when women cyclists were viewed as i...</td>
    </tr>
    <tr>
      <th>497</th>
      <td>0.0000</td>
      <td>Thu Mar 22 01:47:03 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>A red truck. Pink construction gloves. And the...</td>
    </tr>
    <tr>
      <th>498</th>
      <td>0.0000</td>
      <td>Thu Mar 22 01:31:06 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>RT @kevinroose: We talked with Mark Zuckerberg...</td>
    </tr>
    <tr>
      <th>499</th>
      <td>0.0000</td>
      <td>Thu Mar 22 01:17:04 +0000 2018</td>
      <td>@nytimes</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>5</td>
      <td>The hashtag #DeleteFacebook appeared more than...</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 8 columns</p>
</div>




```python
sentiments_pd.to_csv("News_sentiments.csv",encoding="utf-8",index=False)
```

Creat Scatter Plot


```python
#creat scatter plot
colors=["DarkRed","Grey","Red","Blue","Black"]
media_source=sentiments_pd["Media"].unique()
i=0
plt.figure(figsize=(12,10))
for channel in target_terms:
    print(channel)
    tweet_ago=np.arange(len(sentiments_pd[sentiments_pd['Media']==channel]['Compound Score']))
    plt.scatter(tweet_ago,sentiments_pd[sentiments_pd['Media']==channel]['Compound Score'],marker='o',color=colors[i],s=100,edgecolor="black",label=channel)
    i+=1
plt.title("Sentitment Analysis of Tweet(%s)"%(time.strftime("%x")))
plt.ylabel("Tweet Polarity")
plt.xlabel("Tweets Ago")
plt.xlim(105,-5)
plt.legend(loc="best",bbox_to_anchor=(1,0.5),frameon=True)
plt.savefig("Output_Sentiment_Analysis")
plt.grid(True)
plt.show()
```

    @BBCWorld
    @CBSNews
    @CNN
    @FoxNews
    @nytimes
    


![png](output_9_1.png)


Creat The Overall Bar Chart


```python
#Get compound scores for each news channels for the bar graph
sentiments_table=sentiments_pd.pivot_table(index='Media',values='Compound Score',aggfunc=np.mean)
sentiments_table
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Compound Score</th>
    </tr>
    <tr>
      <th>Media</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>@BBCWorld</th>
      <td>-0.118619</td>
    </tr>
    <tr>
      <th>@CBSNews</th>
      <td>-0.060230</td>
    </tr>
    <tr>
      <th>@CNN</th>
      <td>-0.095281</td>
    </tr>
    <tr>
      <th>@FoxNews</th>
      <td>-0.029469</td>
    </tr>
    <tr>
      <th>@nytimes</th>
      <td>0.010934</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Creat bar plot visualizing the overall sentiments of the last 100 tweets from each channels
x_axis=np.arange(len(sentiments_table.index.values))
tick_locations=[value+0.4 for value in x_axis]
fig,ax=plt.subplots(figsize=(10,10))
plt.xticks(tick_locations,sentiments_table.index.values,rotation="horizontal")
rect=plt.bar(sentiments_table.index.values,sentiments_table["Compound Score"],color=colors, alpha=1,align="edge")
plt.grid
```




    <function matplotlib.pyplot.grid>




```python
plt.title("Overall Media Sentiment based on Twitter(%s)"%(time.strftime("%x")))
plt.xlabel("Media")
plt.ylabel("Tweet Polarity")
ax.grid(linestyle='dotted')
plt.savefig("Output_compoundscore_analysis")
plt.show()
```


![png](output_13_0.png)


Obeservations:
1. @nytimes has the most positive tweets in those five media channels
2. @BBCWorld has the most negative tweets in those five media channels. 
3. All of those five media channels contents are more neutral, becuase their scores are close to zero. 
