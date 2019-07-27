# Homework 7: Webscraping

I have approached the problem using Python, the <a href="https://pypi.org/project/wget/">Wget library</a> and the <a href="https://www.crummy.com/software/BeautifulSoup/bs4/doc/">Beautiful Soup library</a>.

> download issues of “Richmond Times Dispatch” (Years 1860-1865, only!)

First we get all the links from the page:

baseUrl = 'http://www.perseus.tufts.edu/hopper/collection?collection=Perseus:collection:RichTimes'
downloadBaseUrl ='http://www.perseus.tufts.edu/hopper/dltext?'
saveFolder = 'C:/Users/Sissi/Desktop/Lesson7_Files/'

#get all hyperlinks on site and store in list
response = requests.get(baseUrl)
print(response)
soup = BeautifulSoup(response.text, "html.parser")
results= soup.findAll('a')
print("received all links...")

Next we filter the results by date

awLinkList = []
URLList = []

for result in results:
    #filter by year
    if "1860" or "1861" or "1862" or "1863" or "1864" or "1865" in str(result):
        #only if it links to a document
        if("doc=" in str(result.get('href'))):
            URLList.append(result.get('href'))
            rawLinkList.append(result)

#We get the URLs from the links and store it in a list (URLList).
URLList = list(dict.fromkeys(URLList))
print("found ", len(rawLinkList) , " raw links")
print("stored ", len(URLList) , " URLs")
print("example : ", URLList[0])
print("substring: ", URLList[0][5:], "\n\n")


#Define custom function to download files 
def downloadFile(URLreceived):
    print("Downloading URL ", URL, "\n" )
    args = 'wget '+ URLreceived+ ' -P '+ saveFolder + ' --quiet -nc';
    print("running args on os " , args)
    os.system(args)


#Save file#extract URLS
fullURLList =[]

#For testing, make sure we can just download a couple with this variable
maxDownloads = 100000;
for URL in URLList:
    maxDownloads -= 1
    if maxDownloads < 0:
        break

#Lastly, we cut the URLs so we only get the part after the questionmark, and download the file through a custom function.
    
    indexOfQuestionmark = URL.index("?") +1
    URL = URL[indexOfQuestionmark:]
    #print("Cutted URL: ",URL)
    URL = downloadBaseUrl + URL
    
    fullURLList.append(URL)
    time.sleep(0.5)
    downloadFile(URL)



_Back to [start](https://elisabethluif.github.io/)_
