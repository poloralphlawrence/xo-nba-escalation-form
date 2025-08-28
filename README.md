Aside from the HTML code (index.html), an Apps Script Code is needed to record the form submission to a Google Sheet. 
Use the code below in setting up your Apps Script Code. Ensure to change the IDs of the Google Sheet in the code before running.

----------------
function doPost(e) {
  var sheet = SpreadsheetApp.openById('1Jj8wC0YRJKbse9xSJuXmwHIA0kh83htRgr8GMktSU1g').getSheetByName('Escalations'); // Replace with your actual sheet name
  var data = e.parameter;
  var timestamp = new Date();
  
  // Capture all the form fields
  var emailAddress = data.emailAddress || "";  // XO Email
  var TIDField = data.TIDField || "";  // Ticket ID
  var NBAIDField = data.NBAIDField || "";  // NBA ID
  var CIAMIDField = data.CIAMIDField || "";  // CIAM ID
  var errorCode = data.errordropdown || "";  // Error Code
  var devicesText = data.devices || "";  // Devices
  var otherDevices = data.OtherField || "";  // Devices
  var appVersion = data.AVField || "";  // App Version
  var osVersion = data.OSField || "";  // OS Version
  var browser = data.BRField || "";  // Browser
  var location = data.LocField || "";  // Location
  var contentType = data.contentdropdown || "";  // Type of Content
  var specificGameFeed = data.SGField || "";  // Specific Game and Feed
  var purchasePlatform = data.ppdropdown || "";  // Purchase Platform
  var additionalNotes = data.ANField || "";  // Additional Notes
  //var otherDevices = data.OtherField || "";  // Other Devices
  
  var ipAddress = data.ipAddress || "";  // IP Address (from the form)
  
    // Check if headers are present
  if (sheet.getLastRow() === 0) {
    sheet.appendRow([
      'Timestamp', 'Email Address', 'Ticket ID', 'NBA ID', 'CIAM ID', 'Error Code', 'Device','Other Devices','App Version', 'OS Version', 'Browser', 
      'Location', 'Type of Content', 'Specific Game and Feed', 'Purchase Platform', 'Additional Notes', 'Agent IP Address'
    ]);
  }

  // Append the new row with the form data
  sheet.appendRow([
    timestamp, emailAddress, TIDField, NBAIDField, CIAMIDField, errorCode, devicesText, otherDevices, appVersion, osVersion, browser, location,
    contentType, specificGameFeed, purchasePlatform, additionalNotes, ipAddress
  ]);

  /*// Prepare Slack notification
  var slackMessage = { text: 'New form submission received:', 
    attachments: [{ 
        fields: [ 
          {title: 'Timestamp', value: timestamp}, 
          {title: 'Email Address', value: emailAddress}, 
          {title: 'Ticket ID', value: TIDField}, 
          {title: 'NBA ID', value: NBAIDField}, 
          {title: 'CIAM ID', value: CIAMIDField}, 
          {title: 'Error Code', value: errorCode}, 
          {title: 'Devices', value: devicesText}, 
          {title: 'Other Devices', value: otherDevices}, 
          {title: 'App Version', value: appVersion}, 
          {title: 'OS Version', value: osVersion}, 
          {title: 'Browser', value: browser}, 
          {title: 'Location', value: location}, 
          {title: 'Type of Content', value: contentType}, 
          {title: 'Specific Game and Feed', value: specificGameFeed}, 
          {title: 'Purchase Platform', value: purchasePlatform}, 
          {title: 'Additional Notes', value: additionalNotes}, 
          {title: 'IP Address', value: ipAddress} 
          ] 
          }] 
          }; 
          // Send notification to Slack 
          var options = { 
            method: 'post',
            contentType: 'application/json', 
            payload: JSON.stringify(slackMessage) }; 
            
          UrlFetchApp.fetch('https://hooks.slack.com/services/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', options);*/

  return ContentService.createTextOutput('Success. Your submission has been recorded.');
}
