# Working with Data

## Type of data 
Prior to any storing or querying, we need to understand the format of the data. Each type will use different method to analyze the data. The common few types are the following:

* **CSV**:

Comma-separated values (CSV) is a delimited text file that uses a comma to separate values. It stores tabular data in plain text. A common csv files looks like the following. Usually, the first line has the column names, and the line below is a record in the data. You can use Excel to open and view the csv files.  

```csv
Year,Make,Model,Description,Price
1997,Ford,E350,"ac, abs, moon",3000.00
1999,Chevy,"Venture ""Extended Edition""","",4900.00
1999,Chevy,"Venture ""Extended Edition, Very Large""",,5000.00
1996,Jeep,Grand Cherokee,"MUST SELL!air, moon roof, loaded",4799.00
```

CSV file is good for tabular format. It is human readable while easy to implement and parse. However, it has poor support with special characters and is lack of universal standard. This link summarize the csv format quite well: <https://www.shopping-cart-migration.com/must-know-tips/5985-csv-what-why-and-how>

* **JSON**:

JavaScript Object Notation (JSON) is a lightweight data-interchange format. It is language independent. When exchanging data between website and server, we can only exchange text-based data. JSON is a good one to choose as it is text. We can convert any JS object into JSON and send it to the server, and vice versa. Meanwhile, it is really easy to parse and translate. A common JSON format looks like the following:

```JSON
{
    "glossary": {
        "title": "example glossary",
		"GlossDiv": {
            "title": "S",
			"GlossList": {
                "GlossEntry": {
                    "ID": "SGML",
					"SortAs": "SGML",
					"GlossTerm": "Standard Generalized Markup Language",
					"Acronym": "SGML",
					"Abbrev": "ISO 8879:1986",
					"GlossDef": {
                        "para": "A meta-markup language, used to create markup languages such as DocBook.",
						"GlossSeeAlso": ["GML", "XML"]
                    },
					"GlossSee": "markup"
                }
            }
        }
    }
}
```

Benefit of JSON is it is less verbose comparing to XML. It is object aligned, which means it doesn't need to be a tabular format. Instead, you can have more flexibility regarding schema for object database. However, it is still difficult to understand by human because of many braces and commas, though it is much better than XML already.

* **XML**:

eXtensible Markup Language is a markup language much like HTML. It is designed to store and transport data, while HTML was designed to focus on the visualization of the data. A common JSON format looks like the following (same content as the JSON example):

```XML
<!DOCTYPE glossary PUBLIC "-//OASIS//DTD DocBook V3.1//EN">
 <glossary><title>example glossary</title>
  <GlossDiv><title>S</title>
   <GlossList>
    <GlossEntry ID="SGML" SortAs="SGML">
     <GlossTerm>Standard Generalized Markup Language</GlossTerm>
     <Acronym>SGML</Acronym>
     <Abbrev>ISO 8879:1986</Abbrev>
     <GlossDef>
      <para>A meta-markup language, used to create markup
languages such as DocBook.</para>
      <GlossSeeAlso OtherTerm="GML">
      <GlossSeeAlso OtherTerm="XML">
     </GlossDef>
     <GlossSee OtherTerm="markup">
    </GlossEntry>
   </GlossList>
  </GlossDiv>
 </glossary>
```

Compared to JSother text-based data transmission format, it is quite verbose, which means it can cause higher storage and transportation cost. However, it is still important to learn as the industry still uses XML heavility to transfer the data, expeicially for websites.

* **YAML**:

YAML is a human-readable data serialization language. We don't need to mention whether the line is a string or not. Usually it is auto-detected by the parser. Each item has a hyphen in front of it. And idndentation shows the hierarchy.

Example:

```YAML
---
- A
- B
- C
- ['a', 'b', 'c']

# Dictionary
---
AA: A
BB: B
CC: C
DD:
  - a
  - b
  - c
```

The '---' means the beginning of YAML doc. "..." to indicate the end of the doc

## Tools

There are many different types of tools that can faciliatate data understanding based on different types of format:

* **Command Line**:

A great place to understand your command line is here <https://explainshell.com/>.

For csv files:

```bash
#<https://www.linuxtechi.com/linux-ls-command-examples-beginners/>
Ls -al
# Looks at the first line in the csv since it is just text file
Head -1 xxx.csv

Vi xxx.csv

# Concatenate and have the word count
Cat xxx.csv | wc -l
# Clean the window
Clear
Cat xxx.csv | head -1
Cat xxx.csv | tail -1
# Be able to go into each line
Tmux new
```

For JSON files:

See tutorials here: <https://stedolan.github.io/jq/tutorial/>

```bash
#There is only one single line
Cat views.json | wc -1
# Count the screenName
Cat views.json | jq -C ‘.[]|.tableAuthor|.screenName’ | sort | uniq | wc -l
# Less views-pretty.json
Cat view.json | jq ‘.’ > views-pretty.json
# You can also do select in jq select(id = ,,)
```

For XML files:

```bash
#There is only one single line
Cat views.xml | wc -1
Cat views.xml | xmllint --format - | pygmentize -l xml | less
```

* **Jupyter or IPython**:

Python is an interpreted language which execute one statement at a time. IPython shell is an enhanced Python interpreter on the command line, while Jupyter notebook acts as a type of interactive document with kernels.

Python's Jupyter kernel uses IPython system for the underlying behaviour. You can choose Python 2 or 3 based on your coding habit. The document saved from Jupyter notebook will have .ipynb as the ending.

```bash
ipython             # to enter the IPython command
jupyter notebook    # to enter the jupyter notebook, which will open the browser usually at localhost 8888
exit()              # or Ctril-D to quite the Ipython command promp
```

```python
%run file.py        # run the python file in an empty namespace, press ctrl-C to interrupt
b?                  # ? will return info regarding the object
add_something??     # will show the function source code if possible
%load file.py       # import a script into the code cell
%paste              # take what in the clipboard
%cpaste             # give you a prompt for pasting the code into and you can paste as many as you want before your execution, leave with Ctrl-C
%matplotlib         # matplot in IPython
%matplotlib inline  # matplot in notebook
```

% is the magic command in IPython, see <https://ipython.readthedocs.io/en/stable/interactive/magics.html>
For more shorcut in IPython: <https://jakevdp.github.io/PythonDataScienceHandbook/01.02-shell-keyboard-shortcuts.html>

Working with YAML file:

```python
import yaml
while open ("ex.yml") as ex:
    rst = yaml.load(ex)
    print(rst)
    type(rst)
```

* **Excel**: It is great for csv format to look at the overall dataset.
* **REPLs**: A read-eval-print loop which takes single input and return the result after evaluation
* **Other Exploratory Environments**

## Source

* <https://json.org/example.html>
* <https://www.w3schools.com/js/js_json_intro.asp>
* <https://en.wikipedia.org/wiki/Comma-separated_values>
* <https://dzone.com/articles/why-you-cloud-be-using-json-vs-xml>
* <http://candidjava.com/advantage-and-disadvantage-of-json/>
* <https://www.w3schools.com/xml/xml_whatis.asp>
* <https://beginnersbook.com/2018/10/advantages-and-disadvantages-of-xml/>
* <https://www.oreilly.com/library/view/python-for-data/9781491957653/ch02.html#ipython_basics>
* <https://learning.oreilly.com/library/view/network-programmability-and/9781491931240/ch05.html#dataformats>