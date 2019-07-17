

## Data analysis and visualization of knowledge graph for star war movies

👉👉[**You can have a look at this Project first**](http://starwar-visualization.s3-website-us-west-1.amazonaws.com) 👈👈


This project collected data from online database [**SWAPI**](https://swapi.co), which is the world's first quantified and programmatically-accessible data source for all the data from the Star Wars canon universe!

The dataset include 6 APIs: Planets, Spaceships, Vehicles, People, Films and Species, from all SEVEN Star Wars films.

### 1. Data collection

We can get the json file of all data from this website, then use urllib in python3 to download and save data. 



```python
import warnings
warnings.simplefilter('ignore')

import urllib
import json
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib
%matplotlib inline
matplotlib.style.use('ggplot')
```


```python
films = []
for x in range(1,8):
    films.append('httP://swapi.co/api/films/' + str(x) + '/')

headers = {}
headers["User-Agent"] = "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.104 Safari/537.36 Core/1.53.3226.400 QQBrowser/9.6.11681.400"

fw = open('../csv/films.txt', 'w')
for item in films:
    print(item)
    request = urllib.request.Request(item, headers=headers)
    response = urllib.request.urlopen(request, timeout=20)
    result = response.read().decode('utf-8')
    print(result)
    fw.write(result + '\n')

fw.close()
```

    httP://swapi.co/api/films/1/
    {"title":"A New Hope","episode_id":4,"opening_crawl":"It is a period of civil war.\r\nRebel spaceships, striking\r\nfrom a hidden base, have won\r\ntheir first victory against\r\nthe evil Galactic Empire.\r\n\r\nDuring the battle, Rebel\r\nspies managed to steal secret\r\nplans to the Empire's\r\nultimate weapon, the DEATH\r\nSTAR, an armored space\r\nstation with enough power\r\nto destroy an entire planet.\r\n\r\nPursued by the Empire's\r\nsinister agents, Princess\r\nLeia races home aboard her\r\nstarship, custodian of the\r\nstolen plans that can save her\r\npeople and restore\r\nfreedom to the galaxy....","director":"George Lucas","producer":"Gary Kurtz, Rick McCallum","release_date":"1977-05-25","characters":["https://swapi.co/api/people/1/","https://swapi.co/api/people/2/","https://swapi.co/api/people/3/","https://swapi.co/api/people/4/","https://swapi.co/api/people/5/","https://swapi.co/api/people/6/","https://swapi.co/api/people/7/","https://swapi.co/api/people/8/","https://swapi.co/api/people/9/","https://swapi.co/api/people/10/","https://swapi.co/api/people/12/","https://swapi.co/api/people/13/","https://swapi.co/api/people/14/","https://swapi.co/api/people/15/","https://swapi.co/api/people/16/","https://swapi.co/api/people/18/","https://swapi.co/api/people/19/","https://swapi.co/api/people/81/"],"planets":["https://swapi.co/api/planets/2/","https://swapi.co/api/planets/3/","https://swapi.co/api/planets/1/"],"starships":["https://swapi.co/api/starships/2/","https://swapi.co/api/starships/3/","https://swapi.co/api/starships/5/","https://swapi.co/api/starships/9/","https://swapi.co/api/starships/10/","https://swapi.co/api/starships/11/","https://swapi.co/api/starships/12/","https://swapi.co/api/starships/13/"],"vehicles":["https://swapi.co/api/vehicles/4/","https://swapi.co/api/vehicles/6/","https://swapi.co/api/vehicles/7/","https://swapi.co/api/vehicles/8/"],"species":["https://swapi.co/api/species/5/","https://swapi.co/api/species/3/","https://swapi.co/api/species/2/","https://swapi.co/api/species/1/","https://swapi.co/api/species/4/"],"created":"2014-12-10T14:23:31.880000Z","edited":"2015-04-11T09:46:52.774897Z","url":"https://swapi.co/api/films/1/"}
    httP://swapi.co/api/films/2/
    {"title":"The Empire Strikes Back","episode_id":5,"opening_crawl":"It is a dark time for the\r\nRebellion. Although the Death\r\nStar has been destroyed,\r\nImperial troops have driven the\r\nRebel forces from their hidden\r\nbase and pursued them across\r\nthe galaxy.\r\n\r\nEvading the dreaded Imperial\r\nStarfleet, a group of freedom\r\nfighters led by Luke Skywalker\r\nhas established a new secret\r\nbase on the remote ice world\r\nof Hoth.\r\n\r\nThe evil lord Darth Vader,\r\nobsessed with finding young\r\nSkywalker, has dispatched\r\nthousands of remote probes into\r\nthe far reaches of space....","director":"Irvin Kershner","producer":"Gary Kurtz, Rick McCallum","release_date":"1980-05-17","characters":["https://swapi.co/api/people/1/","https://swapi.co/api/people/2/","https://swapi.co/api/people/3/","https://swapi.co/api/people/4/","https://swapi.co/api/people/5/","https://swapi.co/api/people/10/","https://swapi.co/api/people/13/","https://swapi.co/api/people/14/","https://swapi.co/api/people/18/","https://swapi.co/api/people/20/","https://swapi.co/api/people/21/","https://swapi.co/api/people/22/","https://swapi.co/api/people/23/","https://swapi.co/api/people/24/","https://swapi.co/api/people/25/","https://swapi.co/api/people/26/"],"planets":["https://swapi.co/api/planets/4/","https://swapi.co/api/planets/5/","https://swapi.co/api/planets/6/","https://swapi.co/api/planets/27/"],"starships":["https://swapi.co/api/starships/15/","https://swapi.co/api/starships/10/","https://swapi.co/api/starships/11/","https://swapi.co/api/starships/12/","https://swapi.co/api/starships/21/","https://swapi.co/api/starships/22/","https://swapi.co/api/starships/23/","https://swapi.co/api/starships/3/","https://swapi.co/api/starships/17/"],"vehicles":["https://swapi.co/api/vehicles/8/","https://swapi.co/api/vehicles/14/","https://swapi.co/api/vehicles/16/","https://swapi.co/api/vehicles/18/","https://swapi.co/api/vehicles/19/","https://swapi.co/api/vehicles/20/"],"species":["https://swapi.co/api/species/6/","https://swapi.co/api/species/7/","https://swapi.co/api/species/3/","https://swapi.co/api/species/2/","https://swapi.co/api/species/1/"],"created":"2014-12-12T11:26:24.656000Z","edited":"2017-04-19T10:57:29.544256Z","url":"https://swapi.co/api/films/2/"}
    httP://swapi.co/api/films/3/
    {"title":"Return of the Jedi","episode_id":6,"opening_crawl":"Luke Skywalker has returned to\r\nhis home planet of Tatooine in\r\nan attempt to rescue his\r\nfriend Han Solo from the\r\nclutches of the vile gangster\r\nJabba the Hutt.\r\n\r\nLittle does Luke know that the\r\nGALACTIC EMPIRE has secretly\r\nbegun construction on a new\r\narmored space station even\r\nmore powerful than the first\r\ndreaded Death Star.\r\n\r\nWhen completed, this ultimate\r\nweapon will spell certain doom\r\nfor the small band of rebels\r\nstruggling to restore freedom\r\nto the galaxy...","director":"Richard Marquand","producer":"Howard G. Kazanjian, George Lucas, Rick McCallum","release_date":"1983-05-25","characters":["https://swapi.co/api/people/1/","https://swapi.co/api/people/2/","https://swapi.co/api/people/3/","https://swapi.co/api/people/4/","https://swapi.co/api/people/5/","https://swapi.co/api/people/10/","https://swapi.co/api/people/13/","https://swapi.co/api/people/14/","https://swapi.co/api/people/16/","https://swapi.co/api/people/18/","https://swapi.co/api/people/20/","https://swapi.co/api/people/21/","https://swapi.co/api/people/22/","https://swapi.co/api/people/25/","https://swapi.co/api/people/27/","https://swapi.co/api/people/28/","https://swapi.co/api/people/29/","https://swapi.co/api/people/30/","https://swapi.co/api/people/31/","https://swapi.co/api/people/45/"],"planets":["https://swapi.co/api/planets/5/","https://swapi.co/api/planets/7/","https://swapi.co/api/planets/8/","https://swapi.co/api/planets/9/","https://swapi.co/api/planets/1/"],"starships":["https://swapi.co/api/starships/15/","https://swapi.co/api/starships/10/","https://swapi.co/api/starships/11/","https://swapi.co/api/starships/12/","https://swapi.co/api/starships/22/","https://swapi.co/api/starships/23/","https://swapi.co/api/starships/27/","https://swapi.co/api/starships/28/","https://swapi.co/api/starships/29/","https://swapi.co/api/starships/3/","https://swapi.co/api/starships/17/","https://swapi.co/api/starships/2/"],"vehicles":["https://swapi.co/api/vehicles/8/","https://swapi.co/api/vehicles/16/","https://swapi.co/api/vehicles/18/","https://swapi.co/api/vehicles/19/","https://swapi.co/api/vehicles/24/","https://swapi.co/api/vehicles/25/","https://swapi.co/api/vehicles/26/","https://swapi.co/api/vehicles/30/"],"species":["https://swapi.co/api/species/1/","https://swapi.co/api/species/2/","https://swapi.co/api/species/3/","https://swapi.co/api/species/5/","https://swapi.co/api/species/6/","https://swapi.co/api/species/8/","https://swapi.co/api/species/9/","https://swapi.co/api/species/10/","https://swapi.co/api/species/15/"],"created":"2014-12-18T10:39:33.255000Z","edited":"2015-04-11T09:46:05.220365Z","url":"https://swapi.co/api/films/3/"}
    httP://swapi.co/api/films/4/
    {"title":"The Phantom Menace","episode_id":1,"opening_crawl":"Turmoil has engulfed the\r\nGalactic Republic. The taxation\r\nof trade routes to outlying star\r\nsystems is in dispute.\r\n\r\nHoping to resolve the matter\r\nwith a blockade of deadly\r\nbattleships, the greedy Trade\r\nFederation has stopped all\r\nshipping to the small planet\r\nof Naboo.\r\n\r\nWhile the Congress of the\r\nRepublic endlessly debates\r\nthis alarming chain of events,\r\nthe Supreme Chancellor has\r\nsecretly dispatched two Jedi\r\nKnights, the guardians of\r\npeace and justice in the\r\ngalaxy, to settle the conflict....","director":"George Lucas","producer":"Rick McCallum","release_date":"1999-05-19","characters":["https://swapi.co/api/people/2/","https://swapi.co/api/people/3/","https://swapi.co/api/people/10/","https://swapi.co/api/people/11/","https://swapi.co/api/people/16/","https://swapi.co/api/people/20/","https://swapi.co/api/people/21/","https://swapi.co/api/people/32/","https://swapi.co/api/people/33/","https://swapi.co/api/people/34/","https://swapi.co/api/people/36/","https://swapi.co/api/people/37/","https://swapi.co/api/people/38/","https://swapi.co/api/people/39/","https://swapi.co/api/people/40/","https://swapi.co/api/people/41/","https://swapi.co/api/people/42/","https://swapi.co/api/people/43/","https://swapi.co/api/people/44/","https://swapi.co/api/people/46/","https://swapi.co/api/people/48/","https://swapi.co/api/people/49/","https://swapi.co/api/people/50/","https://swapi.co/api/people/51/","https://swapi.co/api/people/52/","https://swapi.co/api/people/53/","https://swapi.co/api/people/54/","https://swapi.co/api/people/55/","https://swapi.co/api/people/56/","https://swapi.co/api/people/57/","https://swapi.co/api/people/58/","https://swapi.co/api/people/59/","https://swapi.co/api/people/47/","https://swapi.co/api/people/35/"],"planets":["https://swapi.co/api/planets/8/","https://swapi.co/api/planets/9/","https://swapi.co/api/planets/1/"],"starships":["https://swapi.co/api/starships/40/","https://swapi.co/api/starships/41/","https://swapi.co/api/starships/31/","https://swapi.co/api/starships/32/","https://swapi.co/api/starships/39/"],"vehicles":["https://swapi.co/api/vehicles/33/","https://swapi.co/api/vehicles/34/","https://swapi.co/api/vehicles/35/","https://swapi.co/api/vehicles/36/","https://swapi.co/api/vehicles/37/","https://swapi.co/api/vehicles/38/","https://swapi.co/api/vehicles/42/"],"species":["https://swapi.co/api/species/1/","https://swapi.co/api/species/2/","https://swapi.co/api/species/6/","https://swapi.co/api/species/11/","https://swapi.co/api/species/12/","https://swapi.co/api/species/13/","https://swapi.co/api/species/14/","https://swapi.co/api/species/15/","https://swapi.co/api/species/16/","https://swapi.co/api/species/17/","https://swapi.co/api/species/18/","https://swapi.co/api/species/19/","https://swapi.co/api/species/20/","https://swapi.co/api/species/21/","https://swapi.co/api/species/22/","https://swapi.co/api/species/23/","https://swapi.co/api/species/24/","https://swapi.co/api/species/25/","https://swapi.co/api/species/26/","https://swapi.co/api/species/27/"],"created":"2014-12-19T16:52:55.740000Z","edited":"2015-04-11T09:45:18.689301Z","url":"https://swapi.co/api/films/4/"}
    httP://swapi.co/api/films/5/
    {"title":"Attack of the Clones","episode_id":2,"opening_crawl":"There is unrest in the Galactic\r\nSenate. Several thousand solar\r\nsystems have declared their\r\nintentions to leave the Republic.\r\n\r\nThis separatist movement,\r\nunder the leadership of the\r\nmysterious Count Dooku, has\r\nmade it difficult for the limited\r\nnumber of Jedi Knights to maintain \r\npeace and order in the galaxy.\r\n\r\nSenator Amidala, the former\r\nQueen of Naboo, is returning\r\nto the Galactic Senate to vote\r\non the critical issue of creating\r\nan ARMY OF THE REPUBLIC\r\nto assist the overwhelmed\r\nJedi....","director":"George Lucas","producer":"Rick McCallum","release_date":"2002-05-16","characters":["https://swapi.co/api/people/2/","https://swapi.co/api/people/3/","https://swapi.co/api/people/6/","https://swapi.co/api/people/7/","https://swapi.co/api/people/10/","https://swapi.co/api/people/11/","https://swapi.co/api/people/20/","https://swapi.co/api/people/21/","https://swapi.co/api/people/22/","https://swapi.co/api/people/33/","https://swapi.co/api/people/36/","https://swapi.co/api/people/40/","https://swapi.co/api/people/43/","https://swapi.co/api/people/46/","https://swapi.co/api/people/51/","https://swapi.co/api/people/52/","https://swapi.co/api/people/53/","https://swapi.co/api/people/58/","https://swapi.co/api/people/59/","https://swapi.co/api/people/60/","https://swapi.co/api/people/61/","https://swapi.co/api/people/62/","https://swapi.co/api/people/63/","https://swapi.co/api/people/64/","https://swapi.co/api/people/65/","https://swapi.co/api/people/66/","https://swapi.co/api/people/67/","https://swapi.co/api/people/68/","https://swapi.co/api/people/69/","https://swapi.co/api/people/70/","https://swapi.co/api/people/71/","https://swapi.co/api/people/72/","https://swapi.co/api/people/73/","https://swapi.co/api/people/74/","https://swapi.co/api/people/75/","https://swapi.co/api/people/76/","https://swapi.co/api/people/77/","https://swapi.co/api/people/78/","https://swapi.co/api/people/82/","https://swapi.co/api/people/35/"],"planets":["https://swapi.co/api/planets/8/","https://swapi.co/api/planets/9/","https://swapi.co/api/planets/10/","https://swapi.co/api/planets/11/","https://swapi.co/api/planets/1/"],"starships":["https://swapi.co/api/starships/21/","https://swapi.co/api/starships/39/","https://swapi.co/api/starships/43/","https://swapi.co/api/starships/47/","https://swapi.co/api/starships/48/","https://swapi.co/api/starships/49/","https://swapi.co/api/starships/32/","https://swapi.co/api/starships/52/","https://swapi.co/api/starships/58/"],"vehicles":["https://swapi.co/api/vehicles/4/","https://swapi.co/api/vehicles/44/","https://swapi.co/api/vehicles/45/","https://swapi.co/api/vehicles/46/","https://swapi.co/api/vehicles/50/","https://swapi.co/api/vehicles/51/","https://swapi.co/api/vehicles/53/","https://swapi.co/api/vehicles/54/","https://swapi.co/api/vehicles/55/","https://swapi.co/api/vehicles/56/","https://swapi.co/api/vehicles/57/"],"species":["https://swapi.co/api/species/32/","https://swapi.co/api/species/33/","https://swapi.co/api/species/2/","https://swapi.co/api/species/35/","https://swapi.co/api/species/6/","https://swapi.co/api/species/1/","https://swapi.co/api/species/12/","https://swapi.co/api/species/34/","https://swapi.co/api/species/13/","https://swapi.co/api/species/15/","https://swapi.co/api/species/28/","https://swapi.co/api/species/29/","https://swapi.co/api/species/30/","https://swapi.co/api/species/31/"],"created":"2014-12-20T10:57:57.886000Z","edited":"2015-04-11T09:45:01.623982Z","url":"https://swapi.co/api/films/5/"}
    httP://swapi.co/api/films/6/
    {"title":"Revenge of the Sith","episode_id":3,"opening_crawl":"War! The Republic is crumbling\r\nunder attacks by the ruthless\r\nSith Lord, Count Dooku.\r\nThere are heroes on both sides.\r\nEvil is everywhere.\r\n\r\nIn a stunning move, the\r\nfiendish droid leader, General\r\nGrievous, has swept into the\r\nRepublic capital and kidnapped\r\nChancellor Palpatine, leader of\r\nthe Galactic Senate.\r\n\r\nAs the Separatist Droid Army\r\nattempts to flee the besieged\r\ncapital with their valuable\r\nhostage, two Jedi Knights lead a\r\ndesperate mission to rescue the\r\ncaptive Chancellor....","director":"George Lucas","producer":"Rick McCallum","release_date":"2005-05-19","characters":["https://swapi.co/api/people/1/","https://swapi.co/api/people/2/","https://swapi.co/api/people/3/","https://swapi.co/api/people/4/","https://swapi.co/api/people/5/","https://swapi.co/api/people/6/","https://swapi.co/api/people/7/","https://swapi.co/api/people/10/","https://swapi.co/api/people/11/","https://swapi.co/api/people/12/","https://swapi.co/api/people/13/","https://swapi.co/api/people/20/","https://swapi.co/api/people/21/","https://swapi.co/api/people/33/","https://swapi.co/api/people/46/","https://swapi.co/api/people/51/","https://swapi.co/api/people/52/","https://swapi.co/api/people/53/","https://swapi.co/api/people/54/","https://swapi.co/api/people/55/","https://swapi.co/api/people/56/","https://swapi.co/api/people/58/","https://swapi.co/api/people/63/","https://swapi.co/api/people/64/","https://swapi.co/api/people/67/","https://swapi.co/api/people/68/","https://swapi.co/api/people/75/","https://swapi.co/api/people/78/","https://swapi.co/api/people/79/","https://swapi.co/api/people/80/","https://swapi.co/api/people/81/","https://swapi.co/api/people/82/","https://swapi.co/api/people/83/","https://swapi.co/api/people/35/"],"planets":["https://swapi.co/api/planets/2/","https://swapi.co/api/planets/5/","https://swapi.co/api/planets/8/","https://swapi.co/api/planets/9/","https://swapi.co/api/planets/12/","https://swapi.co/api/planets/13/","https://swapi.co/api/planets/14/","https://swapi.co/api/planets/15/","https://swapi.co/api/planets/16/","https://swapi.co/api/planets/17/","https://swapi.co/api/planets/18/","https://swapi.co/api/planets/19/","https://swapi.co/api/planets/1/"],"starships":["https://swapi.co/api/starships/48/","https://swapi.co/api/starships/59/","https://swapi.co/api/starships/61/","https://swapi.co/api/starships/32/","https://swapi.co/api/starships/63/","https://swapi.co/api/starships/64/","https://swapi.co/api/starships/65/","https://swapi.co/api/starships/66/","https://swapi.co/api/starships/74/","https://swapi.co/api/starships/75/","https://swapi.co/api/starships/2/","https://swapi.co/api/starships/68/"],"vehicles":["https://swapi.co/api/vehicles/33/","https://swapi.co/api/vehicles/50/","https://swapi.co/api/vehicles/53/","https://swapi.co/api/vehicles/56/","https://swapi.co/api/vehicles/60/","https://swapi.co/api/vehicles/62/","https://swapi.co/api/vehicles/67/","https://swapi.co/api/vehicles/69/","https://swapi.co/api/vehicles/70/","https://swapi.co/api/vehicles/71/","https://swapi.co/api/vehicles/72/","https://swapi.co/api/vehicles/73/","https://swapi.co/api/vehicles/76/"],"species":["https://swapi.co/api/species/19/","https://swapi.co/api/species/33/","https://swapi.co/api/species/2/","https://swapi.co/api/species/3/","https://swapi.co/api/species/36/","https://swapi.co/api/species/37/","https://swapi.co/api/species/6/","https://swapi.co/api/species/1/","https://swapi.co/api/species/34/","https://swapi.co/api/species/15/","https://swapi.co/api/species/35/","https://swapi.co/api/species/20/","https://swapi.co/api/species/23/","https://swapi.co/api/species/24/","https://swapi.co/api/species/25/","https://swapi.co/api/species/26/","https://swapi.co/api/species/27/","https://swapi.co/api/species/28/","https://swapi.co/api/species/29/","https://swapi.co/api/species/30/"],"created":"2014-12-20T18:49:38.403000Z","edited":"2015-04-11T09:45:44.862122Z","url":"https://swapi.co/api/films/6/"}
    httP://swapi.co/api/films/7/
    {"title":"The Force Awakens","episode_id":7,"opening_crawl":"Luke Skywalker has vanished.\r\nIn his absence, the sinister\r\nFIRST ORDER has risen from\r\nthe ashes of the Empire\r\nand will not rest until\r\nSkywalker, the last Jedi,\r\nhas been destroyed.\r\n \r\nWith the support of the\r\nREPUBLIC, General Leia Organa\r\nleads a brave RESISTANCE.\r\nShe is desperate to find her\r\nbrother Luke and gain his\r\nhelp in restoring peace and\r\njustice to the galaxy.\r\n \r\nLeia has sent her most daring\r\npilot on a secret mission\r\nto Jakku, where an old ally\r\nhas discovered a clue to\r\nLuke's whereabouts....","director":"J. J. Abrams","producer":"Kathleen Kennedy, J. J. Abrams, Bryan Burk","release_date":"2015-12-11","characters":["https://swapi.co/api/people/1/","https://swapi.co/api/people/3/","https://swapi.co/api/people/5/","https://swapi.co/api/people/13/","https://swapi.co/api/people/14/","https://swapi.co/api/people/27/","https://swapi.co/api/people/84/","https://swapi.co/api/people/85/","https://swapi.co/api/people/86/","https://swapi.co/api/people/87/","https://swapi.co/api/people/88/"],"planets":["https://swapi.co/api/planets/61/"],"starships":["https://swapi.co/api/starships/77/","https://swapi.co/api/starships/10/"],"vehicles":[],"species":["https://swapi.co/api/species/3/","https://swapi.co/api/species/2/","https://swapi.co/api/species/1/"],"created":"2015-04-17T06:51:30.504780Z","edited":"2015-12-17T14:31:47.617768Z","url":"https://swapi.co/api/films/7/"}



```python
fr = open('../csv/films.txt', 'r')
films = []
for line in fr:
    line = json.loads(line.strip('\n'))
    films.append(line)
fr.close()

# 获取 characters,planets,starships,vehicles,species
targets = ['characters', 'planets', 'starships', 'vehicles', 'species']
for target in targets:
    fw = open('../csv/' + target + '.txt', 'w')
    data = []
    for item in films:
        tmp = item[target]
        for t in tmp:
            if t in data:
                continue
            else:
                data.append(t)
                
            while 1:
                print(t)
                try:
                    request = urllib.request.Request(t, headers=headers)
                    response = urllib.request.urlopen(request, timeout=20)
                    result = response.read().decode('utf-8')
                except Exception as e:
                    continue
                else:
                    fw.write(result + '\n')
                    break
                finally:
                    pass

    print (str(len(data)), target)
    fw.close()
```

    https://swapi.co/api/people/1/
    https://swapi.co/api/people/2/
    https://swapi.co/api/people/3/
    https://swapi.co/api/people/4/
    https://swapi.co/api/people/5/
    https://swapi.co/api/people/6/
    https://swapi.co/api/people/7/
    https://swapi.co/api/people/8/
    https://swapi.co/api/people/9/
    https://swapi.co/api/people/10/
    https://swapi.co/api/people/12/
    https://swapi.co/api/people/13/
    https://swapi.co/api/people/14/
    https://swapi.co/api/people/15/
    https://swapi.co/api/people/16/
    https://swapi.co/api/people/18/
    https://swapi.co/api/people/19/
    https://swapi.co/api/people/81/
    https://swapi.co/api/people/20/
    https://swapi.co/api/people/21/
    https://swapi.co/api/people/22/
    https://swapi.co/api/people/23/
    https://swapi.co/api/people/24/
    https://swapi.co/api/people/25/
    https://swapi.co/api/people/26/
    https://swapi.co/api/people/27/
    https://swapi.co/api/people/28/
    https://swapi.co/api/people/29/
    https://swapi.co/api/people/30/
    https://swapi.co/api/people/31/
    https://swapi.co/api/people/45/
    https://swapi.co/api/people/11/
    https://swapi.co/api/people/32/
    https://swapi.co/api/people/33/
    https://swapi.co/api/people/34/
    https://swapi.co/api/people/36/
    https://swapi.co/api/people/37/
    https://swapi.co/api/people/38/
    https://swapi.co/api/people/39/
    https://swapi.co/api/people/40/
    https://swapi.co/api/people/41/
    https://swapi.co/api/people/42/
    https://swapi.co/api/people/43/
    https://swapi.co/api/people/44/
    https://swapi.co/api/people/46/
    https://swapi.co/api/people/48/
    https://swapi.co/api/people/49/
    https://swapi.co/api/people/50/
    https://swapi.co/api/people/51/
    https://swapi.co/api/people/52/
    https://swapi.co/api/people/53/
    https://swapi.co/api/people/54/
    https://swapi.co/api/people/55/
    https://swapi.co/api/people/56/
    https://swapi.co/api/people/57/
    https://swapi.co/api/people/58/
    https://swapi.co/api/people/59/
    https://swapi.co/api/people/47/
    https://swapi.co/api/people/35/
    https://swapi.co/api/people/60/
    https://swapi.co/api/people/61/
    https://swapi.co/api/people/62/
    https://swapi.co/api/people/63/
    https://swapi.co/api/people/64/
    https://swapi.co/api/people/65/
    https://swapi.co/api/people/66/
    https://swapi.co/api/people/67/
    https://swapi.co/api/people/68/
    https://swapi.co/api/people/69/
    https://swapi.co/api/people/70/
    https://swapi.co/api/people/71/
    https://swapi.co/api/people/72/
    https://swapi.co/api/people/73/
    https://swapi.co/api/people/74/
    https://swapi.co/api/people/75/
    https://swapi.co/api/people/76/
    https://swapi.co/api/people/77/
    https://swapi.co/api/people/78/
    https://swapi.co/api/people/82/
    https://swapi.co/api/people/79/
    https://swapi.co/api/people/80/
    https://swapi.co/api/people/83/
    https://swapi.co/api/people/84/
    https://swapi.co/api/people/85/
    https://swapi.co/api/people/86/
    https://swapi.co/api/people/87/
    https://swapi.co/api/people/88/
    87 characters
    https://swapi.co/api/planets/2/
    https://swapi.co/api/planets/3/
    https://swapi.co/api/planets/1/
    https://swapi.co/api/planets/4/
    https://swapi.co/api/planets/5/
    https://swapi.co/api/planets/6/
    https://swapi.co/api/planets/27/
    https://swapi.co/api/planets/7/
    https://swapi.co/api/planets/8/
    https://swapi.co/api/planets/9/
    https://swapi.co/api/planets/10/
    https://swapi.co/api/planets/11/
    https://swapi.co/api/planets/12/
    https://swapi.co/api/planets/13/
    https://swapi.co/api/planets/14/
    https://swapi.co/api/planets/15/
    https://swapi.co/api/planets/16/
    https://swapi.co/api/planets/17/
    https://swapi.co/api/planets/18/
    https://swapi.co/api/planets/19/
    https://swapi.co/api/planets/61/
    21 planets
    https://swapi.co/api/starships/2/
    https://swapi.co/api/starships/3/
    https://swapi.co/api/starships/5/
    https://swapi.co/api/starships/9/
    https://swapi.co/api/starships/10/
    https://swapi.co/api/starships/11/
    https://swapi.co/api/starships/12/
    https://swapi.co/api/starships/13/
    https://swapi.co/api/starships/15/
    https://swapi.co/api/starships/21/
    https://swapi.co/api/starships/22/
    https://swapi.co/api/starships/23/
    https://swapi.co/api/starships/17/
    https://swapi.co/api/starships/27/
    https://swapi.co/api/starships/28/
    https://swapi.co/api/starships/29/
    https://swapi.co/api/starships/40/
    https://swapi.co/api/starships/41/
    https://swapi.co/api/starships/31/
    https://swapi.co/api/starships/32/
    https://swapi.co/api/starships/39/
    https://swapi.co/api/starships/43/
    https://swapi.co/api/starships/47/
    https://swapi.co/api/starships/48/
    https://swapi.co/api/starships/49/
    https://swapi.co/api/starships/52/
    https://swapi.co/api/starships/58/
    https://swapi.co/api/starships/59/
    https://swapi.co/api/starships/61/
    https://swapi.co/api/starships/63/
    https://swapi.co/api/starships/64/
    https://swapi.co/api/starships/65/
    https://swapi.co/api/starships/66/
    https://swapi.co/api/starships/74/
    https://swapi.co/api/starships/75/
    https://swapi.co/api/starships/68/
    https://swapi.co/api/starships/77/
    37 starships
    https://swapi.co/api/vehicles/4/
    https://swapi.co/api/vehicles/6/
    https://swapi.co/api/vehicles/7/
    https://swapi.co/api/vehicles/8/
    https://swapi.co/api/vehicles/14/
    https://swapi.co/api/vehicles/16/
    https://swapi.co/api/vehicles/18/
    https://swapi.co/api/vehicles/19/
    https://swapi.co/api/vehicles/20/
    https://swapi.co/api/vehicles/24/
    https://swapi.co/api/vehicles/25/
    https://swapi.co/api/vehicles/26/
    https://swapi.co/api/vehicles/30/
    https://swapi.co/api/vehicles/33/
    https://swapi.co/api/vehicles/34/
    https://swapi.co/api/vehicles/35/
    https://swapi.co/api/vehicles/36/
    https://swapi.co/api/vehicles/37/
    https://swapi.co/api/vehicles/38/
    https://swapi.co/api/vehicles/42/
    https://swapi.co/api/vehicles/44/
    https://swapi.co/api/vehicles/45/
    https://swapi.co/api/vehicles/46/
    https://swapi.co/api/vehicles/50/
    https://swapi.co/api/vehicles/51/
    https://swapi.co/api/vehicles/53/
    https://swapi.co/api/vehicles/54/
    https://swapi.co/api/vehicles/55/
    https://swapi.co/api/vehicles/56/
    https://swapi.co/api/vehicles/57/
    https://swapi.co/api/vehicles/60/
    https://swapi.co/api/vehicles/62/
    https://swapi.co/api/vehicles/67/
    https://swapi.co/api/vehicles/69/
    https://swapi.co/api/vehicles/70/
    https://swapi.co/api/vehicles/71/
    https://swapi.co/api/vehicles/72/
    https://swapi.co/api/vehicles/73/
    https://swapi.co/api/vehicles/76/
    39 vehicles
    https://swapi.co/api/species/5/
    https://swapi.co/api/species/3/
    https://swapi.co/api/species/2/
    https://swapi.co/api/species/1/
    https://swapi.co/api/species/4/
    https://swapi.co/api/species/6/
    https://swapi.co/api/species/7/
    https://swapi.co/api/species/8/
    https://swapi.co/api/species/9/
    https://swapi.co/api/species/10/
    https://swapi.co/api/species/15/
    https://swapi.co/api/species/11/
    https://swapi.co/api/species/12/
    https://swapi.co/api/species/13/
    https://swapi.co/api/species/14/
    https://swapi.co/api/species/16/
    https://swapi.co/api/species/17/
    https://swapi.co/api/species/18/
    https://swapi.co/api/species/19/
    https://swapi.co/api/species/20/
    https://swapi.co/api/species/21/
    https://swapi.co/api/species/22/
    https://swapi.co/api/species/23/
    https://swapi.co/api/species/24/
    https://swapi.co/api/species/25/
    https://swapi.co/api/species/26/
    https://swapi.co/api/species/27/
    https://swapi.co/api/species/32/
    https://swapi.co/api/species/33/
    https://swapi.co/api/species/35/
    https://swapi.co/api/species/34/
    https://swapi.co/api/species/28/
    https://swapi.co/api/species/29/
    https://swapi.co/api/species/30/
    https://swapi.co/api/species/31/
    https://swapi.co/api/species/36/
    https://swapi.co/api/species/37/
    37 species


### 2. Basic analysis


```python
fr = open('../csv/films.txt','r')
fw = open('../csv/stat_basic.csv','w')
fw.write('title,key,value\n')

for line in fr:
    tmp = json.loads(line.strip('\n'))
    fw.write(tmp['title'] + ',' + 'characters,' + str(len(tmp['characters'])) + '\n')
    fw.write(tmp['title'] + ',' + 'planets,' + str(len(tmp['planets'])) + '\n')
    fw.write(tmp['title'] + ',' + 'starships,' + str(len(tmp['starships'])) + '\n')
    fw.write(tmp['title'] + ',' + 'vehicles,' + str(len(tmp['vehicles'])) + '\n')
    fw.write(tmp['title'] + ',' + 'species,' + str(len(tmp['species'])) + '\n')

fr.close()
fw.close()

```


```python
stats = pd.read_csv('../csv/stat_basic.csv')
stats.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A New Hope</td>
      <td>characters</td>
      <td>18</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A New Hope</td>
      <td>planets</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A New Hope</td>
      <td>starships</td>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A New Hope</td>
      <td>vehicles</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A New Hope</td>
      <td>species</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Visualization of overall stats

fig, ax = plt.subplots(figsize=(12, 6))
sns.barplot(x='key', y ='value', hue='title', data=stats)
ax.set_title('Overview of all movies', fontsize=16)
plt.xlabel('')
plt.show()
```


![png](star_war_files/star_war_7_0.png)


#### "Attack of the Clones" has most characters 


```python
fr = open('../csv/characters.txt','r')
fw = open('../csv/stat_characters.csv','w')
fw.write('name,height,mass,gender,homeworld\n')
for line in fr:
    tmp = json.loads(line.strip('\n'))
    if tmp['height'] == 'unknown':
        tmp['height'] = '-1'
    if tmp['mass'] == 'unknown':
        tmp['mass'] = '-1'
    if tmp['gender'] == 'none':
        tmp['gender'] = 'n/a'
    fw.write(tmp['name'] + ',' + tmp['height'] + ',' + tmp['mass'] + ',' + tmp['gender'].strip() + ',' + tmp['homeworld'] + '\n')

fr.close()
fw.close()

```


```python
stat_characters = pd.read_csv('../csv/stat_characters.csv')
stat_characters.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>height</th>
      <th>mass</th>
      <th>gender</th>
      <th>homeworld</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Luke Skywalker</td>
      <td>172</td>
      <td>77.0</td>
      <td>male</td>
      <td>https://swapi.co/api/planets/1/</td>
    </tr>
    <tr>
      <th>1</th>
      <td>C-3PO</td>
      <td>167</td>
      <td>75.0</td>
      <td>NaN</td>
      <td>https://swapi.co/api/planets/1/</td>
    </tr>
    <tr>
      <th>2</th>
      <td>R2-D2</td>
      <td>96</td>
      <td>32.0</td>
      <td>NaN</td>
      <td>https://swapi.co/api/planets/8/</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Darth Vader</td>
      <td>202</td>
      <td>136.0</td>
      <td>male</td>
      <td>https://swapi.co/api/planets/1/</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Leia Organa</td>
      <td>150</td>
      <td>49.0</td>
      <td>female</td>
      <td>https://swapi.co/api/planets/2/</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Visualization of characters

fig, ax = plt.subplots(figsize=(12, 6))
sns.scatterplot(x='mass', y ='height', hue='gender', data=stat_characters)
ax.set_title('Visualization of characters', fontsize=16)
plt.show()
```


![png](star_war_files/star_war_11_0.png)


### 3. Build relationship data file

Now we use python to create 3 json files to build our interactive graph, we will use **html、css、javascript and d3 to finish** it.

You can open the `index.html` in this directory, the website is based on it.

### 4. Visualization

The core is D3, **Data Driven Documents**, which is data-driven documents. It is one of the most popular JS visualization libraries. The core idea of D3 is to use data to generate elements on a web page. When data is updated, the appearance and attributes of elements on a web page are modified according to the changes of data.

We need to prepare an `SVG` and `G` as drawing containers in HTML code, then `select ()` the containers in JS code, then `select all ()` the `SVG` elements to be generated, bind data for `SVG` elements using `data (), execute enter (). append ()` and `exit (). remove ()` according to the corresponding state of data and elements, and control `SVG` using `attr ()` to control svg.  Element appearance, and dynamically update the appearance of SVG elements according to user interaction

In order to achieve this graph, we generate circle and text elements based on node data; line elements based on node links; circle and text size and color are controlled according to node data; and when the mouse is hovering and dragging, the corresponding time is triggered, and the appearance and display of elements are changed. The implementation of the timeline is similar. The only difference is that rect element is used here.

