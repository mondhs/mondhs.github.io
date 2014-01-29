---
layout: post
title: GreaseMonkey hide table rows
lang: en
date: 2014-01-29 23:59
categories:
    - en
summary: Howto close jira subtask from parent ticket?
tags:
- greasemonkey
---

Each month I need to process huge table in a system. It contains many rows that do not belong to jurisdiction. It would be nice if it would be possible extract information using WS or REST. Due there is no such possibility, then User Interface should be tweaked. 

As a proposal was used greasemonkey to remove certain rows: [firefox/addon/greasemonkey](https://addons.mozilla.org/en-US/firefox/addon/greasemonkey/)

This script does not use other libraries like jquery.

### Filter Out

*   Be sure that you install the greasemonkey to firefox.

*   Include the script in greasemonkey. Click on icon to expand menu and select "New user Script":

*   Copy/paste code:

```
	// ==UserScript==
	// @name        My rows only
	// @namespace   MY_STUFF
	// @include     https://MY_SERVER/MY_PATH*
	// @version     1

	//define list of all ids that I am interested in
	var myRowId = ["ID_1","ID_2","ID_3"];

	//transform array to hash map to find id easier
	var allRows = {};
	for (var i = 0; i < myRowId.length; i++) {
		allRows[myRowId[i]]="MY_ID"; 
	}

	//find collection of the rows element where id is
	var xPathRes = document.evaluate('//tr/td[@class="CLASS_NAME_THAT_HAS_ID"]/div/span/a',
			document, null, XPathResult.UNORDERED_NODE_SNAPSHOT_TYPE, null);

	//iterate trhough elements with id and if it is not in myRowId list just hide it
	var i = 0;
	while( (link = xPathRes.snapshotItem(i) ) != null) {
		var complexIdArr =link.innerHTML.split(" "); 
		if(allRows[complexIdArr[0]] == null){
			link.parentNode.parentNode.parentNode.parentNode.style.display="none";
		}
		i++;
	}



	// ==/UserScript==
```

*   You should change xpath `//tr/td[@class="CLASS_NAME_THAT_HAS_ID"]/div/span/a` to rows element that contains ids

*   You should fill your IDs in the format that is provided (in place of \["ID_1","ID_2","ID_3"\];)

*   After refresh you should see only rows that contains  defined id in myRowId array:



