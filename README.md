# Culture Works Tone Analyzer

## Repository Objectives

Integration the Webapp and Chrome extension with the REST APIs

## Project background

This project will create a tool that allows people to understand his or her tone in written communications.
It will aggregate specific communications and provide the user with a tone profile dashboard, which may also be exported to PDF.
The tone profile will give individuals awareness of how he or she typically communicates with others.
The tone is analyzed by sending the written text through the Watson Tone Analyzer APIs, then it is aggregated to present a holistic score per person. Additionally, the tool must be able to present a score for single written communications as they are drafted. 

## Individual requirements

Please refer to the REST API spec to learn about the API: http://www.topcoder.com/challenge-details/30073159/?type=develop&noncache=true

1. Integrate the Webapp with the backend REST API
After the user is logged-in, it should save the jwtToken into the cookie, which will be used by the Chrome Extension. 
New Document Page
 - upload button - call the ``` /tone/profile/file ``` API to upload the file and get the analysis result. Files with txt, doc and docx formats are supported, and the file content should be displayed in the editor on the page, with the analysis results displayed on the right. 
text in editor - call the ```/tone/profile/text``` API to analyze the text in editor, and show the analysis results on the right. 
For both cases, user can click the sentence tone to highlight the corresponding sentences in the editor
- Tone Profile Page
call the ```/tone/profile``` to get the user's tone profile.  Display different content depending upon the profile status. 
none - display a static message in-progress - display the progress bar, and you should poll the api to get new progress and update the page, until the scanning is completed or failed.  If user clicks cancel-scan link, the /tone/profile/cancelScan API should be called to cancel the api, then you should call the /tone/profile again to display the results properly. 

completed - display the full profile
failed - display a static message
- Scan button
the link should be disabled if the scanning is in-progress. The text should be "rescan" if the status is completed or failed, and should be "scan" by default. 
when user clicks the button, you should call the /emailProvider/oauth API, if the response has a "redirect" field, it means the email-provider is not authorized. You need to open the URL in redirect in a current tab to perform the authorization; otherwise call the /tone/profile/scan endpoint to do the scanning. Then you should poll the /tone/profile endpoint to update the scanning progress
- Registration Email Verification Page - this is a new page, the page should look like the current forgot-password page (except it won't have the email-input and cancel button, and the text should be changed to something like: "Your email is verified, please click the button below to login" or some error message if the token is invalid, and the Done button should be "Continue to Login". Its URL is used in the forgot-password email. 
- Reset password - the button on the reset-password page should be "Done". if reset password from user-settings page, click it to go to the settings page. This page will also be used in the forgot-password email, click it to reset the forgotten password, and move to a new reset-password-successful page if it's successful. 
- Reset Forgot Password Successful Page - this is a new page used above. this is a new page, the page should look like the current forgot-password page (except it won't have the email-input and cancel button, and the text should be changed to something like: "Your password is reset successfully, please click the button below to login" or some error message if the token is invalid, and the Done button should be "Continue to Login".
- Authorization Successful Page - this is a new page. the page should look like the current forgot-password page (except it won't have the email-input and cancel button and done button, and the text should be changed to "We are authorized successfully to scan your emails, and the scanning will be started immediately". This page should call the /tone/profile/scan and then redirect to the tone-profile page to show the progress.  
- Authorization Error Page - this is a new page. the page should look like the current forgot-password page (except it won't have the email-input and cancel button, and the text should be changed to "We are not authorized to scan your emails or some error occurs during the authorization, please retry later.". The Done button should move user to the tone-profile page. 
Login/Register/Settings/ForgotPassword/ResetPassword pages - they should be very intuitive

2. Integrate the Chrome Extension with the backend REST API
The extension uses the cookie created from the webapp to determine if the user is logged or not. Make sure you are using the jwtToken saved by the webapp after logged-in
scan button
call the /tone/profile/status endpoint when the popup is loaded. The text should be "rescan" if the status is completed or failed, and should be "scan" by default.
when user clicks the button, if the scanning is in-progress, simply open the tone-profile page from webapp; otherwise, call the /emailProvider/oauth api and do the same stuff as the webapp.

3. Create detailed Azure deployment instructions for webapp and backend. Note that you need to use Azure SendGrid to send the emails. 

Note that you may have to update the back-end if it's really necessary, and please get confirmation first if you need to.
And the input validation and back-end API error should be handled properly in webapp and chrome extension. 

 

