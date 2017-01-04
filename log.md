# 100 Days Of Code - Log

### Day 1: January 3, 2017, Monday

**Today's Progress**:
Started first app: Mississippi Legislature Data Slurper aka "MsLegis"

**Thoughts:**
I started an app that would download and parse the xml from the
MS Legislature website which contains info on all its members. Then, it will store
the parsed data in a database (a NoSQL one perhaps?) I'm doing it in Elixir
so I can gain experience in that language.

Found 2 modules to use; HTTPotion to get the xml from the site and SweetXml to parse it.
HTTPotion is a no-brainer to use, but SweetXml is more tricksy. I had to clean out
the tab and endline chars along with the metadata at the top of the document in
order for it to start parsing. Plus, querying the XML nodes is case sensitive,
important to know since the nodes are in ALL CAPS and using lower case to search
returns _nil_.

SweetXml looks like it has some nice and easy ways to turn the nodes into Elixir
"objects", but I wonder if the other Elixir XML modules out there would require
less cleanup beforehand.

That said, I was successful in getting the xml to parse and got the app to print out the
Speaker of the House's name (Philip A. Gunn, by the way).

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)
