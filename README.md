# sharepoint-get-list-item-attachments
/*
 * JQuery WYSIWYG Web Form Designer
 * Copyright 2015 AgilePoint Inc
 */

/* Add your JS code Here (Press Ctrl+Space keys for intellisense) */

eFormEvents.onFormLoadComplete = function () {
getListItemAttachments();
}

function getListItemAttachments(){
//use _spPageContextInfo.webAbsoluteUrl for dynamic site URL
var listtitle = "Entry";
var itemID = $("#ListItemID").val();
var currentSiteURLPath = "";
var webUrl = "";
if(itemID){
		try {
  webUrl = _spPageContextInfo.webAbsoluteUrl + "/_api/web/lists/GetByTitle('" + listtitle + "')/items('" + itemID + "')/AttachmentFiles";
}
catch(err) {
  currentSiteURLPath = window.location.href.match(/^.*(?=(\/_layouts))/g);
  if(currentSiteURLPath.length > 0){
	currentSiteURLPath = currentSiteURLPath[0];  
  }
  //console.log(currentSiteURLPath);
  webUrl = currentSiteURLPath + "/_api/web/lists/GetByTitle('" + listtitle + "')/items('" + itemID + "')/AttachmentFiles";
}
/* console.log(webUrl); */
$.ajax({  
        url:webUrl,  
        type: 'GET',    
  async: false,  
        headers:    
  { 
  "Accept": "application/json;odata=verbose"
  },  
  cache: false,  
 success: function(data){ 
  var results;
  var attachments = "";
   results = data.d.results;
   if(results.length > 0){
	   for (i = 0; i < results.length; i++){
	var attachmentURL = "<a href='"+ results[i].ServerRelativeUrl +"' target='_blank'>" + results[i].FileName + "</a>";
	attachments = attachments + attachmentURL + "<br>";
   }   
   }
   else{
	 attachments = "No attachments found.";  
   }
   document.getElementById("AttachmentsHTMLControl").innerHTML = attachments;
   },
 error: function(xhr, status, error){
	 document.getElementById("AttachmentsHTMLControl").innerHTML = "Error while fetching attachments";
   }   
}); 	
}
}
