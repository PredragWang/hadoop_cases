<html>
<head>
<title>Assignment 1: Twitter Sentiment Analysis in Python</title>

</head>

<body>

<div>

<p>
Twitter represents a fundamentally new instrument to make social measurements. Millions of people voluntarily express opinions across any topic imaginable --- this data source is incredibly valuable for both research and business.</p>

<p>For example, researchers have shown that the &quot;mood&quot; of communication on twitter <a
href="http://www.nytimes.com/2011/09/30/science/30twitter.html"><strong>reflects biological rhythms</strong></a> and can even used to <a
href="http://arxiv.org/pdf/1010.3003&embedded=true"><strong>predict the stock market</strong></a>. A
student here at UW used geocoded tweets to <a
href="http://cliffmass.blogspot.com/2012/07/thunderstorm-fest.html"><strong>plot a map of locations where &quot;thunder&quot; was mentioned in the context of a storm system in Summer 2012.</strong></a></p>

<p>Researchers from Northeastern University and Harvard University studying the characteristics and dynamics of Twitter <a
href="http://www.ccs.neu.edu/home/amislove/twittermood/"><strong>have an excellent resource</strong></a> for learning more about how Twitter can be used to analyze moods at national scale.</p>

<p>In this assignment, you will
<ul>
<li> access the twitter Application Programming Interface(API) using python</li>
<li>estimate the public's perception (the <em>sentiment</em>) of a particular term or phrase</li>
<li>analyze the relationship between location and mood based on a sample of twitter data</li>
</ul>
</p>

<p>Some points to keep in mind:
<ul>
<li>This assignment is open-ended in several ways. You'll need to make some decisions about how best to solve the problem and implement them carefully.</li>
<li><strong>It is perfectly acceptable to discuss your solution on the forum, but don't share code.</strong></li>
<li><strong>Each student must submit their own solution to the problem.</strong></li>
<li>You will have an unlimited number of tries for each submission.</li>
<li>Your code will be run in a protected environment, so you should only use the Python standard libraries unless you are specifically instructed otherwise. Your code should also not rely on any external libraries or web services.</li>
</ul>
</p>

<h4>The Twitter Application Programming Interface</h4>

<p>Twitter provides a very rich REST API for querying the system, accessing data, and control your account. You can <a href="https://dev.twitter.com/docs">read more about the Twitter API</a></p>

<h4>Python 2.7.3 environment</h4>

<p>If you are new to Python, you may find it valuable to work through the <a href="http://www.codecademy.com/tracks/python">codeacademy Python tutorials</a>. Focus on tutorials 1-9, plus tutorial 12 on File IO. In addition, many students have recommended <a href="https://developers.google.com/edu/python/">Google's Python class</a>.</p>

<p>You will need to establish a Python programming environment to complete this assignment. You can install Python yourself by <a href="http://www.python.org/download/">downloading it from the Python website</a>, or can use the <a href="https://spark-public.s3.amazonaws.com/datasci/Coursera-Data-Science-Ubuntu.ova">class virtual machine</a>.
</p>

<h4>Unicode strings</h4>

<p>Strings in the twitter data prefixed with the letter &quot;u&quot; are unicode strings. For example:</p>

<pre>u&quot;This is a string&quot;</pre>

<p>Unicode is a standard for representing a much larger variety of characters beyond the roman alphabet (greek, russian, mathematical symbols, logograms from non-phonetic
writing systems such as kanji, etc.) </p>

<p>In most circumstances, you will be able to use a unicode object just like a string.</p>

<p>If you encounter an error involving printing unicode, you can use the <a
href="http://docs.python.org/2/library/stdtypes.html#str.encode">encode</a> method to properly print the international characters, like this:</p>

<pre>
unicode_string = u"aaaàçççñññ"
encoded_string = unicode_string.encode('utf-8')
print encoded_string
</pre>

<h4>Getting Started</h4>

<p>Once again: If you are new to Python, many students have recommended <a href="https://developers.google.com/edu/python/">Google's Python class.</a></p>


<h3>Problem 1: Get Twitter Data</h3>

<p>As always, the first step is to <a href="https://class.coursera.org/datasci-002/wiki/GithubInstructions">make sure your assignment materials up to date.</a></p>

<p>To access the live stream, you will need to install the <a href="http://pypi.python.org/pypi/oauth2/"><strong>oauth2 library</strong></a> so you can properly authenticate.</p>

<p>This library is already installed on the <a href="https://spark-public.s3.amazonaws.com/datasci/Coursera-Data-Science-Ubuntu.ova">class virtual machine</span></a>, but you can install it yourself in your Python environment.  (The command <code>$ pip install oauth2</code> should work for most environments.)</p>

<p>The steps below will help you set up your twitter account to be able to access the live 1% stream.</p>

<ol>
<li>Create a twitter account if you do not already have one.</li>

<li>Go to <a href="https://dev.twitter.com/apps"><strong>https://dev.twitter.com/apps</strong></a> and log in with your twitter credentials.</li>

<li>Click &quot;Create New App&quot;</li>

<li>Fill out the form and agree to the terms. Put in a dummy website if you don't have one you want to use.</li>

<li>On the next page, click the &quot;API Keys&quot; tab along the top, then scroll all the way down until you see the section &quot;Your Access Token&quot;</li>

<li>Click the button &quot;Create My Access Token&quot;. You can <a href="https://dev.twitter.com/docs/auth"><strong>Read more about Oauth authorization.</strong></a></li>

<li>You will now copy four values into twitterstream.py.  These values are your &quot;API Key&quot;, your &quot;API secret&quot;, your &quot;Access token&quot; and your &quot;Access token secret&quot;.  All four should now be visible on the API Keys page. (You may see &quot;API Key&quot; referred to as &quot;Consumer key&quot; in some places in the code or on the web; they are synonyms.) Open twitterstream.py and set the variables corresponding to the api key, api secret, access token, and access secret.  You will see code like the below: 

<pre>
api_key = "&lt;Enter api key&gt;"
api_secret = "&lt;Enter api secret&gt;"
access_token_key = "&lt;Enter your access token key here&gt;"
access_token_secret = "&lt;Enter your access token secret here&gt;"

</pre>
</li>

<li>Run the following and make sure you see data flowing and that no errors occur.

<pre>$ python twitterstream.py > output.txt</pre>

This command pipes the output to a file.  Stop the program with Ctrl-C, but wait at least <strong>3 minutes</strong> for data to accumulate. Keep the file output.txt for the duration of the assignment; we will be reusing it in later problems.  Don't use someone else's file; we will check for uniqueness in other parts of the assignment.
</li>

<li>If you wish, modify the file to use the <a href="https://dev.twitter.com/docs/api/1.1/get/search/tweets">twitter search API</a> to search for specific terms.  For example, to search for the term &quot;microsoft&quot;, you can pass the following url to the twitterreq function:
<pre>
https://api.twitter.com/1.1/search/tweets.json?q=microsoft
</pre>
</li>

</ol>


<p class="turnin">What to turn in: The first 20 lines of the twitter data you downloaded from the web. You can save the first 20 lines to a file <code>problem_1_submission.txt</code> by using the following command:
<pre>$ head -n 20 output.txt > problem_1_submission.txt</pre>
</p>

<h3>Problem 2: Derive the sentiment of each tweet</h3>

<p>For this part, you will compute the sentiment of each tweet based on the sentiment scores of the terms in the tweet. The sentiment of a tweet is equivalent to the sum of the sentiment scores for each term in the tweet.</p>

<p>You are provided with a skeleton file <code>tweet_sentiment.py</code> which accepts two arguments on the command line: a <em>sentiment file</em> and a tweet file like the one you generated in Problem 1. You can run the skeleton program like this:</p>

<pre>$ python tweet_sentiment.py AFINN-111.txt output.txt</pre>

<p>The file AFINN-111.txt contains a list of pre-computed sentiment scores. Each line in the file contains a word or phrase followed by a sentiment score. Each word or phrase that is found in a tweet but not found in AFINN-111.txt should be given a sentiment score of 0. See the file AFINN-README.txt for more information.</p>

<p>To use the data in the AFINN-111.txt file, you may find it useful to build a dictionary.  Note that the AFINN-111.txt file format is tab-delimited, meaning that the term and the score are separated by a tab character.  A tab character can be identified a "\t".The following snippet may be useful:

<pre>
afinnfile = open("AFINN-111.txt")
scores = {} # initialize an empty dictionary
for line in afinnfile:
  term, score  = line.split("\t")  # The file is tab-delimited. "\t" means "tab character"
  scores[term] = int(score)  # Convert the score to an integer.

print scores.items() # Print every (term, score) pair in the dictionary
</pre>
</p>  

<p>The data in the tweet file you generated in Problem 1 is represented as <em>JSON</em>, which stands for JavaScript Object Notation. It is a simple format for representing nested structures of data --- lists of lists of dictionaries of lists of .... you get the idea.</p>

<p>Each line of <code>output.txt</code> represents a <a href="https://dev.twitter.com/docs/streaming-apis/messages">streaming message</a>.  Most, but not all, will be <a href="https://dev.twitter.com/docs/platform-objects/tweets">tweets</a>. (The skeleton program will tell you how many lines are in the file.)</p>

<p>It is straightforward to convert a JSON string into a Python data structure; there is a library to do so called <code>json</code>.</p>

<p>To use this library, add the following to the top of <code>tweet_sentiment.py</code></p>

<pre>import json</pre>

<p>Then, to parse the data in <code>output.txt</code>, you want to apply the function <code>json.loads</code> to every line in the file.</p>

<p>This function will parse the json data and return a python data stucture; in this case, it returns a dictionary.  If needed, take a moment to <strong><a href="http://docs.python.org/2/library/stdtypes.html#typesmapping">read the documentation for Python dictionaries</a></strong>.</p>

<p>You can read the <a href="https://dev.twitter.com/docs/platform-objects/tweets">Twitter documentation</a> to understand what information each tweet contains and how to access it, but it's not too difficult to deduce the structure by direct inspection.</p>

<p>Your script should print to stdout the sentiment of each tweet in the file, one numeric sentiment score per line.  The first score should correspond to the first tweet, the second score should correspond to the second tweet, and so on.  If you sort the scores, they won't match up.  If you sort the tweets, they won't match up.  If you put the tweets into a dictionary, the order will not be preserved.   Once again: <strong class="important">The nth line of the file you submit should contain only a single number that represents the score of the nth tweet in the input file!</strong></p>

<p class="important">NOTE: You must provide a score for <strong>every</strong> tweet in the sample file, even if that score is zero.  You can assume the sample file will only include English tweets and no other types of streaming messages.</p>

<p>To grade your submission, we will run your program on a tweet file formatted the same way as the <code>output.txt</code> file you generated in Problem 1.</p>

<p>Hint: This is real-world data, and it can be messy!   Refer to the <a href="https://dev.twitter.com/docs/platform-objects/tweets">twitter documentation</a> to understand more about the data structure you are working with.  Don't get discouraged, and ask for help on the forums if you get stuck!</p>

<p class="turnin">What to turn in: The file <code>tweet_sentiment.py</code> after you've verified that it returns the correct answers.</p>

<h3>Problem 3: Derive the sentiment of new terms</h3>

<p>In this part you will be creating a script that computes the sentiment for the terms that <strong>do not</strong> appear in the file AFINN-111.txt.</p>

<p>Here's how you might think about the problem: We know we can use the sentiment-carrying words in AFINN-111.txt to deduce the overall sentiment of a tweet.  Once you deduce the sentiment of a tweet, you can work backwards to deduce the sentiment of the non-sentiment carrying words that do <em>not</em> appear in AFINN-111.txt.  For example, if the word <code>soccer</code> always appears in proximity with positive words like <code>great</code> and <code>fun</code>, then we can deduce that the term <code>soccer</code> itself carries a positive sentiment.</p>

<p>Don't feel obligated to use it, but the following paper may be helpful for developing a sentiment metric.  Look at the Opinion Estimation subsection of the Text Analysis section in particular.</p>

<p><a href="http://www.cs.cmu.edu/~nasmith/papers/oconnor+balasubramanyan+routledge+smith.icwsm10.pdf">O'Connor, B., Balasubramanyan, R., Routedge, B., &amp; Smith, N. From Tweets to Polls: Linking Text Sentiment to Public Opinion Time Series. (ICWSM), May 2010.</a></p>

<p>You are provided with a skeleton file <code>term_sentiment.py</code> which accepts the same two arguments as tweet_sentiment.py and can be executed using the following command:

<pre>$ python term_sentiment.py AFINN-111.txt output.txt</pre>

</p>

<p>Your script should print output to stdout.  Each line of output should contain a term, followed by a space, followed by the sentiment.  That is, each line should be in the format <code>&lt;term:string&gt; &lt;sentiment:float&gt;</code></p>

<p>For example, if you have the pair <code>("foo", 103.256)</code> in Python, it should appear in the output as:

<pre>foo 103.256</pre>
</p>

<p>The order of your output does not matter.</p>

<p class="turnin">What to turn in: The file <code>term_sentiment.py</code></p>

<p>How we will grade Part 3: We will run your script on a file that contains strongly positive and strongly negative tweets and verify that the non-sentiment-carrying terms in the strongly positive tweets are assigned a higher score than the non-sentiment-carrying terms in negative tweets.  Your scores need not (and likely will not) exactly match any specific solution.</p>

<p>If the grader is returning "Formatting error: <line of output>", make note of the line of text returned in the message.  This line corresponds to a line of your output.  The grader will generate this error if <code>line.split()</code> does not return <strong>exactly two items</strong>.  One common source of this error is to not remove the two calls to the "lines" function in the solution template; this function prints the number of lines in each file.  Make sure to check the first two lines of your output!</p>

<h3>Problem 4: Compute Term Frequency</h3>

<p>Write a Python script <code>frequency.py</code> to compute the <em>term frequency histogram</em> of the livestream data you harvested from Problem 1.</p>

<p>The frequency of a term can be calculated as <code>[# of occurrences of the term in all tweets]/[# of occurrences of all terms in all tweets]</code></p>

<p>Your script will be run from the command line like this:

<pre>$ python frequency.py &lt;tweet_file&gt;</pre>
</p>

<p>You should assume the tweet file contains data formatted the same way as the livestream data.</p>


<p>Your script should print output to stdout.  Each line of output should contain a term, followed by a space, followed by the frequency of that term in the <strong>entire file</strong>. There should be one line per <strong>unique</strong> term in the entire file. Even if 25 tweets contain the word <code>lol</code>, the term <code>lol</code> should only appear once in your output (and the frequency will be at least 25!)  Each line should be in the format <code>&lt;term:string&gt; &lt;frequency:float&gt;</code></p>

<p>For example, if you have the pair <code>(bar, 0.1245)</code> in Python it should appear in the output as:

<pre>bar 0.1245</pre>
</p>

<p>If you wish, you may consider a term to be a multi-word phrase, but this is not required.  You may compute the frequencies of individual tokens only.</p>

<p>Depending on your method of parsing, you may end up computing frequencies for hashtags, links, stop words, phrases, etc.  If you choose to filter out these non-words, that's ok too.</p>

<p class="turnin">What to turn in: The file <code>frequency.py</code></p>

<h3>Problem 5: Which State is happiest?</h3>

<p>Write a Python script <code>happiest_state.py</code> that returns the name of the happiest state
as a string.</p>

<p>Your script <code>happiest_state.py</code> should take a file of tweets as input.  It will be called from the command line like this:

<pre>$ python happiest_state.py &lt;sentiment_file&gt; &lt;tweet_file&gt;</pre>
</p>

<p>The file <code>AFINN-111.txt</code> contains a list of pre-computed sentiment score.</pre>

<p>Assume the tweet file contains data formatted the same way as the livestream data.</pre>

<p>It's a good idea to make use of your solution to Problem 2.</p>

<p>There are different ways you might assign a location to a tweet.  Here are three:
<ul>

<li>Use the <code>coordinates</code> field (a part of the <code>place</code> object, if it exists, to geocode the tweet.  This method gives the most reliable location information, but unfortunately this field is not always available and you must figure out some way of translating the coordinates into a state.</li>

<li>Use the other metadata in the <code>place</code> field.  Much of this information is hand-entered by the twitter user and may not always be present or reliable, and may not typically contain a state name.</li>

<li>Use the <code>user</code> field to determine the twitter user's home city and state.  This location does not necessarily correspond to the location where the tweet was posted, but it's reasonable to use it as a proxy.</li>
</ul>
</p>

<p>You are free to develop your own strategy for determining the state that each tweet
originates from.</p>

<p>You may find it useful to use this <a href="http://code.activestate.com/recipes/577305-python-dictionary-of-us-states-and-territories/">python dictionary of state abbreviations</a>.</p>

<p>You can ignore any tweets for which you cannot assign a location in the United States.</p>

<p>In this file, each line is a Tweet object, as<a
href="https://dev.twitter.com/docs/platform-objects/tweets">described in the twitter documentation</a>.</p>

<p>Note: Not every tweet will have a <code>text</code> field --- again, real data is dirty!  Be prepared to debug, and feel free to throw out tweets that your code can't handle to get
something working.  For example, you might choose to ignore all non-English tweets.</p>

<p>Your script should print the two letter state abbreviation of the state with the highest average tweet sentiment to stdout.</p>

<p>Note that you may need a <strong>lot</strong> of tweets in order to get enough tweets with location data.  Let the live stream run for a while if you wish.</p>

<p><strong>Your script will not have access to the Internet, so you cannot rely on third party services to resolve geocoded locations!</strong></p>

<p class="turnin">What to turn in: The file <code>happiest_state.py</code></p>

<h3>Problem 6: Top ten hash tags</h3>

<p>Write a Python script <code> top_ten.py</code> that computes the ten most frequently occurring hashtags from the data you gathered in Problem 1.</p>


<p>Your script will be run from the command line like this:

<pre>$ python top_ten.py &lt;tweet_file&gt;</pre>
</p>

<p>You should assume the tweet file contains data formatted the same way as the livestream data.</p>

<p>In the tweet file, each line is a Tweet object, as <a href="https://dev.twitter.com/docs/platform-objects/tweets">described in the twitter documentation</a>.  To find the hashtags, you should not parse the <code>text</code> field; the hashtags have already been extracted by twitter.</p>

<p>Your script should print to stdout each hashtag-count pair, one per line, in the following format:</span></p>


<p>Your script should print output to stdout.  Each line of output should contain a hashtag, followed by a space, followed by the frequency of that hashtag in the <strong>entire file</strong>. There should be one line per <strong>unique</strong> hashtag in the entire file.  Each line should be in the format <code>&lt;hashtag:string&gt; &lt;frequency:float&gt;</code></p>

<p>For example, if you have the pair <code>(bar, 30)</code> in Python it should appear in the output as:

<pre>bar 30</pre>
</p>

<p class="turnin">What to turn in: the file <code>top_ten.py</code></p>

</div>

</body>

</html>

