---
title: "Computer vision applied to the recognition of Magic cards"
tags:
  - ml
  - python
  - mtg
toc: true
toc_label: "Computer vision applied to the recognition of Magic cards"
toc_sticky: true
header:
    teaser: /assets/images/mtgscan/arena_cards.jpg
    og_image: /assets/images/mtgscan/arena_cards.jpg
---

[**Web application using Azure OCR, Flask, Celery, Redis, SocketIO.**](http://mtgscan.azurewebsites.net)

# Introduction

[Magic](https://magic.wizards.com/en) is a card game (played both online and IRL) in which you have to build a pile of cards (*deck*) to play against other players. The number of cards in a deck can be large (between 40 and 100, most of the time) and listing them (to attend a tournament or sell them, for example) can be tedious.  
In this article, I describe the development of **mtgscan**, a recognition tool to automatically list cards from an image, which can be a photo or a screenshot.

# Card recognition

Most attempts to recognize Magic cards seem to rely on using image processing and/or training a neural network (usually CNN) on the overall image.  
For example, [this](https://tmikonen.github.io/quantitatively/2020-01-01-magic-card-detector) article describe the following pipeline :  
- Pre-process the image: resizing, histogram equalization...  
- Segmentation: isolation of each card, by finding convex hulls
- Recognition: compute the perceptual hash of each card and compare it to a database of all hashs of known cards.  
[This other project](https://github.com/hj3yoo/mtg_card_detector) tried to use a neural network combined with perceptual hashing, but without improving much.

However, those attempts do not work if the cards are stacked, as is usually the case when listing a deck or collection.

Instead, I used an OCR to recognize the title of the cards, rather than the whole image.

<center><a href="https://github.com/fortierq/mtgscan"><img src="https://gh-card.dev/repos/fortierq/mtgscan.svg" /></a></center>
<br>
This project is explained below.

## Text recognition (OCR)

There are several existing OCR. I firstly tried [Tesseract](https://tesseract-ocr.github.io/) which is probably the best open-source OCR. However, the results were pretty bad.

Therefore, I tried proprietary, cloud solutions: 
- [Azure Read OCR](https://docs.microsoft.com/fr-fr/azure/cognitive-services/computer-vision/overview-ocr)
- [Google Cloud Vision](https://cloud.google.com/vision/docs/ocr)
- [AWS Textract](https://aws.amazon.com/textract)

Those OCR can be only used in the cloud and can't be used locally. They are free for a limited use, which is more than enough for me. For example, [the free tier of Azure OCR](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/computer-vision) grant up to 20 requests by minute and 5000 by month.  
They are much more efficient that Tesseract and, after comparing them on few images, I opted for the Azure OCR.  

![](/assets/images/mtgscan/ocr.jpg)
<center>Aside from the swamp (hard to see), the results of Azure OCR are very good.</center>

## Post-processing by approximate string matching

For each recognized block of text, we need to check if it is a card name. To this end, I store a dictionary of all Magic cards, from [mtgjson](https://mtgjson.com/). This is very time-efficient since a lookup in a Python dictionary is O(1).  
However, the OCR often makes small errors:

![](/assets/images/mtgscan/bazaar_ocr.jpg)

Here, left parenthesis have been mistakenly added to some card names, two cards have truncated names and a Karakas in sideboard is totally wrong.  
To fix those problems, I used fuzzy search (approximate string matching) with [SymSpell](https://github.com/wolfgarbe/SymSpell), which find the closest word in a dictionnary by [edit distance](https://en.wikipedia.org/wiki/Edit_distance).  
To be efficient, SymSpell expects a bound on the distance. By experience, a maximum edit distance of 6 gave good results. I also reject a text if its ratio distance/length (percentage of errors per character) is too high.

Other post-processing that I found useful:
- Reject texts that are too short (< 3 characters) or too long (> 30).
- If '..' appears in text, it might mean that the name is truncated. Hence I do a prefix search in the SymSpell dictionary.
- Some frequent keywords on cards are mistakenly detected as card name. Therefore I rejected some cards such as [Sacrifice](https://gatherer.wizards.com/Pages/Card/Details.aspx?name=sacrifice). 

## Results

Here is the result on the above screenshot:

![](/assets/images/mtgscan/bazaar_cards.jpg)

We can spot 3 errors:
- An extra Squee, Goblin Nabob is detected.
- [The Ur-Dragon](https://gatherer.wizards.com/pages/card/Details.aspx?multiverseid=479384) is not recognized, because of its mana cost being added to its name.
- The second Karakas is not recognized, due to too many error from the OCR.

[On this test set](https://github.com/fortierq/mtgscan/tree/master/tests/samples), I got a 10% error rate, which seems good to me: the decklist can be corrected by hand while still saving a lot of time.

# Web application

<center><a href="https://github.com/fortierq/mtgscan-app"><img src="https://gh-card.dev/repos/fortierq/mtgscan-app.svg" /></a></center>
