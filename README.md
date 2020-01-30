# GT India Hackathon Feb 2020

## Table of Contents:
1. Background
2. Problem Definition
3. Test cases and Helper code
4. Solution Expected / Submission
5. How will the solution be evaluated?

## 1. Background

Data analysis on real-time streaming data is becoming fairly common these days. 
Typically, streaming data is unbounded and made available incrementally over time 
as opposed to all at once. Analysis on streaming data can take various interesting forms - 
ranging from twitter feed sentiment analysis, discovering tending topics on social media, 
to anomaly detection in sensor data in internet-of-things or even CC transactions .

The problem defined below captures the essence of the above and many interesting variations
can be build on top of this. 

## 2. Problem Definition

`Problem`

	Write an app called 'top_words' that:
	
	0. Reads streaming data.
	1. For every N second time-window, keeps an account of the K most frequently used 
	   words in that time-window
	2. Provides an endpoint ('localhost:8888/top') for clients that shows what those K words 
	   are at the moment.
	3. Time-window is optional such that if its not provided then the
	   app simply provides top K words since beginning. 
	4. 'top_words' takes 2 integer arguments, first is K, second is time window in seconds
	   Example 1: 'top_words 15 3' means top 15 words, reset every 3 seconds   
	   Example 2: 'top_words 15' means top 15 words, beginning-to-end of stream   
   
`Example:`
   
    Assume N=10 seconds, K=100, and words.txt is being streamed into your app.
    This means every 10 seconds your app will refresh the most frequently seen 100 words.
    At any time, a client can hit the app's endpoint and it would provide top 100 words at
    that moment.
    Example output is shown below in 3.1 
     
`Hints`

    0. App can simulate streaming data by reading from unix pipe. 
       Example: cat words.txt | top_words 
    1. A web server, embedded or in another process could provide the endpoint. 
    2. As you are developing with small files, you many see streamin is too fast. Helper code
       is provided that can slow it down. Use as:
       > cat words.txt | ./stream_flow_control| ./top_words
    3. If time-window is made optional and set to False, then at the end of streaming in 
       the app would simply have the top K words of the file you are streaming. 
       This would help test functionality of the core logic. 
    4. The top K words should be appear sorted in descending order by frequency while 
       writing to log or hitting endpoint. 
    5. If multiple words occur with the same count, its fine to keep any of those.
    6. Keep original case i.e dont change 'Airport' to 'airport' 
    7. It would be good to stick to Python 3 std library as much as possible and keep non-
       standard libraries to minimum.  

       
## 3. Test cases and Helper code

3.1  You can use words.txt to check functionality for top 10 words:
   > cat words.txt | ./stream_flow_control| ./top_words 10

Should print these words (sorted preferred)
   (8, 'Jurassic'), (7, 'Spielberg'), (4, 'Will'), (4, 'Smith'), 
   (4, 'Park'), (4, 'Jaws'), (2, 'Sony'), (2, 'James'), (2, 'Inception'), 
   (2, 'Bond')
   In this testcase, there are other words with a count of 2, so its ok to drop any 

3.2 Testcase 2, 
> cat war-and-peace.txt | ./top_words 15

(1963, 'Pierre'), (1579, 'Prince'), (1213, 'Natasha'), (1181, 'man'), 
(1143, 'Andrew'), (1017, 'himself'), (924, 'time'), (893, 'face'), 
(881, 'French'), (843, 'know'), (826, 'eyes'), (812, 'old'), (781, 'men'), 
(776, 'Rostov'), (771, 'room')
## 4. Solution Expected / Submission

```
The goal should be to write code that is close to production quality as possible. 
Primarily, this is what you would be evaulated on.  Production quality means:
1. Test coverage of the core part of code or what you changed in the code.
2. You system design
3. Code maintainability

Instructions on actual submission would be shared on a Slack channel. The submission
should have:
1. Your code with a README file. It should run with the same command-line as above.
2. Unit tests
3. And finally a short write up on what would you do differently if this has to be
   made web scale to handle an extremely large amount of traffic. 
```
## 4. How would the solution be evaluated?
```
1. We will run you app on testcases (not shared with you) for functional correctness 
   and completeness.
2. Code covers edge cases and has sanity checks in place.
3. When streaming data ends i.e. app cannot read from std in anymore, it should shut down
   cleanly.
4. Your write up on how to scale this for massive amount of streaming data.
```