# Spin-Rewriter-API-for-Google-Apps-Script
Since Spin Rewriter only provides Python and PHP examples on their website I thought I would share my JavaScript Google Apps Script code for working with Spin Rewriter API!
## Getting Started
First you will need to have your Spin Rewriter(SR) account information.  Your SR login email and your SR API Key.  API key can be found [here](https://www.spinrewriter.com/cp-api) once you are logged in.
## Making API Call
Here is how you make a call on the SR API.

```javascript
function uniqueSpin(text) {
  //var text = "YOU_ARTICLE_FOR_SR";
  var email = "YOUR_SR_EMAIL";
  var key = "YOU_SR_API_KEY";
  var url = "https://www.spinrewriter.com/action/api";
  var payload = {
    email_address: email,
    api_key: key,
    action: "unique_variation",
    text: text
  };
  var options = {
    'method' : 'post',
    'payload' : payload
  };
  try {
    var responce = UrlFetchApp.fetch(url, options);
    //7 seconds delay between api calls 7000 miliseconds = 7 seconds!
    Utilities.sleep(7000);
    responce = JSON.parse(responce.getContentText());
    //responce.responce is the spun text here
    return responce.response;
  } catch (e) {
    Logger.log(e);
  }
}
```
Refer to [this link](https://www.spinrewriter.com/cp-api-documentation) for action options.
- api_quota: returns the number of made and remaining API calls for the 24-hour period
- text_with_spintax: returns the processed spun text with spintax
- unique_variation: returns a unique variation of processed given text
- unique_variation_from_spintax: returns a unique variation of already spun text

As you can see it is a really simple api with no 'head' variable necessary.
### Additional Variables that can be included in payload include:

- protected_terms: A list of keywords and key phrases that you do NOT want to spin. One term per line, i.e. terms are separated by the "\n" (newline) character.
- auto_protected_terms: Should Spin Rewriter automatically protect all Capitalized Words except for those in the title of your original text?
- confidence_level: The confidence level of the One-Click Rewrite process. (low, medium, high)
- nested_spintax: Should Spin Rewriter also spin single words inside already spun phrases?
- auto_sentences: Should Spin Rewriter spin complete sentences?
- auto_paragraphs: Should Spin Rewriter spin entire paragraphs?
- auto_new_paragraphs: Should Spin Rewriter automatically write additional paragraphs on its own?
- auto_sentence_trees: Should Spin Rewriter automatically change the entire structure of phrases and sentences?
- use_only_synonyms: Should Spin Rewriter use only synonyms of the original words instead of the original words themselves?
- reorder_paragraphs: Should Spin Rewriter intelligently randomize the order of paragraphs and unordered lists when generating spun text?
- spintax_format: The spintax format of the returned spun text.
  - {|}: the {first option|second option} spintax used (default setting)
  - {~}: the {first option~second option} spintax used
  - \[|\]: the \[first option|second option\] spintax used
  - \[spin\]: the \[spin\]first option|second option\[/spin\] spintax used
  - #SPIN: the {#SPIN: first option || second option #} spintax used
## Dealing with Long Strings
Take note that there has to be a 7 seconds delay between api calls.  Also for spinning text you can only spin up to 4000 words at a time so I have implemented this workaround for really large texts:
```javascript
var massiveString = "YOUR_LARGE_TEXT";
var words = massiveString.split(" ");
var maxWords = 4000;
var spunString = "";
for (var i = 0; i < (words.length / maxWords); i++) {
  var start = i * maxWords;
  var stop = (i + 1) * maxWords;
  var partString = words.slice(start, stop).join(" ");
  spunString += uniqueSpin(partString);
}
```
This breaks down your large request into several small requests.
## Conclusion
Spin Rewriter is a really simple api allowing you to interact with a really powerful Natural Language Processing Technology! :simple_smile:
