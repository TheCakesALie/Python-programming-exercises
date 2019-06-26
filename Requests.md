# Python3 Requests Module Examples
This file contains examples for different actions using the Requests III module. All examples are Python3.

## https://3.python-requests.org/user/quickstart

### Download Images
```python3
r = requests.get('https://imgs.xkcd.com/comics/python.png')

with open('img.jpg', 'wb') as f:
  for chunk in r.iter_content(chunk_size=128):
    f.write(chunk)
```

### Save Responses in JSON Dictionary
```python3
payload = {'username': 'corey', 'password': 'testing'}
r = requests.post('https://httpbin.org/post', data=payload)

r_dict = r.json() #Saves JSON response as a dictionary

print(r_dict['form']) #Retrieve particular value
```

### Timeout in Seconds - Should be used for production code
```python3
requests.get('https://github.com/', timeout=0.001)
```

### Errors and Exceptions

In the event of a network problem (e.g. DNS failure, refused connection, etc), Requests will raise a **```ConnectionError```** exception.
**```Response.raise_for_status()```** will raise an **```HTTPError```** if the HTTP request returned an unsuccessful status code.
If a request times out, a **```Timeout```** exception is raised.
If a request exceeds the configured number of maximum redirections, a **```TooManyRedirects```** exception is raised.

### Web Scraper with Request and BeautifulSoup4
```python3
from bs4 import BeautifulSoup
import requests
import csv

source = requests.get('http://coreyms.com').text

soup = BeautifulSoup(source, 'lxml')

csv_file = open('cms_scrape.csv', 'w')

csv_writer = csv.writer(csv_file)
csv_writer.writerow(['headline', 'summary', 'video_link'])

for article in soup.find_all('article'):
    headline = article.h2.a.text
    print(headline)

    summary = article.find('div', class_='entry-content').p.text
    print(summary)

    try:
        vid_src = article.find('iframe', class_='youtube-player')['src']

        vid_id = vid_src.split('/')[4]
        vid_id = vid_id.split('?')[0]

        yt_link = f'https://youtube.com/watch?v={vid_id}'
    except Exception as e:
        yt_link = None

    print(yt_link)

    print()

    csv_writer.writerow([headline, summary, yt_link])

csv_file.close()
```
