# Homework 8: Text Markup











**8b**

#Define imports and source folders
import re, os
from bs4 import BeautifulSoup

source = "./src/"
target = "./articles/"


#if target directory doesn't exist, make one
try:
    os.mkdir(target)
except Exception:
    pass

#for the sake of seeing how many we already have converted
counter = 0
print("Importing data from src folder: ", source)

filesInSrc = os.listdir(source)

for fileName in filesInSrc:

    fullPath = os.path.abspath("./src/" + fileName)
    
    #Get the xml file and load it in using BeautifulSoup
    with open(fullPath, encoding="utf8") as file:
        data = file.read()
        soup = BeautifulSoup(data, 'lxml')
        #Get Date, and article content
        date = soup.find_all("date", limit = 2)[1]
        issuedDate = date.get("value")
        articles = soup.find_all("div3", {"type":"article"})
        
        counter+=1
        print("Found ", len(articles), " articles...(",counter,")")
        
        #Loop through all articles in one dataset
        for i in range(0, len(articles)):

            currentArticle = articles[i].get_text()
            newFileName = os.path.abspath("./articles/"+ issuedDate+"/")
            
            #For convenience, store them in subfolders
            try:
                os.mkdir(newFileName)
            except Exception:
                pass
            
            #Save out article to .txt file
            newFileName = os.path.join(newFileName, issuedDate + "_" + str(i) +".txt")
            
            #print("Opening path... ", newFileName, "...")

            with open(newFileName, "w", encoding="utf8") as newFile:
                newFile.write(currentArticle)
                #print("Writing article ", newFileName, "...")
                

        




_Back to [start](https://elisabethluif.github.io/)_
