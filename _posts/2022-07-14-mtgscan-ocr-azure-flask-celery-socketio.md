---
title: "Computer vision applied to the recognition of Magic cards"
tags:
  - ml
  - python
  - ocr
  - azure
  - celery
  - socketio
toc: true
toc_label: "Computer vision applied to the recognition of Magic cards"
toc_sticky: true
header:
    teaser: /assets/images/mtgscan/arena_cards.jpg
    og_image: /assets/images/mtgscan/arena_cards.jpg
---

Mtgscan is a project aiming at recognizing Magic cards from an image (a photo or a screenshot), using OCR (Optical Character Recognition). <br>
<center><b><a href="http://34.132.83.57">Test the application (an URL to an image is pre filled)</a></b></center>

<center>
<div style="display: inline-block; padding:20px;"><a href="https://github.com/fortierq/mtgscan"><img src="https://gh-card.dev/repos/fortierq/mtgscan.svg" /></a></div>
<div style="display: inline-block; padding:20px;"><a href="https://github.com/fortierq/mtgscan-app"><img src="https://gh-card.dev/repos/fortierq/mtgscan-app.svg" /></a></div>
</center>

# Introduction

[Magic](https://magic.wizards.com/en) is a card game (played both online and IRL) in which you have to build a pile of cards (*deck*) to play against other players. The number of cards in a deck can be large (between 40 and 75, most of the time) and listing them by hand (to attend a tournament or sell them, for example) can be tedious.  

# Card recognition

Most attempts to recognize Magic cards seem to rely on using image processing and/or training a neural network (usually CNN) on the overall image.  
For example, [this](https://tmikonen.github.io/quantitatively/2020-01-01-magic-card-detector) article describe the following pipeline :  
- **Pre-process the image**: resizing, histogram equalization...  
- **Segmentation**: isolation of each card, by finding convex hulls.
- **Recognition**: compute the perceptual hash of each card and compare it to a database of all hashs of known cards.  
[This other project](https://github.com/hj3yoo/mtg_card_detector) tried to use a neural network combined with perceptual hashing, but without improving much.

However, those attempts do not work if the cards are stacked, as is usually the case when listing a deck or collection.

Instead, I used an OCR to recognize the title of the cards, rather than the whole image.

## Text recognition (OCR)

There are several existing OCR. I firstly tried [Tesseract](https://tesseract-ocr.github.io/) which is probably the best open-source OCR. However, the results were pretty bad.

Therefore, I turned to proprietary, cloud solutions: 
- [Azure Read OCR](https://docs.microsoft.com/fr-fr/azure/cognitive-services/computer-vision/overview-ocr)
- [Google Cloud Vision](https://cloud.google.com/vision/docs/ocr)
- [AWS Textract](https://aws.amazon.com/textract)

Those OCR can be only used in the cloud and can't be used locally. They are free for a limited use, which is more than enough for me. They are much more efficient that Tesseract and, after comparing them on few images, I opted for the Azure OCR which was performing better.  

<center>
<table class="tg">
<thead>
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-c3ow"><b>Azure</b></th>
    <th class="tg-c3ow"><b>GCP</b></th>
    <th class="tg-c3ow"><b>AWS</b></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-c3ow">OCR</td>
    <td class="tg-c3ow">Read</td>
    <td class="tg-c3ow">Vision</td>
    <td class="tg-c3ow">Textract</td>
  </tr>
  <tr>
    <td class="tg-c3ow">Limits</td>
    <td class="tg-c3ow">5000/month and 20/minute</td>
    <td class="tg-c3ow">1000/month</td>
    <td class="tg-c3ow">1000/month</td>
  </tr>
</tbody>
</table>
</center>

Azure OCR can be used through http request, where results are accessed via [polling](https://en.wikipedia.org/wiki/Polling_(computer_science)), each recognized block containing a bounding box and a text:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Ffortierq%2Fmtgscan%2Fblob%2F51286baf116a37dfac9e87803b24a297ea461398%2Fmtgscan%2Focr%2Fazure.py%23L28-L53&style=github&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>

<center><img src="/assets/images/mtgscan/ocr.jpg" width="80%"></center>

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
- An extra Squee, Goblin Nabob is detected in the rules text.  
- [The Ur-Dragon](https://gatherer.wizards.com/pages/card/Details.aspx?multiverseid=479384) is not recognized, because of its mana cost being added to its name.
- The second Karakas is not recognized, due to too many error from the OCR.

[On this test set](https://github.com/fortierq/mtgscan/tree/master/tests/samples), I got a 10% error rate, which seems good to me: the decklist can be corrected by hand while still saving a lot of time.

# Web application

## Web application framework: Flask

I used [Flask](https://flask.palletsprojects.com) as a web application framework. By default, Flask uses the web server included in [Werkzeug](https://werkzeug.palletsprojects.com), which is easy to use but not suited for production.

## Task queue: Celery

Since scanning an image takes some time (roughly 10 seconds), I used [Celery](https://docs.celeryq.dev) task queue to scan cards as a background job (a worker), thus not freezing the server. Another possibility was to process images asynchronously with [asyncio](https://docs.python.org/fr/3/library/asyncio.html), but that would only use one thread which is not suited for scaling the server.  

## Message broker: Redis

To transfer messages between the web application and the workers, we need a message queue. Popular choices include RabbitMQ, ZeroMQ and [Redis](https://redis.io). I used the later, which can also be used as a database. 

## Communication between client and server: SocketIO

To send an image from the browser's client to the server, I used [SocketIO](https://socket.io/). It is based on [WebSocket](https://en.wikipedia.org/wiki/WebSocket), which is a protocol for bidirectional communication between a client and a server. When the server has finished processing an image, it sends the decklist back to the client:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Ffortierq%2Fmtgscan-app%2Fblob%2F069203a12e804f8e741dea8c92823b532e094ec9%2Fserver%2Fstatic%2Findex.js&style=vs&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>

## Web server: Eventlet

Unfortunately, Werkzeug does not support WebSockets. I used [Eventlet](https://eventlet.net) instead.

## Putting it all together

![](/assets/images/mtgscan-app.png)

Connect Redis to SocketIO and Celery:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Ffortierq%2Fmtgscan-app%2Fblob%2F069203a12e804f8e741dea8c92823b532e094ec9%2Fserver%2Fapp.py%23L15-L19&style=vs&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>

Get an image from the client, send it to the worker and send the decklist back:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Ffortierq%2Fmtgscan-app%2Fblob%2F069203a12e804f8e741dea8c92823b532e094ec9%2Fserver%2Fapp.py%23L34-L53&style=vs&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>

## Containerization: Docker

I used the following Dockerfile for Flask and Celery:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Ffortierq%2Fmtgscan-app%2Fblob%2F069203a12e804f8e741dea8c92823b532e094ec9%2Fserver%2FDockerfile&style=vs&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>
Where `poetry.lock` contains the requirements of the application.

Finally, docker-compose starts Flask, Celery and Redis:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Ffortierq%2Fmtgscan-app%2Fblob%2Fmaster%2Fdocker-compose.yml&style=vs&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>
The env files contain the environment variables: the credentials for Azure and the password used for Redis.

## Deployment: GCP

When it comes to deployment, I tried GCP and Azure. GCP's free tier is more generous since it includes a [free VM (e2-micro)](https://cloud.google.com/compute/vm-instance-pricing#e2_sharedcore_machine_types). Azure only have free App services that offer much less liberty and does not support docker-compose very well.  
The app hosted on GCP is available at [http://mtgscan.net](http://mtgscan.net).

# Perspectives

- Use HTTPS instead of HTTP for [http://mtgscan.net](http://mtgscan.net).
- Make a Twitter bot scanning cards (by using [http://mtgscan.net](http://mtgscan.net) through a REST API), when mentioned.  
- Adapt the app for other card games.
