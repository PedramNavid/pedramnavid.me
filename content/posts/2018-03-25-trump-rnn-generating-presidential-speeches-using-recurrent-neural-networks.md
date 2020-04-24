---
title: Trump RNN - Generating Presidential Speeches using Recurrent Neural Networks
author: Pedram Navid
date: '2018-03-25'
slug: trump-rnn-generating-presidential-speeches-using-recurrent-neural-networks
categories: []
tags:
  - deep learning
  - neural networks
  - rnn
  - python
---

With Trump's victory, I thought it would be interesting to scope out some
Trump-related datasets and get some practice with different machine learning
algorithms. I've seen some work with generative text with [Markov
Chains](https://github.com/jsvine/markovify), but thought a [Recurrent Neural
Network](http://machinelearningmastery.com/text-generation-lstm-recurrent-neural-networks-python-keras/)
might be a little more fun to play with.

## Web Scraping

Grabbing a large enough dataset was the first problem to solve. I did find a
[Github that had some Trump
speeches](https://github.com/ryanmcdermott/trump-speeches) but the data was 3
months old as of this post. I decided instead to create a simple scraper to
capture all 55 of his speeches from [The American Presidency
Project](http://www.presidency.ucsb.edu/). It had been several years since I've
really coded with Python, so a simple scraper seemed like a good way to get back
into it.

**[My Python Trump Speech Scraper with 55
speeches.](https://github.com/PedramNavid/trump_speeches)**

The code itself is very simple and not very elegant, but it gets the job done. I
used Beautiful Soup to target the links on the main listing of speeches, and a
second scraper to capture each individual speech. There's a few things I still
would like to do, just like error catching, and figuring out a more elegant way
to write the data into text, but for the purposes of creating a RNN, the script
works just fine. Before I started working on the script, though, I threw the
code up on Twitter to a few friends of mine from the #rstats world and before I
knew it, [David Robinson](https://twitter.com/drob), of Stack Overflow, had
already done an interesting bit of analysis on the text data. [caption
id="attachment_65" align="aligncenter" width="792"]![Relationship Graph by David
Robinson:
http://rpubs.com/dgrtwo/227833](/img/drob-trump.jpeg)
Relationship Graph by David Robinson: http://rpubs.com/dgrtwo/227833[/caption]

## Recurrent Neural Network

This part proved to be the hardest. The plan was use torch-rnn to build a neural
network using the speeches from the scraper to generate new text. But I couldn't
get torch to work properly on Windows, so I had to install Ubuntu to get a Linux
session going, and then start working through dependencies to install everything
I need to build then neural net.
[torch-rnn](https://github.com/jcjohnson/torch-rnn) was based on
[char-rnn](https://github.com/karpathy/char-rnn) but with the added benefit of
using Torch which can take advantage of GPU processing for faster training.

Installing Torch was relatively straight-forward, so long as you follow the
[instructions on the Torch
website,](http://torch.ch/docs/getting-started.html#_) and don't decide to try
installing luarocks and other packages through Ubunutu's package manager
instead. With Torch installed and tested, the computer rebooted and secure-bios
turned off (can't forget that step..), I finally could get to the task of
training the rnn. The rest of the steps were trivial. With the data copied over
to the torch-rnn data folder, I trained for several days (!), a total of 522000
iterations and 2000 epochs. With training done, I was able to sample using the
most recent checkpoint to get some pretty decent (if sometimes nonsensical)
results. Here are some results along with the text used to seed the generation.

**Make America** 

```
Make America safe again.And we are going to win the White House.It's
going to be a signature to restore dignity and opportunity to every child in
this country who have suffered so much previous to the problem is that the
American people will send new skyscrapers into the World Trade
Organization.
```

**Data Science** 

```
Data science if I believe it, and continued to shut representative
regulations on the patriotic security interests of the media wall. The problem
is that couple -in illegal immigrant required this nation. American workers. Our
steelworkershily, of the United States has lost leverstand November 8th, and
make this corrupt begins, are in a great jobs every American and walking out
$800 million jobs.We are going by right of disas Americans you're going to cover
up with Russia, will happens want to start, the mejored the nation is a result
of these communities, and aggressive dibilions from federal law enforcement and
education, on the poor African American communities when I don't let people who
era of favors for any successful flag something, our actions foine how victory
house
```

Although the results are mostly nonsensical and
sometimes not even English, we can get a good sense of what the RNN is seeing in
Trump's speeches. Phrases like security interests, illegal immigrant, American
workers, great jobs and $800 million jobs, poor African American communities,
are all turns of phrase that can easily be read in Trump's voice.

## Next Steps

55 speeches in the grand scheme of things isn't that large. The entire dataset
is slightly under one megabyte. I could definitely use more training data to
help the RNN with its output. Despite all that, it was a pretty interesting
project and a great way to dip my toes into neural nets.