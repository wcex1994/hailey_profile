## Working with Data

### Type of data 
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

A common JSON format looks like the following (same content as the JSON example):

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

### Tools 

There are many different types of tools that can 

Source:

* <https://json.org/example.html>
* <https://www.w3schools.com/js/js_json_intro.asp>
* <https://en.wikipedia.org/wiki/Comma-separated_values>
* <https://dzone.com/articles/why-you-cloud-be-using-json-vs-xml>
* <http://candidjava.com/advantage-and-disadvantage-of-json/>
