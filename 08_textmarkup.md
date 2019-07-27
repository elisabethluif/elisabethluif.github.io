# Homework 8: Text Markup

## 8a

> Write a python script that will create clean copies of text from each issue of the “Dispatch” that you scraped before.


Define imports and source folders
```python
    import re, os
    source = "./src/"
    target = "./cleaned/"
```

Import data from src folder
```python
    filesInSrc = os.listdir(source)
```

Loop through all files from src
```python
    for fileName in filesInSrc:

        fullPath = os.path.abspath("./src/" + fileName)

        with open(fullPath, encoding="utf8") as file:
            data = file.read()
            #Clean data using regex
            cleanData = re.sub("<[^<]+>","",data)
            print("Reading from path: ",fullPath)


            cleanDataName = os.path.abspath("./cleaned/" + fileName + "_clean.xml")
            print("Clean data name: ", cleanDataName)
```

Create cleaned folder if it doesn't exist
```python
            try:
                os.mkdir(target)
            except Exception:
                pass
```

Save out to cleaned folder
```python
            with open(cleanDataName, "w", encoding="utf8") as newFile:
                newFile.write(cleanData)
```



## 8b

> Write a python script that will create clean copies of articles (!) from all issues of the “Dispatch”.

Define imports and source folders
```python
    import re, os
    from bs4 import BeautifulSoup

    source = "./src/"
    target = "./articles/"
```

If target directory doesn't exist, make one
```python
    try:
        os.mkdir(target)
    except Exception:
        pass
```

For the sake of seeing how many we already have converted
```python
    counter = 0
    print("Importing data from src folder: ", source)

    filesInSrc = os.listdir(source)

    for fileName in filesInSrc:

    fullPath = os.path.abspath("./src/" + fileName)
```
    
Get the xml file and load it in using BeautifulSoup
```python
    with open(fullPath, encoding="utf8") as file:
        data = file.read()
        soup = BeautifulSoup(data, 'lxml')
```

Get Date, and article content
```python
        date = soup.find_all("date", limit = 2)[1]
        issuedDate = date.get("value")
        articles = soup.find_all("div3", {"type":"article"})
        
        counter+=1
        print("Found ", len(articles), " articles...(",counter,")")
 ```
        
Loop through all articles in one dataset
```python
        for i in range(0, len(articles)):

            currentArticle = articles[i].get_text()
            newFileName = os.path.abspath("./articles/"+ issuedDate+"/")
```

For convenience, store them in subfolders
```python
            try:
                os.mkdir(newFileName)
            except Exception:
                pass
```

Save out article to .txt file
```python
            newFileName = os.path.join(newFileName, issuedDate + "_" + str(i) +".txt")
            
            #print("Opening path... ", newFileName, "...")

            with open(newFileName, "w", encoding="utf8") as newFile:
                newFile.write(currentArticle)
                #print("Writing article ", newFileName, "...")
                
```
        


****

_Back to [start](https://elisabethluif.github.io/)_
