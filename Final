import datetime
import time
import re
import os
import errno
import sys
import smtplib
import fnmatch

#Function 1
#This function send a email and report all errors on this one
def sendMail(inputValue, allErrorLogs):

    if (createErrorLogFile and ('y' ==  inputValue)):
          from email.mime.multipart import MIMEMultipart
          from email.mime.text import MIMEText
          from email.mime.base import MIMEBase
          from email import encoders
#Change here your email (sending and receiving can be different)
          fromaddr = "XXX"
          toaddr = "XXX"

          msg = MIMEMultipart()

          msg['From'] = fromaddr
          msg['To'] = toaddr
          msg['Subject'] = "Log du jour"
          body = ""
          for key in allErrorLogs.keys():
              body += "Erreurs de type "+key+" :\n"
              for error, message in allErrorLogs[key].items():
                  body += "\tNombre d'erreurs "+error+" : "+str(len(message))+"\n"

          msg.attach(MIMEText(body, 'plain'))

          filename = getFileNameWithPath()
          attachment = openFile(filename)

          part = MIMEBase('application', 'octet-stream')
          part.set_payload((attachment).read())
          encoders.encode_base64(part)
          part.add_header('Content-Disposition', "attachment; filename= %s" % filename)

          msg.attach(part)
#You need to change here your server mail (here it's GMX)
          server = smtplib.SMTP('mail.gmx.com', 587)
          server.starttls()
#Put your password here (sending password's email)
          server.login(fromaddr, "XXX")
          text = msg.as_string()
          server.sendmail(fromaddr, toaddr, text)
          server.quit()
          print("Mail sended")
          
#Function 2:
#This function create two list (one list for each log type with one dictionarie for each error)
def MyErrorCodes():
    ListErrorCode={
    'apache': {
            '401':[],
            '403':[],
            '404':[]
              },
    'auth': {'POSSIBLE':[],
            }
        }
    return ListErrorCode

#Function 3
#This function get all error in logs files with a path
def getAllErrorLogsWithPath(path, ListErrorCode):
     for root, dirs, files in os.walk(path):
        for file in files:
            if (file.lower().endswith(('.gz'))):
                import gzip
                isGzip = True
                logFile = gzip.open(path + file , 'r')
            else:
                logFile = openFile(path + file)
                isGzip = False
            print(path + file + ' analyse in progress...')
            for logType in ListErrorCode.keys():
                if fnmatch.fnmatch(file, "*"+logType+"*"):
                    print("Search for 401, 403 or 404 or possible break-in error in current file in progess...")
                    for logRow in logFile:
                        if (isGzip):
                            logRow = logRow.decode('utf-8')
                        dictCodes = ListErrorCode.get(logType)
                        for code in dictCodes.keys():
                            if (getErrorLog(logRow, code)):
                                ListErrorCode[logType][code].append(logRow)
                    print(path + file + ' analyse finished...')

        return ListErrorCode



#Function 4
#This function open a file in read mode with a path and file name
def openFile(fileLogPath):
      return open(fileLogPath)

#Function 5
#This function check error 401, 403 and 404 for a row and return error code found if there is one else return false
def getErrorLog(logRow, code):
    #regexErrorCodes = r"^(.*?(\b(401|403|404|POSSIBLE)\b)[^$]*)$" #Ceci est une regex : https://bit.ly/2U2S9lB
    match = re.search(r"^(.*?(\b(" + re.escape(code) + r")\b)[^$]*)$", logRow)
    if (match):
        return match.group(2)
    else:
        return ''

#Function 6
#This function show all errors found in console if user press 'y'
def showErrorLogs(inputValue, allErrorLogs):
    print(inputValue)
    if(inputValue == 'y'):
        print(allErrorLogs)
    else:
        print('OK, no result on console')

#Function 7
#This function write errors file if user press 'y'
def createErrorLogFile(inputValue):
    if('y' == inputValue):
        erroFileName = writeErrorLogs(allErrorLogs)
        time.sleep(1)
        print('File ' + erroFileName + ' was created')

#Function 8
#This function create (if not exists) '/log/ folder and create an .error file with today's date
def writeErrorLogs(allErrorLogs):
    errorsFileName = getFileNameWithPath()
    if not os.path.exists(os.path.dirname(errorsFileName)):
        try:
            #os.path.dirname
            os.makedirs(os.path.dirname(errorsFileName))
        except OSError as exc: # Guard against race condition
            if exc.errno != errno.EEXIST:
                raise
    if os.path.exists(errorsFileName):
        os.remove(errorsFileName)
        print("deleted")
    with open(errorsFileName, "w") as errorsRecap:
        for key in allErrorLogs.keys():
            errorsRecap.write("Erreurs de type "+key+" :\n")
            for error, message in allErrorLogs[key].items():
                errorsRecap.write("Erreurs "+error+" : \n")
                for line in message:
                    errorsRecap.write(line)
        errorsRecap.close()
    return errorsFileName

def getDateOfTheDay():
    return  datetime.datetime.now().strftime("%d%m%Y")

def getFileNameWithPath():
    return "./errors/" + getDateOfTheDay() + "/logs.error"

#Main Application

logsPath = "./logs/"
ListErrorCode = MyErrorCodes()
print("Starting read all logs files ...")
allErrorLogs = getAllErrorLogsWithPath(logsPath, ListErrorCode)

if not(allErrorLogs):
    print("There are no logs in the folder : " + logsPath)
    input("Press any case to exit !")
    sys.exit()

if (len(sys.argv) > 1) and (sys.argv[1] == "all"):
    console_input = input('Result on console : y/n ? \n')
    showErrorLogs(console_input, allErrorLogs)

    file_input = input('Generate error file on ./errors and sending report : y/n ? \n')
    createErrorLogFile(file_input)

    mail_input =  input('Send mail with error log : y/n ? \n')
    sendMail(mail_input, allErrorLogs)
    
else:
    showErrorLogs('n', allErrorLogs)
    createErrorLogFile('y')
    sendMail('y', allErrorLogs)

input("Press any case to exit !")
sys.exit()
