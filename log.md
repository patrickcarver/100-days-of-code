# 100 Days Of Code - Log

### Day 1: January 3, 2017, Tuesday

**Today's Progress:**
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

**Today's Progress:**
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

### Day 3: January 5, 2017, Thursday

**Today's Progress:**
Got the members' xml pages to load into memory

**Thoughts:**
Explored if SweetXml could parse xml without me having to remove the first 5 lines
of metadata in the roster and members' page as well as all the tab and endline chars.  Verdict: Nope. I tried the "parse" function, but it just threw an unhelpful error.

I was able to list the member xml files to the cmd line. When I tried to concatenate
the name of the xml files with the base url, I kept getting an error. Turns out
HTTPotion spits out character lists aka single-quote strings for the member xml
file names, but my base url string was a binary aka double-quote strings. In Elxir,
you can't put those together. So, I converted the character lists to binaries, and,
voila, it worked.

I also got the xml files themselves to load and spew their contents straight to
the command line, though it errored on Mims's page I think because of the quotation
marks in a section. I changed it to print out the party affiliation and again it
errored after displaying a good many. I'll have to figure out tomorrow what it's
choking on.   

Need to refactor out the common code in the "clean_list_xml" and "clean_member_xml"
functions since they differ only in the string that contains the xml metadata that
needs to be removed.

Also, I need to write actual tests for this thing.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

### Day 4: January 7, 2017, Saturday

**Today's Progress:**
Refactored clean xml functions into module. Refactored code for xml processing.

**Thoughts:**
Spent Friday moving cubicle pieces for new office, so took the evening off to rest.

I cleaned up the functions that remove the xml metadata for the member list and
member profile xml pages and are now a module. The strings I used to remove
the metadata are now in a module struct.

Also, refactored the xml processing by creating more functions. Moved the
HTTPotion outside those functions so they aren't dependent on it, i.e., it will
be easier to change to something else if I so decide.

On the git front, I remembered to start using branches instead of checking in
on the master branch.

Added a couple of tests of the xml metadata, but relied they were pointless.
Just testing a string in the struct to a string in the test doesn't matter.

Overall, the app doesn't do anything more, but is structured cleaner.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 5: January 8, 2017, Sunday

**Today's Progress:**
Create module to store xquery ...er... queries and a module to perform those queries.

**Thoughts:**
Another day of refactoring, this time focusing on the `create_list` function in
the main module.

I researched SweetXml functions and saw that I could dynamically put in text
by using `sigil_x` instead of `~x`. This let me put the actual queries in a struct
that could be used by functions that execute said queries.

I created a module to place the xquery-running functions and named it `GetLinks`
with functions `get_officer_link` and `get_member_links`. After looking
at the code for a little bit, I realized those functions were pretty generic.
`get_officer_link` just returned text and `get_member_links` returned a list. So,
I renamed them `get_text` and `get_list`, respectively, and renamed the module `GetXQueryResult`.

For tomorrow, I want to try to make the `GetXQueryResult` more generic and not
rely directly on the SweetXml module, i.e., make it easier to swap in another
Elixir xml module (which I may do at some point for comparison's sake).

Oh yeah, I _still_ need to write tests.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 6: January 11, 2017, Wednesday
**Today's Progress:**
Fixed runtime errors. Started getting assigning variables from member xml data.

**Thoughts:**
Moving furniture all day for my new office on Monday and working out on Tuesday
left me very tired those evenings to I only did a few minutes of coding. I did
wake up early on Wednesday morning and made up some time.

I ran the app and fixed runtime issues due to using binaries instead of charlists.

I did find an option for `SweetXml`'s `xpath` function to return a list of
strings rather then a charlist that needs to be run through an `Enum.map` to convert
to strings.   

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 7: January 12, 2017, Thursday
**Today's Progress:**
Piddled with getting a stubborn member xml file to process.

**Thoughts:**
Early morning Bible study + long day at work = not very productive evening of coding.

There is an issue with the member xml page for Representative Sam Mims. In the text
for his bio, there are two funky quote characters that I'm sure are the
cause of the app throwing up when trying to parse it.

I've put in some coding to confirm my suspicions and I'm trying to figure a way
to handle the issue.  Not too much progress because sleep grasped me in its loving
embrace not to long after I started coding.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 8: January 13, 2017, Friday
**Today's Progress:**
Conquered the the parsing issue with Sam Mim's xml page. Abstracted out fetching
xml from URLs.

**Thoughts:**
And early bedtime the night before meant a very early rising for yours truly.
A great time to catch up on some coding.

...And... Huzzah! Fixed the parsing of Mim's page! It was the two weird quote characters
that was gumming things up; the stumbling block for me was figuring out what
exactly the app saw them as. After a bunch of trial and error, what led me to
the solution was looping through the characters of the xml string, calling `String.grapheme` on each, and printing each on a separate line on the command line.
I just skimmed through the output and saw how those strange quotes were being
represented; `<<147>>` and `<<148>>`. I just wrote a function to remove those and
BAM! SUCCESS! An aggravating problem squashed.

Also, I wrote a module to wrap calls to `HTTPotion` to keep from calling it directly.
That way, I can easily swap out for another web-scrapper if I decide to down the road.

Lastly, I refactored some function calls to directly pipe into other functions.
I still get in the habit of ye olde way of declaring temp variables. The code is
more "Elixiry" now.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 9: January 15, 2017, Sunday
**Today's Progress:**
Added some tests, modified some modules to use attributes

**Thoughts:**
Long week of bug squashing at work, so I took a break on Saturday from coding.

Finally put in some tests. Maybe one day I'll practice TDD; it's hard to change
how you go about coding, though.

ExUnit is pretty straight forward.

Discovered [module attributes](https://elixirschool.com/lessons/basics/modules/)! For a few modules that I used structs to store
variables acting as constants, I rewrote them to use attributes with simple
functions exposing them. At first I referred to the the attributes directly. The
compiler didn't complain, but one of my tests threw an error. After reading some
documentation I found out that I need some functions to wrap those attributes
if I wanted to call them from outside their modules.

Removing the structs cleaned up my code nicely. The "base" URL and xml file name
for the House could now be stored neatly and concatenated in a separate module.
Also, I no longer had to declare structs for the xquery and xml metadata strings,

I put in a good bit of time today, so it more than makes up for skipping yesterday.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 10: January 16, 2017, Monday
**Today's Progress:**
Moved modules to own files, added a Behaviour to a module.

**Thoughts:**
Slightly cheating here since I did some more coding after I wrote up yesterday's
progress, but actually did some this morning.

I had all my modules in one file, so I moved them to their own. Plus, I added
some comments to the `MsLegis` module.

I added a Behaviour to the `GetXQueryResult` module. In Elixir, a Behaviour
is similar to an interface in Java, C#, or ActionScript; it tells whatever
module includes it that it must implement functions with names, arguments, and
return types specified by that Behaviour.

So why that module? It uses `SweetXml` to return parsed data from xml strings
passed to it; if I create another module that imports a different 3rd party
module, I can help ensure that new module works if it implements that Behaviour.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 11: January 17, 2017, Tuesday

**Today's Progress:**
Started work on parser for member names.

**Thoughts:**
Actually did some TDD today! I created tests for removing periods and commas in
member names (e.g. "Ed Blackmon, Jr." -> "Ed Blackmon Jr") and finding members'
suffixes (e.g. "Sr", "Jr", "III"). Then I created the module and functions needed.

There is simply one string of text from the xml for members' name, so the above
functions will be needed to break part the text.

I did remove the need for an if/else statement in the `ProcessMemberName.get_suffix`
function to handle `nil` when there is no suffix found. I used `to_string` on
the result of the `Enum.find` function; `nil` converts to `""`.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper)

## Day 12: January 21, 2017, Saturday

**Today's Progress:**
Working on putting name in struct

**Thoughts**:
A few days of short sleep and chugging away at work required a few days hiatus.

Working on creating a struct for the member's name. Found `ExConstructor`, a Hex
package that allows easy initializing of a struct with values. I changed
`ProcessMemberName.get_suffix`; instead of passing in the whole name string,
I created another function `is_suffix?` to determine if the last element in the split name
string was in a list of suffixes, then `get_suffix` called this function
and returned the value of the argument if true, and an empty string if not.

**Link to work:** [MsLegis](https://github.com/patrickcarver/Mississippi-Legislature-Data-Slurper) 
