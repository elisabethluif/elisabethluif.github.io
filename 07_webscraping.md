# Homework 7: Webscraping

I have approached the problem using Python, the <a href="https://pypi.org/project/wget/">Wget library</a> and the <a href="https://www.crummy.com/software/BeautifulSoup/bs4/doc/">Beautiful Soup library</a>.

> download issues of “Richmond Times Dispatch” (Years 1860-1865, only!)

First we get all the links from the page:

<code>downloadBaseUrl ='http://www.perseus.tufts.edu/hopper/dltext?'
response = requests.get(baseUrl)
soup = BeautifulSoup(response.text, "html.parser")

#get all links on site and store in list
results= soup.findAll('a')
</code>

Next we filter the results by date

<code>for result in results:
    #filter by year
    if "1860" or "1861" or "1862" or "1863" or "1864" or "1865" in str(result):
        #only if it links to a document
        if("doc=" in str(result.get('href'))):
            URLList.append(result.get('href'))
            rawLinkList.append(result)</code>

We get the URLs from the links and store it in a list (URLList).

<code># SAVE FILE

def downloadFile(URLreceived):
    print("Downloading URL ", URL, "\n" )
    wget.download(URL, out=saveFolder)


fullURLList =[]
#extract URLS
maxDownloads = 100000;
for URL in URLList:
    maxDownloads -= 1
    if maxDownloads < 0:
        break


   indexOfQuestionmark = URL.index("?") +1
   URL = URL[indexOfQuestionmark:]
   #print("Cutted URL: ",URL)
   URL = downloadBaseUrl + URL
    
   fullURLList.append(URL)
   downloadFile(URL)</code>
 
    
Lastly, we cut the URLs so we only get the part after the questionmark, and download the file through a custom function.
    

_Back to [start](https://elisabethluif.github.io/)_
