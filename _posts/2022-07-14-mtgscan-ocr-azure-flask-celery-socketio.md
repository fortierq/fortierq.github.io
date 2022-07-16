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

[Magic](https://magic.wizards.com/en) is a card game (played both online and IRL) in which you have to build a pile of cards (*deck*) to play against other players. The number of cards in a deck can be large (between 40 and 75, most of the time) and listing them by hand (to attend a tournament or sell them, for example) is tedious.  

# Card recognition

Most attempts to recognize Magic cards seem to rely on using image processing and/or training a neural network (usually CNN) on the overall image.  
For example, [this article](https://tmikonen.github.io/quantitatively/2020-01-01-magic-card-detector) pre-process the image, get each card by segmentation and recognize them using a perceptual hash together with a database of card hashs.  
[This other project](https://github.com/hj3yoo/mtg_card_detector) tried to use a neural network combined with perceptual hashing, but without improving much.

However, those attempts do not work if the cards are stacked, as is usually the case when listing a deck or collection.

<center><img src="/assets/images/mtgscan/arena_cards.jpg" width="100%">
<b>Example of stacked cards recognized by mtgscan: each card is only partially visible</b></center><br>

Instead, **I used an OCR to recognize the title of the cards**, rather than the whole image.

## Text recognition (OCR)

There are several existing OCR. I firstly tried [Tesseract](https://tesseract-ocr.github.io/) which is probably the best open-source OCR but the results turned out to be pretty bad.

Therefore, I considered proprietary, cloud solutions: 
- [Azure Read OCR](https://docs.microsoft.com/fr-fr/azure/cognitive-services/computer-vision/overview-ocr)
- [Google Cloud Vision](https://cloud.google.com/vision/docs/ocr)
- [AWS Textract](https://aws.amazon.com/textract)

Those OCRs can be only used in the cloud and can't be used locally. They are free for a limited use, which is more than enough for me. They are much more efficient that Tesseract and, after comparing them on few images, I opted for the **Azure OCR** which was performing better.  

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

<center><img src="/assets/images/mtgscan/ocr.jpg" width="80%"><br>
<b>Aside from the swamp (hard to see), the results of Azure OCR are very good.</b></center>

## Card name recognition

Then, I **lookup in a [card dictionary](https://mtgjson.com/)** for each block of text to decide if it is indeed a card name. However, the OCR often makes small errors that we need to correct first.

## OCR post-processing and approximate string matching

![](/assets/images/mtgscan/bazaar_ocr.jpg)

There are several problems with the OCR result on the above image:
- Some characters have been mistakenly added (`(`, mana value...) to card names.
- [The Ur-Dragon](https://gatherer.wizards.com/pages/card/Details.aspx?multiverseid=479384) and [Hogaak, Arisen Necropolis](https://gatherer.wizards.com/Pages/Card/Details.aspx?multiverseid=464151) have truncated names.
- The [Karakas](https://gatherer.wizards.com/pages/card/details.aspx?name=Karakas) in sideboard is totally wrong.  

To fix those problems, I used **fuzzy search (approximate string matching)** with [SymSpell](https://github.com/wolfgarbe/SymSpell), which find the closest word in a dictionary by [edit distance](https://en.wikipedia.org/wiki/Edit_distance). See [this article](https://seekstorm.com/blog/1000x-spelling-correction) for more details.   
To be efficient, SymSpell expects a bound on the distance. By experience, a maximum edit distance of 6 gave good results. I also reject a text if its ratio distance/length (percentage of errors per character) is too high.

Other post-processing that I found useful:
- Reject texts that are too short (< 3 characters) or too long (> 30).
- Remove special characters (@, !, ...).
- If '..' appears in text, it might mean that the name is truncated. Hence I do a prefix search in the SymSpell dictionary.
- Some frequent keywords on cards are mistakenly detected as card name. Therefore I rejected some cards such as [Sacrifice](https://gatherer.wizards.com/Pages/Card/Details.aspx?name=sacrifice). 
- Try to detect card multipliers (e.g. 4x).

## Results

Here is the result on the above screenshot:

![](/assets/images/mtgscan/bazaar_cards.jpg)

We can spot 3 errors:
- An extra [Squee, Goblin Nabob](https://gatherer.wizards.com/pages/card/Details.aspx?multiverseid=106473) is detected in the rules text.  
- [The Ur-Dragon](https://gatherer.wizards.com/pages/card/Details.aspx?multiverseid=479384) is not recognized, because of its mana cost being added to its name.
- The [Karakas](https://gatherer.wizards.com/pages/card/details.aspx?name=Karakas) in sideboard is not recognized, due to too many error from the OCR.

[On this test set](https://github.com/fortierq/mtgscan/tree/master/tests/samples), I got a **10% error rate**, which seems good to me: the decklist can be corrected by hand while still saving a lot of time.

# [Web application](http://mtgscan.net)

## Web application framework: Flask

I used [Flask](https://flask.palletsprojects.com) as a web application framework. By default, Flask uses the web server included in [Werkzeug](https://werkzeug.palletsprojects.com), which is easy to use but not suited for production.

## Task queue: Celery

Since scanning an image takes some time (roughly 10 seconds), I used [Celery](https://docs.celeryq.dev) task queue to scan cards as a background job (a worker), thus not freezing the server. Another possibility was to process images asynchronously with [asyncio](https://docs.python.org/fr/3/library/asyncio.html), but that would only use one thread which is not suited for scaling the server.  

## Message broker: Redis

To transfer messages between the web application and the workers, we need a message queue. Popular choices include RabbitMQ, ZeroMQ and [Redis](https://redis.io). I used the later, which can also be used as a database. 

## Communication between client and server: SocketIO

To send an image from the browser's client to the server, I used [SocketIO](https://socket.io/). It is based on [WebSocket](https://en.wikipedia.org/wiki/WebSocket), which is a protocol for bidirectional communication between a client and a server. When the server has finished processing an image, it sends the decklist back to the client:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Ffortierq%2Fmtgscan-app%2Fblob%2Fmaster%2Fserver%2Fstatic%2Findex.js&style=vs&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>

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

## Deployment: GCP vs Azure

When it comes to deployment, I tried GCP and Azure:  

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-yj5y{background-color:#efefef;border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-v0hj{background-color:#efefef;border-color:inherit;font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
</style>
<center>
<table class="tg">
<thead>
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-yj5y"><span style="font-weight:bold">Azure App Service F1</span></th>
    <th class="tg-yj5y"><span style="font-weight:bold">Azure App Service B1</span></th>
    <th class="tg-yj5y"><span style="font-weight:bold">Azure VM B1s</span></th>
    <th class="tg-v0hj">Azure VM B2s</th>
    <th class="tg-v0hj">GCP VM e2-micro</th>
    <th class="tg-v0hj">GCP VM e2-medium</th>
    <th class="tg-v0hj">GCP VM e2-standard-2</th>
    <th class="tg-v0hj">GCP App Engine</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-v0hj">RAM</td>
    <td class="tg-c3ow">1 GB</td>
    <td class="tg-c3ow">1.75 GB</td>
    <td class="tg-c3ow">1 GB</td>
    <td class="tg-c3ow">4 GB</td>
    <td class="tg-c3ow">1 GB</td>
    <td class="tg-c3ow">4GB</td>
    <td class="tg-c3ow">8 GB</td>
    <td class="tg-c3ow"></td>
  </tr>
  <tr>
    <td class="tg-v0hj">CPU</td>
    <td class="tg-c3ow">Shared</td>
    <td class="tg-c3ow">Dedicated</td>
    <td class="tg-c3ow">1 vCPU</td>
    <td class="tg-c3ow">2 vCPU</td>
    <td class="tg-c3ow">2 vCPUs shared</td>
    <td class="tg-c3ow">2 vCPU shared</td>
    <td class="tg-c3ow">2 vCPU</td>
    <td class="tg-c3ow"></td>
  </tr>
  <tr>
    <td class="tg-v0hj">Price/month</td>
    <td class="tg-c3ow">1 Free</td>
    <td class="tg-c3ow">$13.14</td>
    <td class="tg-c3ow">$7.59</td>
    <td class="tg-c3ow">$30.37</td>
    <td class="tg-c3ow">$6.11</td>
    <td class="tg-c3ow">$24.46</td>
    <td class="tg-c3ow">$48.91</td>
    <td class="tg-c3ow"></td>
  </tr>
  <tr>
    <td class="tg-v0hj">Support compose</td>
    <td class="tg-c3ow">Partially</td>
    <td class="tg-c3ow">Partially</td>
    <td class="tg-c3ow">Yes</td>
    <td class="tg-c3ow">Yes</td>
    <td class="tg-c3ow">Yes</td>
    <td class="tg-c3ow">Yes</td>
    <td class="tg-c3ow">Yes</td>
    <td class="tg-c3ow">No</td>
  </tr>
</tbody>
</table></center>

Azure App Service and GCP App Engine are very similar and aims at being easier to deploy simple applications. Since there is no support for docker-compose in GCP App Engine, I did not consider this option. Hosting on [Azure App Service](https://mtgscan.azurewebsites.net) was significantly easier with the already configured environnement (docker, DNS, SSL...). However 1 GB of RAM was not enough when more than 2 requests occur simultaneously. The B1 plan was a minimum. On the plus side: the integration between Azure and Visual Code.

I also tried to [host my app on a virtual machine](http://mtgscan.net), offering more liberty. [GCP VMs](https://cloud.google.com/compute/vm-instance-pricing) seemed a bit more attracting to me than
[Azure VMs](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/linux). I had no problem connecting to the GCP VM via Visual Code remote SSH.

Moreover [Azure](https://azure.microsoft.com/en-us/pricing/free-services) and [GCP](https://cloud.google.com/free) offer free trial and free tiers, although GCP is a bit more generous:

<table class="tg">
<thead>
  <tr>
    <th class="tg-c3ow"></th>
    <th class="tg-v0hj">At account creation</th>
    <th class="tg-v0hj">Free tier VM </th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-v0hj">Azure</td>
    <td class="tg-c3ow">$200 the first month</td>
    <td class="tg-c3ow">B1s (for 1 year)</td>
  </tr>
  <tr>
    <td class="tg-v0hj">GCP</td>
    <td class="tg-c3ow">$300 the first 3 months</td>
    <td class="tg-c3ow"><span style="font-weight:500;font-style:normal">e2-micro</span><br>(forever)</td>
  </tr>
</tbody>
</table>

The free tier can also be used to partially cover the cost of more expensive services.

# Perspectives

- Make a Twitter bot scanning cards (by using [http://mtgscan.net](http://mtgscan.net) through a REST API), when mentioned.  
- Adapt the app for other card games.
