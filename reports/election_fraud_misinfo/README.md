# YouTube - Election Fraud Misiniformation

A high level overview of this report, including the ability to explore videos discussing the topic, can be found here:
https://pendulumco.webflow.io/reports/election-fraud-mis-disinformation-report


## Narrative Definition

We define the “election fraud” narrative in a very similar way to this portion of YouTube’s updated terms of service: “Content that advances false claims that widespread fraud, errors, or glitches changed the outcome of any past U.S. presidential election”.

The two differences are that:
- We only focus on the 2020 presidential election.
- YouTube hasn’t clearly defined “advances false claims”, so we must use our own definition. We determine that any clip in which the content creator is making statements clearly aimed at increasing the viewers chances of believing such false claims are included. Easily interpreted forms of insinuation and parody count as well. We also take the video title and description into account.

Videos are either labeled as “supporting” the narrative, “disputing” the narrative, or “other” if they are discussing "election fraud", but not in a manner that is related to the narrative or in a manner which doesn't make their stance clear.


## Election Fraud Discussion Heuristic

We detect “election fraud” discussion in videos by searching the transcripts (automatically generated closed captions) of videos uploaded by political channels for pairs of keywords commonly used when discussing the topic. These keywords don’t necessarily need to be adjacent, but must be relatively close to each other. Specifically we do the following:
- Video transcripts consist of a series of chunks of text. We create “snippets” from these chunks by either using a sliding window that combines four contiguous chunks of text at a time or combining enough chunks to create a snippet with 100 or more tokens.
- We identify snippets in which one or more of the words “election”, “ballot”, or “vote” occur along with one or of the words “fraud”, “rigged”, “stole”, “steal”, and “theft”. We also consider words in which these are prefixes (such as plural versions of the nouns).

Our labeled data indicates that this heuristic has high precision as we’ve found that only 21 / 795 of the cases identified by it were discussing something other than “election fraud”. More analysis needs to be done to measure the recall (coverage) of the heuristic, but based on the immense scale of videos identified so far, it appears to be high as well. We leave this analysis of recall for future work.


## Data Labeling

Only the first mention of “election fraud” in each video was reviewed. If the label for the first snippet in a video is unclear, then subsequent snippets are analyzed. If a snippet is found that is a clear “support” or “dispute”, then the entire video is given that label.

For our initial analysis of “election fraud” narratives in November 2020, we focused on videos uploaded between Nov 3 and Nov 10. In total 375 mentions of election fraud were labeled including the top 160 viewed “partisan right” videos and top 110 viewed videos from all other channels. These manually labeled videos account for a small portion of the 4,895 videos discussing “election fraud” during this period. However, they have a combined 282M views and they cover 64% of the overall “election fraud” video views during the period.

During our recent expansion we’ve labeled another 400 videos. This included the top 100 viewed videos from the following categories:
- Videos predicted to “support” narrative uploaded between Nov 3 and Dec 9
- Videos predicted to “dispute” narrative uploaded between Nov 3 and Dec 9
- Videos predicted to “support” narrative uploaded between Dec 10 and Jan 31
- Videos predicted to “dispute” narrative uploaded between Dec 10 and Jan 31

These were chosen to account for the fact that videos uploaded after YouTube changed their TOS on Dec 9 are less likely to support the narrative. The combined sets of labeled videos account for 980M of the total 2.5B views (40%) to videos discussing “election fraud” between Nov 3 and Jan 31.

This distribution of labels were as follows: 374 support, 302 dispute, and 119 other. The way videos were sampled for labeling has impacted this distribution and the overall distribution skews more towards “dispute.”


## Election Fraud Stance Heuristic

Manually reviewing videos is very time consuming. Due to this we only label 795 videos and use a heuristic to predict labels for the remaining 29,445.

This heuristic relies on political labels and left right bias defined by Transparency Tube. It works as follows:
- If other videos by the channel have been manually labeled, use the majority label from these manually labeled videos.
- Otherwise, use the majority label for all other labeled videos from channels with the same soft tags and political lean.

Due to the small number of videos labeled “other”, the heuristic only uses “supporting” and “disputing” labels when making predictions.

We use hold-one-out cross validation to measure the performance of the heuristic and find that it has an accuracy of 81%. This includes a precision of 0.80 and recall of 0.96 for “support” and a precision of 0.83 and recall of 0.95 for “dispute”. However, pre TOS the performance is a bit higher and post TOS it is lower. Pre TOS, the heuristic has a precision of 0.84 and recall of 0.96 for “support” and a precision of 0.84 and recall of 0.94 for “dispute”. Post TOS, the heuristic has a precision of 0.68 and recall of 0.97 for “support” and a precision of 0.80 and recall of 0.97 for “dispute”.


## Limitations

Due to issues with YouTube’s default transcripts, a small percentage of the video links go to a portion of the video that is not aligned with the section of the transcript displayed on this page. You may have to manually select an earlier spot in the video inorder to watch the portion the snippet should be aligned with.

There are a small number of channels that don’t have transcripts enabled. One prominent example is CNN. Further analysis needs to be done to estimate how including channels with disabled transcripts would increase “supporting” and “disputing” view aggregates.
