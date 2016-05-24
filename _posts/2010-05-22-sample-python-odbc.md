---
layout: post
title: "Sample python database connection via ODBC"
date: 2010-05-22
---

First, download and install the <a href="http://code.google.com/p/pyodbc/">pyodbc</a> module.  I needed to additionally install the packages for g++ and python-dev on my debian box.

The install was simple:
<pre>
python setup.py build
python setup.py install
</pre>
Test the install with the following simple query against the e-MD's database:
<pre>
import pyodbc
c = pyodbc.connect('DSN=TOPSDATA;UID=emds;PWD=******')
cursor = c.cursor()
cursor.execute("select * from VIEW_Physician")
while (1):
  row = cursor.fetchone()
  if row == None:
    break
  print row
cursor.close()
c.close()
</pre>