---
layout: post
title: GreaseMonkey close resolved subtasks
lang: en
date: 2014-01-29 23:59
categories:
    - en
summary: Howto close jira subtask from parent ticket?
tags:
- greasemonkey
---

General idea put additional button in jira parent ticket screen(function addButton(...)). This button will have tooltip with all not closed subtasks. If button is clicked then script with rest calls goes one by one trhough all subtask and fetches close transition id(function findTranstionIdForTicket(...)) and then will closeTicket(...).



```
	// ==UserScript==
	// @name        CloseResolvedSubtasks
	// @namespace   JIRA_STUFF
	// @include     https://MY_JIRA_SERVER/browse/*
	// @require http://ajax.googleapis.com/ajax/libs/jquery/1.8.1/jquery.min.js
	// @version     1


	    var $ = unsafeWindow.jQuery;
	    var name = "#closeButtonContainer";  
	    var menuYloc = null;  


		var GM_log = unsafeWindow.console.log;

		function findTranstionIdForTicket(childIssueKey, actionName){
			var transitionId = null;
			$.ajax({
			  type: 'GET',
				url: '/rest/api/2/issue/' + childIssueKey+ '/transitions',
				dataType: 'json',
				contentType: "application/json",
				async:   false,
				success: function(jsonData) {
					//alert(JSON.stringify(jsonData));
					$.each(jsonData["transitions"], function() {
						//alert(this["id"] + this["name"]);
						if(actionName == this["name"]){
							transitionId = this["id"];
							//alert(transitionId);
							return;
						}
					});
				},
				error: function() {
					alert(XMLHttpRequest.responseText);
				}
			});

			return transitionId;
		}

		function closeTicket(childIssueKey, fixVersionArr){
			var transitionId = findTranstionIdForTicket(childIssueKey,"Close Issue");
			requestMap = {
					"update": {
					  "customfield_10793": [ {"set" :  fixVersionArr}]
					 },
					"transition": {  "id":  transitionId }
				}
			console.log("[closeTicket] request: %o",JSON.stringify(requestMap));
			$.ajax({
			  type: 'POST',
				url: '/rest/api/2/issue/' + childIssueKey+ '/transitions',
				dataType: 'json',
				contentType: "application/json",
				async:   false,
				data: JSON.stringify(requestMap),
				success: function(jsonData) {
					console.log("[closeTicket] response: %o",JSON.stringify(jsonData));
				},
				error: function() {
					alert(XMLHttpRequest.responseText);
				}
			});
		}

		function addButton(resolvedTicketMap){
			var fixVersionArr = []
			$('#fixVersions-field > a').each(function(index) {
				var versionEntry = {"name": $(this).text().trim()};
				fixVersionArr.push(versionEntry);
			});
		
			console.log("[addButton] fixVersionArr: %o",fixVersionArr);

			console.log("[addButton] resolvedTicketArr: %o",resolvedTicketArr);
			var button = $( '<div id="closeButtonContainer" style="position:absolute; top:5px;  left:50%;  margin-left:235px; width:200px; z-index:101"> '+
				'<button id="closeButton">Close Resolved</button>'+
				'<div id="closeButtonDetails" style="background:white"/>'+
				'</div>');
			button.click(function() {
				for (var userCd in resolvedTicketMap) {
					var resolvedTicketArr = resolvedTicketMap[userCd];
					for (var i = 0; i < resolvedTicketArr.length; i++){
						childIssueKey = resolvedTicketArr[i];
						closeTicket(childIssueKey, fixVersionArr);
					}
				}
			
				alert("Closed: " + JSON.stringify(resolvedTicketMap));
				//alert($('#header-details-user-fullname').attr('data-username') + " "  +  $('#header-details-user-fullname').text() + resolvedTicketArr);
			});
			button.appendTo("body");

			menuYloc = parseInt($(name).css("top").substring(0,$(name).css("top").indexOf("px")))  
			$(window).scroll(function () {  
				var offset = menuYloc+$(document).scrollTop()+"px";  
				$(name).animate({top:offset},{duration:500,queue:false});  
			});
			$('div#closeButtonContainer').hover(function() {
				$('div#closeButtonDetails').text(
					"Tickets: " + JSON.stringify(resolvedTicketMap) + "; Versions: " + JSON.stringify(fixVersionArr));
			}, function(){
				$('div#closeButtonDetails').text('');
			});

		}





		$(document).ready(function(){

			var resolvedTicketMap = {};
			var currentUser = $('#header-details-user-fullname').attr('data-username').trim();
			//currentUser = "lsucila";
			console.log("[ready]currentUser: %o",currentUser);
			$("td.status").each(function() {
				var status = $(this).text().trim();
				if(status.trim() == "Resolved"){
					assignee = $(this).parent().find("td.assignee").find("a").attr("rel").trim();
					//console.log("[ready] assignee: %o",assignee);
					if(resolvedTicketMap[assignee] == null){
						resolvedTicketMap[assignee] = [];
					}
					resolvedTicketArr = resolvedTicketMap[assignee];
					//if(assignee == currentUser){
					ticket = $(this).parent().find("td.stsummary").find("a").attr("href").trim();
					ticket = ticket.replace(/\/browse\//mg, "");
					resolvedTicketArr.push(ticket);

				}
			});
			if(JSON.stringify(resolvedTicketMap)!='{}'){
				addButton(resolvedTicketMap);
			}



	
	    });//document).ready
	// ==/UserScript==
```
