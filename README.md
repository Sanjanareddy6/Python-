from cStringIO import StringIO
from pdfminer.pdfinterp import PDFResourceManager, PDFPageInterpreter
from pdfminer.converter import TextConverter
from pdfminer.layout import LAParams
from pdfminer.pdfpage import PDFPage
import os
import sys, getopt
from Crypto.Hash import SHA256

#converts pdf, returns its text content as a string
def convert(fname, pages=None):
    if not pages:
        pagenums = set()
    else:
        pagenums = set(pages)
        
    output = StringIO()
    manager = PDFResourceManager()
    converter = TextConverter(manager, output, laparams=LAParams())
    interpreter = PDFPageInterpreter(manager, converter)

    infile = file(fname, 'rb')
    for page in PDFPage.get_pages(infile, pagenums):
        try:
            interpreter.process_page(page)
        except:    
            print("Pdf is not supporting pdf file to convert to text",infile)
            continue
    infile.close()
    converter.close()
    text = output.getvalue()
    output.close
    return text 
   

def convertMultiple(pdfDir, txtDir):
    if pdfDir == "": pdfDir = os.getcwd() + "\\" #if no pdfDir passed in 
    for pdf in os.listdir(pdfDir): #iterate through pdfs in pdf directory
        fileExtension = pdf.split(".")[-1]
        if fileExtension == "pdf":
            pdfFilename = pdfDir + pdf
            try:
               text = convert(pdfFilename) #get string of text content of pdf
               textFilename = txtDir + pdf+".txt"
               print (textFilename)
               textFile = open(textFilename, "w") #make text file
               textFile.write(text) #write text to text file
            except:
               print("Pdf convertMultiple is hitting some issue",pdfFilename)
               continue
pdfDir = "/Users/Rajasekhar/Desktop/project_pdf/"
txtDir = "/Users/Rajasekhar/Desktop/project_txt/"
try:
    convertMultiple(pdfDir, txtDir)
except:
    print("Exception at convert function")
    
