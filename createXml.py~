#!/usr/bin/python

import MySQLdb
import datetime
import xml.etree.cElementTree as ET
import os.path, time
import sys

#values are passed as arguments when called from getInfo.py
databaseUsername=sys.argv[1]
databasePassword=sys.argv[2]
dbName=sys.argv[3]

#where the XML will be saved to, make sure it points to the htdocs folder
filePath="/home/lighttpd/raspberryweather.com/http/output.xml"

db = MySQLdb.connect("localhost",databaseUsername,databasePassword,dbName)
todayDate=datetime.datetime.now()
cursor = db.cursor()
sql="SELECT * FROM temperatures WHERE dateMeasured='%s'" % todayDate.strftime('%Y-%m-%d')

root = ET.Element("root")

doc=ET.SubElement(root,"measureDate")
fieldTemp=ET.SubElement(doc,"date")
fieldTemp.text= todayDate.strftime('%m %d %Y')

cursor.execute(sql)
results=cursor.fetchall()

for row in results:
	doc=ET.SubElement(root,"reading")
	fieldTemp=ET.SubElement(doc,"temperature")
	fieldTemp.text=str(row[1])

	fieldHumidity=ET.SubElement(doc,"humidity")
	fieldHumidity.text=str(row[2])

	fieldHourMeasured=ET.SubElement(doc,"dayHour")
	measurementTime=str(datetime.timedelta(minutes=row[4]))
	measurementTime=measurementTime[:-3] #remove the pesky :00
	fieldHourMeasured.text=str(measurementTime)	

tree = ET.ElementTree(root)
tree.write(filePath)
