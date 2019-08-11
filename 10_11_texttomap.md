# Homework 10 + 11: Text to Map

I tried coding everything without looking at the solution beforehand. The following code is rather bulky, so I don't think it is as efficient as the solution, but I was able to learn a lot from doing it this way.
Also, I tried adding enough prints to clearly see what the current state of the data is.

Define imports, set folders and set variables

        import yaml
        import re, os
        from bs4 import BeautifulSoup
        import csv
        import operator
        from collections import OrderedDict 
        import time

        source = "./src/"
        target = "./tsv/"
        listOfFiles = os.listdir(source)

        ourTSV = []
        positionFrequencyDictionary =  {}
        tempFileCounter = 0
        counter = 0

Make seperate functions for saving

        def handleSaving(lengthOfFiles):
            global tempFileCounter
            global counter
            global ourTSV

            tempFileCounter+=1
            counter+=1

            print("length of files:", lengthOfFiles, " vs counter:", counter)

            if(tempFileCounter >= 20 or counter == lengthOfFiles):
                tempFileCounter=0
                saveOutCSV()

        def saveOutCSV():
            tempFileCounter = 0

            try:
                print("Trying to make directory: ", target)
                os.mkdir(target)
            except Exception:
                pass

            with open("./tsv/positionFrequencyTable.tsv", "w", encoding="utf8") as f9:
                f9.write("\n".join(ourTSV))
                print("Saved out total of ", counter, " files into one tsv.")




Go through all files

        for fileName in listOfFiles:
            handleSaving(len(listOfFiles))

            fullPath = os.path.join(source, fileName)

            with open(fullPath, encoding="utf8") as file:
                data = file.read()
                soup = BeautifulSoup(data, 'lxml')
Extract places

                try:
                    places= soup.find_all('placename')

                except Exception as e:
                    #print("EXCEPTION ",e)
                    pass

                print("Found all places. Length: ", len(places))

                # TGN = The Getty Thesaurus of Geographical Names

                for place in places:

                    latLon = 0
                    
Get TGN Data, as Lat/Long

                    try:
                        rawKey = place['key']
                        latLon = rawKey.replace('tgn,', '')
                    except Exception as e:
                        #print("EXCEPTION ",e)
                        pass


                    placeName = place.get_text()+ '*'+ str(latLon)

                    #print('Lat/Lon:', latLon, ", Place Name: ", placeName)


                    frequency = 0

CHECK IF PLACE IS ALREADY IN DICTIONARY

                    if placeName in positionFrequencyDictionary:
                        frequency = positionFrequencyDictionary[placeName]

                    frequency += 1


                    positionFrequencyDictionary[placeName] = frequency

                    var = "\t".join([placeName, str(frequency)])
                    ourTSV.append(var)




SORT BY FREQUENCY  

        with open("./tsv/positionFrequencyTable.tsv") as receivedTSV:
                reader = csv.reader(receivedTSV, delimiter='\t')

make into dict

                mydict = {rows[0]:rows[1] for rows in reader}

We need to use ordered dict to store order of objects. Also, convert value to int to sort by INT value and not STR value

                sortedOrderedDict = OrderedDict(sorted(mydict.items(), key=lambda t: int(t[1]), reverse=True))

                start_time = time.time()

                with open("./tsv/positionFrequencyTable_sorted.tsv", "w", encoding="utf8", newline='') as output_file:
                    fieldnameVar = ["Placename", "Frequency"]
                    tsv_writer = csv.DictWriter(output_file, fieldnames=fieldnameVar, delimiter='\t')
                    tsv_writer.writeheader()

                    for dictEntry in sortedOrderedDict:
                        tsv_writer.writerow({fieldnameVar[0]: dictEntry, fieldnameVar[1]: sortedOrderedDict[dictEntry]})

                    print("Saved out total of ", counter, " files into one tsv. Took: " ,(time.time() - start_time), " seconds.")




Result:

Unordered .tsv file
![posFrequencytable](https://user-images.githubusercontent.com/48948770/62836824-8e814280-bc67-11e9-9393-d20674372e66.png)

Ordered .tsv file
![sortedposFrequencytable](https://user-images.githubusercontent.com/48948770/62836823-8e814280-bc67-11e9-881c-337de92bcfce.png)





STEP 2 - COMPARING WITH US DATA



        import re, os
        from bs4 import BeautifulSoup
        import csv
        import operator
        from collections import OrderedDict 
        import time

        usDictionary ={}
        ourDictionary = {}



GET OUR DATA AND STORE IN DICTIONARY

        with open("./tsv/positionFrequencyTable_sorted.tsv" , encoding="utf-8") as receivedTSV:
            reader = csv.reader(receivedTSV, delimiter='\t')

            print("Filling our Dictionary...")
            counter = 0

            iteratorCounter = 0
            errorCounter = 0

            for line in reader:

                # skip first entry
                if(iteratorCounter == 0):
                    iteratorCounter += 1
                    continue

                frequency = line[1]
                splitLine = line[0].split("*")

                placeName = splitLine[0]

TRY TO GET COORDINATES

                try:
                    coordinates = int(splitLine[1])
                except:
                
IF MULTIPLE COORDINATES, TRY SPLITTING THEM AND ADDING TO LIST

                    try:
                        coordinates = splitLine[1].split(";")

                        # CHECK IF ITS A NUMBER
                        for coordinateArrayElement in coordinates:
                            int(coordinateArrayElement)



                    except Exception as e:
                        errorCounter +=1
                        continue

STORE FREQUENCY AND COORDINATES IN DATA ARRAY

                data = []
                data.append(int(frequency))
                data.append(coordinates)


HANDLE SAME NAME, APPEND TO LIST

                if(placeName in ourDictionary):

                    # MERGE FREQUENCY IF SAME PLACE NAME
                    ourDictionary[placeName][0] += int(frequency)

                    # ADD COORDINATES TO COORDINATE LIST
                    ourDictionary[placeName][1] = []
                    ourDictionary[placeName][1].append(coordinates)

                # OTHERWISE, ADD DATA
                else:
                    ourDictionary[placeName] = data

                counter+= 1


                if(counter > 10000):
                    print("Working ")
                    counter = 0


            print("Success. Filled our Dictionary, coordinate errors: ", errorCounter)


GET US GEO DATA  

        with open("./US.txt" , encoding="utf-8") as receivedTSV:
            reader = csv.reader(receivedTSV, delimiter='\t')

            print("Filling US Dictionary...")
            counter = 0

            for line in reader:
                placeName = line[1]

                usDictionary[placeName] = line
                counter+= 1

                if(counter > 500000):
                    print("Working ")
                    counter = 0

            print("Success. Filled US Dictionary...")










        notFoundCounter = 0
        qgisList = []
        counter = 0

LOOP THROUGH OUR DICTIONARY AND TRY TO FIND OUR PLACES

        print("Looking through our dictionary and comparing with us dictionary")

        for ourEntry in ourDictionary:

            if ourEntry in usDictionary:


                #print("FOUND ", ourEntry , " - " ,ourDictionary[ourEntry]," in US Dictionary!")
                latitude = usDictionary[ourEntry][4]
                longitude = usDictionary[ourEntry][5]

ourDictionary[placeName] - is a list of [0] place name and [1] DATA               
DATA - is a list  of [0]Frequency and [1-n] coordinate list

                currentEntryValues = ourDictionary[ourEntry]
                currentPlaceName = ourEntry

                currentFrequency = currentEntryValues[0]
                currentData = currentEntryValues[1]


                dataToSave = [currentPlaceName, latitude, longitude, currentFrequency]
                qgisList.append(dataToSave)

                #print("Lat/Lon", latitude," / ", longitude)
                #print("Current Place Name: ", currentPlaceName, " CurrentData: ", currentData, " Frequency: " , currentFrequency)

                if(counter > 500):
                    print("Working ")
                    counter = 0

            else:
                notFoundCounter +=1
                #print("ERROR, COULDNT FIND ",ourEntry," IN US DICTIONARY!")
                #input("Continue?")



        print("Created Qgis list with ", len(qgisList),"entries.\nCould not find " , notFoundCounter, " places in US Dictionary...")


        with open("./tsv/QGIS.csv", "w", encoding="utf8", newline='') as output_file:

            #fieldnameVar = ["Placename", "Latitude", "Longitude", "Frequency"]
            writer = csv.writer(output_file)
            qgisList.insert(0, ["Placename", "Latitude", "Longitude", "Frequency"])
            writer.writerows(qgisList)

        print("Successfully wrote QGIS file..")



Result:

Full Frequency/Posname/Lat/Long Table

![FullsortedposFrequencytable](https://user-images.githubusercontent.com/48948770/62836822-8e814280-bc67-11e9-9b18-c90f60812eec.png)







> GISting the “Dispatch” II: Mapping geographical data from the “Dispatch”



> Extract toponyms (place names) from the “Dispatch” (python)



> Calculate their frequencies (python)



> Add coordinates to those places (QGIS/python)
        Hint: you can create two lists 1) one with place names and their frequencies, 2) with place names and their coordinates; after that, you can merge them with python and generate a CSV file (placename, timeParameter, latitude, longitude, frequency) which can be used for creating a map in QGIS
        
        
> Map them in QGIS, using frequencies to size the markers on the map




****

_Back to [start](https://elisabethluif.github.io/)_
