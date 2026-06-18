### General Purpose of This Project

Create a fine-tuned model capable of differentiating between the types of "takes" in a online community. The goal overall is classification.

## Community

For this project, we will be using the subreddit r/F1Discussions. This is a good pick for the task of classification since there are various takes ranging from analysis to hot takes. Users provide various statements like interesting statistics about certain drivers, opionons on their recent performance, or speculation on future moves.

## Labels

- 'speculation': A post/comment classified as 'speculation' is one that makes predictions about the future actions of a driver or team, or guesses about information that is not readily available. Examples:

1. 
What is going on with Mercedes and George?

Body: I can't help but feel iffy about this whole situation. Spain was an improvement from past few races, but still, almost no crew members showed up to his podium. Feel deflated and sad about the DNF and potential lost win all you want, but one of your drivers is being celebrated on a podium, after 100 GPs with the team, at least show up.

Even their LinkedIn post talks about strategy error, Lewis and Kimi, not even mentioning George. That's not ig or TikTok, LinkedIn is corporate platform. And the YouTube community post isn't much better. 10 photos, George is in 1 of them. The driver who didn't finish the race gets more spotlight than a podium finisher.

They keep saying George has great support from the team, and maybe he does behind the scenes, but would it hurt to actually show that support publicly? The praise sounds empty when the team does stuff like this. Toto can praised George after quali, but if this happens when it actually matters - after the race where the points are awarded and where you can publicly show how you stand by your driver, it isn’t a good look...

2. 
Ralf Schumacher speculated on Sky Sports Germany that Max Verstappen already received an underwhelming Mercedes offer for 2027. Jos Verstappen responded "Ralf, again you bring wrong information".

---

- 'argument': A post/comment classfied as 'argument' is one that tries to make a certain point, and also has supporting evidence to back it up. It may or may not be true, but it is supported and somewhat thought out. Examples:

1. 
Since so many people are bashing Piastri right now

I want to make a post just to remind everybody what an absolutely fantastic and borderline perfect weekend Piastri had last year,at Qatar.

Won every single session except the race,and was miles beyond everybody in terms of pace all weekend.Didnt make a single mistake,mishap,off track,missed braking zone,nothing at all.

Did all this whilst being in the most intense titlefight since 2021,anyone who watched last season knows how much tension there was the last few races.Will Norris run away with it,will Max actually cluth it,will Piastri make a comeback?

So Piastri definetly can handle the pressure.Its bollocks that he cant.He is human sure,but his calm and collected nature is for sure there.

Had it not been for Mclarens incompetence,not wanting to risk doublestacking Norris during the safetycar Piastri would have also not only won the race but most likely have a grand slam. In the end they fucked both of their races.

2. 
People don't understand how many GOAT tier drivers have rough patches.

Man people need to watch more old F1 seasons. 2 crashes and a string of unfortunate and slightly off pace races and everyone thinks they are shit. This goes for Leclerc, Russell and Liastri are getting so much shit for not performing at their best rn.

Almost every F1 champion has had moments like these. Watch Hamiltons 2008 season, half of his races included some form of mistake. Prost 1989 was faster than Senna only like 3 times over the course of an entire season. Prost also crashed from the lead 3 times in 1983. Hakkinnen crashed 3 times in 99 from a comfortable lead as well afaik. Schumacher threw away his 06 title with cheating in Monaco, crashing in Australia, crashing in Hungary.

Like god forbid a driver has a rough patch... almost every even GOAT tier driver has had moments like these, people forget how difficult this sport is and recency bias is unbelievable. I mean, Hamilton is GOAT tier but he's won once and suddenly every post on every F1 sub is saying he is far superior to Leclerc, ignoring the bast year of absolute underperformance. People need to remember how tough this sport is and not take 3 bad results to draw conclusions over a drivers full career.

---

- 'question': A post/comment that can be classified as a 'question' is one that proposes something to the reader, usually with the intention of starting a discussion. Examples:

1. 
Where were you when you first learned about Lewis' move to Ferrari, and what were your first thoughts and reactions?
Personally, I was really surprised. I was a new fan, and wasn't expecting that to happen at all.

How did you guys feel?

(Edit: One more question. As a football fan, would this be the equivalent of messi/ronaldo leaving Man City to go to Real Madrid?)

2. 
What do you think about Jenson Button and his Formula 1 career?


## Hard Edge Cases

When classifying, there may be inputs that could reasonably be placed in to more than one category. 

One specific example are posts that start witha question that isn't open ended, and rather has a binary answer. If it goes on to try and support one of the binary answers, it should be handled as an argument, rather than a question.

## Data Collection

To get our 200 samples, we can run a web scraping script on r/F1Discussions. WE should aim for a roughly even split of the 3 labels, but with a script we should simply take what we can get. To ensure that the least represented category on the subreddit, 'question' gets adequete coverage, we can run the script in 2 rounds.

1. Try to collect about 40 samples with question marks in them. These will likely be questions, or discussions.

2. For the remaining 200, we will simply keep collecting posts as they appear.

In total then, we end up with 240 samples, which we can then clean down to 200 by removing samples that are noisy or don't adhere to the 3 categories. 

## Evaluation

1. Accuracy - a rate of 85-90% is reasonable

2. The ability to classify edge cases reasonably.

## Definition of success

If the tool passes the evaluation metrics above, then it would be ok for deployment. 

## AI Tool Plan

I'll ask claude code to generate me a script that follows the data collection's spec. 

I'll also ask it to create 10 edge cases from this document, 6 of which will be used in the training set, and the remaining 4 in the eval set.

Finally, I will have claude code have a first pass at labeling the data set before going through it myself. 
