README
========

General info
-------------
The crime report PDF (UPD8126.PDF, date 8/14/2014) was downloaded from http://www.city.urbana.il.us/_Police_Media_Reports/.

Tools
-------
PDFMiner - https://github.com/euske/pdfminer/ (Clone or download as a .zip from https://github.com/euske/pdfminer/archive/master.zip; run ```$ python setup.py install```.)

First goal
-----------
Accurately extract text for the first three crimes (U14-04860, U14-04859, U14-04830). These constitute part of the first page of PD8126.PDF.

<img src="https://cloud.githubusercontent.com/assets/4472418/3943367/3e6010e0-259c-11e4-8b82-e686734d2db6.png" width="350px">

Second goal
--------------
Programmatically categorize info for the first three crimes (U14-04860, U14-04859, U14-04858).

### Subgoal 1
Deal with just U14-04860 -- assume we have that text and pull out the info we need
    
### Subgoal 2
Deal with the whole file containing the first three cases, including the header(s)/other info.

First efforts/steps
--------------------
### First goal
* ```$ wget http://www.city.urbana.il.us/_Police_Media_Reports/UPD8126.PDF```

* ```$ pdf2txt.py UPD8126.PDF > UPD8126.txt```

* Get just the text before the case/report thingie that we do not want, U14-04862.
  ```$ awk '/U14-04862/{exit}1' UPD8126.txt > first_three.txt```

### Second goal, subgoal 1
First we need to figure out what info to pull out. Here's what the first crime re
port/case whatever it's called looks like:

```
U14-04860    BATTERY-DOMESTIC                    720-5/12-3.2
              LOCATION:  1500 BLOCK OF HUNTER ST
              OCCURRED: 8/11/2014  19:00  REPORTED:  8/13/2014  13:41
              OFFICER: BRAZELTON,DIXIE

     SUMMARY: THE VICTIM AND OFFENDER ARE SISTERS. THEY ENGAGED IN A
              PHYSICAL ALTERCATION ON 08/11/14.  THE VICTIM DID NOT WANT
              THE POLICE INVOLVED AND DID NOT WANT ANYTHING DONE.

      PEOPLE: VICTIM    AGE: 29 SEX: F  URBANA          IL
              OFFENDER  AGE: 31 SEX: F
```

* Looks pretty straightforward. The fields are, at least for this one (organized by line):
  * top line has the case (let's just call it a "case") number, type of crime (I guess), and some weird number
  * location
  * date and time when the crime occurred and when it was reported
  * name of officer (last, first)
  * summary/description
  * people involved, specifically the victim: age, sex, city, state
  * people involved, specifically the offender: age, sex

* Maybe the info above should be held in an instance of a class like
```Case```, e.g. for location stuff, maybe something like:
``` python
import re
class Case(object):

    def __init__(self, text_chunk):
        self.text = text_chunk

    @property
    def locations(self):
	    locations = re.findall('LOCATION:\s+(\S.*)', self.text)
	    if not locations:
		    return '(No locations found in crime report)'
	    else:
		    return [location.rstrip() for location in locations]
```

``` python
>>> case = Case(text)  # "text" is the U14-04860 text above
>>> case.locations
['1500 BLOCK OF HUNTER ST']
```

```
>>> case = Case('LOCATION: some place \n LOCATION: another place') 
>>> case.locations
['some place', 'another place']

>>> case = Case('blah blah blah')
>>> case.locations
'(No locations found in crime report)'
```

To be continued......
