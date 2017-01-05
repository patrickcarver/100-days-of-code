# 100 Days Of Code - Log

### Day 1: January 3, 2017, Tuesday

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

### Day 2: January 4, 2017, Wednesday

**Today's Progress**:
Created list of House member's xml files from main House member listing.

**Thoughts:**
The goal today was to get a list of the members' individual xml files, which I got done.
The xml for the roster has separate nodes for the Speaker of the House and Speaker Pro Tempore from the rest of the members. And the members' listing was interesting.
Instead of a straight flat list, there were sublists that grouped together 5 members
that were on the same row as displayed with XLST via the browsers. The nodes in the
sublist were "M1_LINK", "M2_LINK", etc. So, this required 5 separate SweetXml
xpath calls get the regular member file names. I created a list of the 2 officers
so I could glue it together with the 5 member lists to make a complete list.
Lastly, I refactored by scooping all that list creation in to a function.

Not a huge milestone. I have church supper on Wednesdays and a Bible study with a
friend of mine first thing Thursday morning to prepare for, so Wednesday evenings
aren't going to be huge in output. Though, I did accomplish a nice and
necessary piece of the app. 

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)
