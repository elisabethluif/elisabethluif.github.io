# Homework 9: Structuring Data

> Generate one TSV-file with the entire content of the “Dispatch”, where each article (or, more broadly—an entry) is a single record; each record should include:
> 1. date;
> 2. type of an entry (there are articles, advertisements, notices, etc);
> 3. header;
> 4. the text of an entry.

Import libraries and define source/target folders
```python
  import yaml
  import re, os
  from bs4 import BeautifulSoup

  source = "./src/"
  target = "./tsv/"
  listOfFiles = os.listdir(source)
  fileCounter = 0
  tempFileCounter=0
  ourCSV = []
```

Go through each file
```python
  for fileName in listOfFiles:
      fullPath = os.path.join(source, fileName)

      with open(fullPath, encoding="utf8") as file:
          data = file.read()
          soup = BeautifulSoup(data, 'lxml')
 ```
          
Try getting date, catch exceptions
```python
          try:
              date = soup.find_all("date", limit = 2)[1]

          except Exception:
              pass
      
          issuedDate = date.get("value")
```
          
Get all articles and items and iterate over them
```python
          articlesÁndItems = soup.find_all("div3")

          #print("Found ", len(articlesÁndItems), " articles/items")
          counter = 0

          for singleEntry in articlesÁndItems:
              counter+=1
              #Extract variables we need for tsv file
              head='None'

              if(singleEntry.head is not None):
                  head=singleEntry.head;

              #print("Single entry:", singleEntry)
```

Catch exception for entrytype
```python        
             entryType =''

              try:
                  entryType=str(singleEntry['type'])
              except KeyError as error:
                  print("ERROR ", error)
                  entryType="None"
```

Clean text variable
```python
              text = re.sub("<[^<]+>", "", singleEntry.get_text())
              text = re.sub(" +\n|\n +", "\n", text)
              text = re.sub("\n+", " ", text)
```              
              
Prepare for TSV
```python
              var = "\t".join(
                  [issuedDate+'_'+str(counter),
                   issuedDate,
                   entryType,
                   head,
                   text]
                  )
              ourCSV.append(var)

          fileCounter+=1
          tempFileCounter+=1
          print("Files handled: " + str(fileCounter))
```

Save out every 100 entries so we don't loose all data
```python
          if(tempFileCounter >= 100):
              tempFileCounter = 0
          
              with open("./tsv/dispatch_as_TSV.tsv", "w", encoding="utf8") as f9:
                  f9.write("\n".join(ourCSV))
                  print("Saved out ", counter, " files into one tsv.")
```

Save out when all files have been handled
```python
  with open("./tsv/dispatch_as_TSV.tsv", "w", encoding="utf8") as f9:
      f9.write("\n".join(ourCSV))
      print("Saved out ", counter, " files into one tsv.")
```

Result:

![Screenshot](https://user-images.githubusercontent.com/48948770/61997211-563e0980-b096-11e9-9124-b3f54922992b.png)


****

_Back to [start](https://elisabethluif.github.io/)_
