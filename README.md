### README

## Community choice

For this project, I chose the r/F1Discussions subreddit. The community is home to various points of views and discussions on Formula 1. As for labels, I had 2 in my final version:

'argument': 'argument': A post/comment classfied as 'argument' is one that tries to make a certain point, and also has supporting evidence to back it up. It may or may not be true, but it is supported and somewhat thought out. Examples:

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

- 'question': A post/comment classified as 'question' is one that proposes a question that could be about anything, ranging from predictions about the future or current events. 

1. 
Where do we think Sainz actually ends up long term?

Williams feels like a stopgap to me. If Audi or Cadillac come knocking with a proper project I think Carlos jumps. Hard to see him spending his prime in the midfield.

2. 
"Where were you when you first learned about Lewis' move to Ferrari, and what were your first thoughts and reactions?
Personally, I was really surprised. I was a new fan, and wasn't expecting that to happen at all.

How did you guys feel?"

---

Originally, I also had a 'speculation' label. However, I encountered problems since there were many posts that could be reasonably considered both speculation and questions, so I decided to combine the two.


## Data Collection

TO collect the data, I had Claude Code write a script that would scrape around 250 posts from the subreddit, and also take a first pass at classifying them. I then went through manually, removing any that were not either of the two labels or correcting incorrect labeling. An issue I had was that I had many more 'argument' posts, so I used  synthetic sampling to create synthetic 'question' posts to achieve a 50/50 split. Here were some difficult examples:

---

Would Michael Schumacher have won the WDC in 2007 and 2008 had he not retired?

The most obvious answer here is yes, but I don’t think it’s actually that straightforward. 

It‘s important to note that Kimi signed his Ferrari contract in 2005, meaning in this hypothetical scenario Schumacher would’ve been up against Raikkonen and not Massa- a much bigger test than any of his previous teammates. I also don’t think he was the absolute best driver in F1 by the time he retired- Alonso showed that in 2006 and had a prime Schumacher been competing he may have taken the title that season. My knowledge of that era isn’t the best but I don’t think it’d be far fetched to say Kimi was slightly better at this point also. 

That being said, he did have a better record against Massa in 2006 than Kimi did in 2007. It was only after Schumacher announced his retirement that Massa started to take poles and wins, whilst Kimi didn’t hit the ground running in 2007 (debut win in Aus aside) straight away and it was 9 podiums including 5 wins in the last 10 races that won him the title. 

Having rewatched 2007 fairly recently, I honestly believe the two McLaren drivers were better than Kimi that season. There were more races where Ferrari were the outright fastest car that year than there were for McLaren, although it is important to note that Kimi lost more points from mechanical issues than either McLaren driver. 

As for 2008- Lewis, Massa and Raikkonen all had their fair share of bozo moments, and it’s very easy to say that a Schumacher/Alonso would’ve walked to that title- but not as easy to actually prove. 

I’m genuinely curious what people have to think on this one because I genuinely don’t know, and I guess we’ll never actually know

---

The issue with this post and many others like it is that while it proposes a question with the intention of starting a discussion, it also argues for certain outcome of that question. I eventually decided to prioritize the fact that it is open to discussion, and made it a question.

## Fine Tuning Approach

Using the distilbert-base-uncased model, I used the following hyper-parameters:

output_dir="./takemeter-model",
    num_train_epochs=3,
    per_device_train_batch_size=4,
    per_device_eval_batch_size=32,
    learning_rate=2e-5,
    weight_decay=0.1,
    warmup_steps=75,
    eval_strategy="epoch",
    save_strategy="epoch",
    save_total_limit=1,
    load_best_model_at_end=True,
    metric_for_best_model="accuracy",
    logging_steps=10,
    report_to="none",

One of the main things I changed for better accuracy was the per_device_train_batch_size, lowering it all the way down to 4. Since the data set was fairly small, a low batch size helped with filtering out some of the noise. 

## Evaluation Report

Here were the final results: 

==================================================
RESULTS COMPARISON
==================================================
Model                               Accuracy
---------------------------------------------
Zero-shot baseline (Groq)              0.963
Fine-tuned DistilBERT                  0.800
---------------------------------------------

Fine-tuning regression: 0.163

| | Predicted: argument | Predicted: question |
|---|---|---|
| **True: argument** | 13 | 2 |
| **True: question** | 4 | 11 |

24/30 correct = **80% accuracy**.


The fine-tuned model actualy performed significantly worse than the groq baseline. I think this can be attributed more to my choice of labels and communtiy than the fine tuning itself. Since the labels are somewhat ambiguous, with many posts being reasonably both questions or arguments, groq perfomed very well, seeing that more reasoning was needed. 

# Wrong predictions and analysis

1. Text:      I mean, it's normal when you don't like it when your teammate is ahead, but suffering to death?

I mean, are we deadass? They think as if Charles is overconfident cocky bastard who thinks that he's nu...
True:      argument
Predicted: question  (confidence: 0.58)

This is an example of a post proposing a question, then providing an argument after. I had orignally labeled it an argument, but it was predicted to be a question.

2. Text:      Anyone else think lawson has built his reputation back?

Liam Lawson has to be one of the most impressive stories on the grid right now.

Since returning to Raging Bulls, he’s been one of the most con...
True:      question
Predicted: argument  (confidence: 0.81)

Again, the argument provided is much longer than the question, so it was predicted to be an argument.

3. Text:      Would Michael Schumacher have won the WDC in 2007 and 2008 had he not retired?

The most obvious answer here is yes, but I don’t think it’s actually that straightforward. 

It‘s important to note that...
True:      question
Predicted: argument  (confidence: 0.82)

Again, we have a hybrid post. It seems like the model did not recognize the patter of question posts as well as it could have.

# Example outputs

| | Text | True Label | Predicted Label | Confidence |
|:-:|---|---|---|:-:|
| **0** | It’s been 12 days since Monaco and I still don... | argument | argument | 0.82 |
| **1** | all the drama online about this season is utte... | argument | argument | 0.68 |
| **2** | Not at all a good look for mercedes. They have... | argument | argument | 0.70 |
| **3** | Mark Webber is underrated\n\nI was never a fan... | argument | argument | 0.82 |
| **4** | What was your reaction to this?\n\nI was (and ... | question | question | 0.73 |

In the first example, the post argues for an outcome of a certain race, which was correctly predicted to be an argument.


## Reflection

The model performed about as well as it could have given the situation and data set. It seemed to have prioritized large argumentative sections to classify posts as 'argument', even if they started or ended with an open ended question which I would've classified as a 'question'. 

## Spec Reflection

The planning document gave me a good opportunity to plan out my thoughts. I could catch when something didn't make sense, correct myself, and have a better, more clearly conveyed idea. This also reduced the amount of time I had to spend ammending Claude Code's output, since it was given clear instrcutions.

I had originally started with 3 labels, and when I realized that differentiating between the 3 was causing low accuracy (not more than 0.6), I ammended my spec and chose to stick to just 2 labels. 

## AI Usage

I had claude code write me a script to scrape posts and take a first pass at classifiying the data set as well. For the script, it started with the Reddit API, which would have required me to apply for a key, and would've taken much longer. Instead, I instructed it to use **Acrtic-Shift** api instead.

