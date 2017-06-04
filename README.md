import urllib2
first = int(input('First:'))
last = int(input('Last:'))

while first <= last:
    response = urllib2.urlopen("https://uisacad5.uis.edu/~rloui2/540/project2/" + str(first) +".pdf")
    file = open("/Users/Rajasekhar/Desktop/project_pdf/"+str(first) + ".pdf", 'wb')
    file.write(response.read())
    file.close()
    response.close()
    first = first + 1
print("Finished")
