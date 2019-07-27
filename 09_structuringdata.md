# Homework 9: Structuring Data

  #Import libraries and define source/target folders
  import yaml
  import re, os
  from bs4 import BeautifulSoup

  source = "./src/"
  target = "./tsv/"
  listOfFiles = os.listdir(source)
  
  #So we can save .tsv every 100 entries
  fileCounter = 0
  
  ourCSV = []

  #Go through each file
  for fileName in listOfFiles:
      fullPath = os.path.join(source, fileName)
      
      with open(fullPath, encoding="utf8") as file:
          data = file.read()
          soup = BeautifulSoup(data, 'lxml')
          
          # Get date and all articles
          date = soup.find_all("date", limit = 2)[1]
          issuedDate = date.get("value")
          articlesÁndItems = soup.find_all("div3")

          print("Found ", len(articlesÁndItems), " articles/items")
          counter = 0
          
          #Go through each article/entry
          
          for singleEntry in articlesÁndItems:
              counter+=1
              
              #Set head
              head='None'

              if(singleEntry.head is not None):
                  head=singleEntry.head;

              #print("Single entry:", singleEntry)



              #Set entry type, with Try statement to catch Errors
              entryType =''

              try:
                  entryType=str(singleEntry['type'])
              except KeyError as error:
                  print("ERROR ", error)
                  entryType="None"



              #Clean text variable

              text = re.sub("<[^<]+>", "", singleEntry.get_text())
              text = re.sub(" +\n|\n +", "\n", text)
              text = re.sub("\n+", " ", text)
              
              #Make TSV Entry
              var = "\t".join(
                  [issuedDate+'_'+str(counter),
                   issuedDate,
                   entryType,
                   head,
                   text]
                  )
              ourCSV.append(var)
          
          fileCounter+=1
          print("Files handled: " + str(fileCounter))
  
          #Save to TSV every 100 files
          if(fileCounter > 100):
              fileCounter = 0
              with open("./tsv/dispatch_as_TSV.tsv", "w", encoding="utf8") as f9:
                  f9.write("\n".join(ourCSV))
                  print("Saved out ", counter, " files into one tsv.")




_Back to [start](https://elisabethluif.github.io/)_
