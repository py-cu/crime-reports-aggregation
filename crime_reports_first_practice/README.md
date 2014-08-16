[[README]]
========

General info
-------------
The crime report PDF (UPD8125.PDF, date 8/14/2014) was downloaded from http://www.city.urbana.il.us/_Police_Media_Reports/.

Tools
-------
PDFMiner - https://github.com/euske/pdfminer/ (Clone or download as a .zip from https://github.com/euske/pdfminer/archive/master.zip; run ```$ python setup.py install```.)

First goal
-----------
Accurately extract text for the first three crimes (U14-04855, U14-04871, U14-0486a9). The file portion.png (embedded below) shows that these span two pages of PD8125.PDF.

<img src="https://cloud.githubusercontent.com/assets/4472418/3942922/058a04ce-257e-11e4-8150-27483caf5fec.png" width="350px">

Second goal:
--------------
Programmatically categorize info for the first three crimes (U14-04855, U14-04871, U14-0486a9).
