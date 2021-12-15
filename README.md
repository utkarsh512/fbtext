# fbscraper
Scraping posts, comments and replies from Facebook using selenium and beautifulsoup.
## Requirements
* `bs4`
* `selenium`
* `cssutils`
## How to use
### Command line style
fbscraper can be executed in command line as
```bash
$ python main.py -e EMAIL -p PASSWORD -x chromeDriverPath -i DonaldTrump -s 100 -d posts.pkl -g true [-c 1000] [-r 1000] [-j false]
```
For more information on the flags, run
```bash
$ python main.py --help
```
### Notebook style
First of all, create a `Session` object for scraping:
```python3
from scraper import Session
sess = Session(
    credentials=(EMAIL, 
                 PASSWORD), 
    chromeDriverPath="chromedriver"
)
```
where `(EMAIL, PASSWORD)` are your facebook credentials and `chromeDriverPath` is the path to the chromedriver.

Then, you can extract recent post URLs of a public pages as
```python3
postURLs = sess.getPostURLs(
    pageID="nytimes", 
    nScrolls=10
)
```
where `pageID` is the unique Facebook ID of the page and `nScrolls` is number of scrolls (as viewed in FB mobile app) upto which posts are to be extracted. The above function will return a list of URLs `postURLs`.

As you now have the list of URLs for the required posts, post data (including comments) can be scraped as
```python3
sess.getPost(
    postURL="https://mbasic.facebook.com/story.php?...",
    dumpAs="posts.pkl",
    nComments=1000,
    getReplies=True,
    nReplies=10
)
```
where 
* `postURL` is the URL of the post
* `dumpAs` is the name of binary file used for appending the post
* `nComments` is the upper-bound on number of comments per post
* `getReplies` should be `True` if you want to scrape replies as well
* `nReplies` is the upper-bound on number of replies per comment

Just make sure postURL starts with `https://mbasic.facebook.com` instead of `https://www.facebook.com`, `https://mobile.facebook.com`, etc.

Done with scraping! Remember to close the session as 
```python3
sess.close()
```
