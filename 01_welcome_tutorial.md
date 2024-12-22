# Tutorial: How to get a Census API Key up and running
This is a tutorial on how to get a Census API key and use it. Follow these steps to get your own key. Intended audience is UC Irvine folks, but it should work for anyone.

Estimated time: 10 minutes

<a class="icon-check-plus"></a> **What you'll need**:
* Access to the Internet
* A working email address

## Why?
The U.S. Census Bureau ("Census Bureau") offers some of its public data in machine-readable format via an Application Programming Interface ("**API**"). It is a PITA to get a lot of data. The detailed profiles are very limited.

There are many ways to access Census data with an API key. You could call it through Python, R, thanks to packages like Pygris, tigris, etc. GIS servers and many web platforms use REST API.

Here, we will get a  key and use the website feature to get data. Follow Steps 1-3 below.

Do you already have an API key? Skip to [Step 3. Try using the key](#)

## Step 1. Request

![Screenshot of US Census Bureau website. Request a U.S. Census Data API Key. Form fields for organization Name, email address, and a checkbox for "I agree to the terms of service". Submit button to request key](images/API_request.png)
_Figure 1: Web Form to Request a Census Data API Key_  

1. Go to [Request a U.S. Census Data API Key](https://api.census.gov/data/key_signup.html).
2. Read the [Terms of service](https://www.census.gov/data/developers/about/terms-of-service.html) linked in the last field before the submit button.
	- Summary: When getting information from Census Bureau data, attribute the data whenever you use it and use it in ways that do not violate geoprivacy, etc.
3. Fill out the form
	1. I entered  `University of California, Irvine` for organization
	2. Any email address should work. I used my UCI email address
	3. Check the box to agree. 
4. Press "Request Key".

## Step 2. Activation

After submitting, you should almost immediately receive an email with subject line, "Census Data API Key Request".
![Screenshot of email from the Census Bureau API Team. Subject line, "Census Data API Key request. Email body includes the API key which has been censored, followed by a link to activate the key](images/email_API_key_string.png)
_Figure 2: Screenshot of activation email from Census Bureau_  

I censored the part with my own key, but it a 40-character string of numbers and letters, which I'll refer to henceforth as  `stringofcharactersandnumbers`.

Until you click the link in this email, the key will not work yet!

Click "Click here to activate your key".

If you got this result, then proceed to [Step 3](#step-3-try-using-the-key)

> **Success**
> Your request for a new API key has been successfully submitted. Please check your email. In a few minutes you should receive a message with instructions on how to activate your new key.

But, if you saw this error message... 

> **Error**
> You've  attempted to validate an unknown key. If it has been more than 48 hours since you submitted your request for this API key then the request has been removed from the system. Please request a new key and activate it within 48 hours.

Then, expand [the accordion below](help-i-got-an-error-message) for a possible solution:

### Help, I got an error message!

<details>
    <summary><h4>Click to expand</h4> </summary>
    <p>If you used your UCI email address (or similar institution's address), it might be due to changes made to the activation link via <a href="https://www.oit.uci.edu/services/communication-collaboration/proofpoint/">Proofpoint Email Security</a>. The process is shown in this diagram (note: the result URL is similar to the real output, but this is fake and for demonstration purposes.)</p>

![Sequence diagram of a link to Reddit.com sent to UCI recipient, which is deemed malicious by Proofpoint. Link is rerouted with URL defense and the result is a link with a bunch of extra crap added to it. Example of link to https://www.reddit.com gets 120 characters appended to it](images/proofpoint_emails_process_edited.svg)
_Figure 3: Example sequence of a link getting modified through Proofpoint email security process_  

<p>Thankfully, you can still identify the original link in the mess. It is preceded by and precedes two underscores in a row (?? what security?? ).  Try these steps:</p>
	    <ol><li>Right click the link text "click here to activate your key". Select "Copy link address"</li>
			<li>Paste the URL in a text editor. </li>
			<li>Identify the original URL. It should begin with<code>https://api.census...</code> and end with a string of numbers and letters right before <code>__;!!</code></li>
			<li>Copy this URL segment. Paste it into your browser's address bar. Press enter/go to the page. </li>
			<li>You should see a success message now. Proceed to Step 3. </li>
			<ul><li>If it still doesn't work, then message me (<a href="mailto:tbtruon1@uci.edu" target="_blank" rel="noopener">tbtruon1@uci.edu</a>) for help!</li></ul>
		</ol>
</details>

## Step 3. Try using the key

Now that your API key has been activated, let's see if this works.

One of the easiest ways is by simply using it on URLs, or through the website. When doing a web call, there is no software or program needed. You just specify your request in a URL and get the result, all in your web browser. 

The schematic below breaks down the components of a URL that will use the API through the website. The variable list includes the variable(s) you are requesting. You can include up to 50 variables in a single API query (separated by commas). 

![API URL address components include Census Data API (https://api.census.gov/data/), dataset (2020/dec/dp), query string (?), get function (get=) followed by variable list (NAME, DP1_0001C, for=state:*), including separators (ampersands), ending with your API key at the end](images/API_key_explainer_large.svg)
_Figure 4: Decoding an API URL address: Components of a Census Data API URL query_  

1. Copy this link 
`https://api.census.gov/data/2020/dec/dp?get=NAME&DP1_0001C&for=state:*&key=stringofcharactersandnumbers` and paste into your web browser. 
2. Replace the last part after `key=`  with your API key. Press enter/navigate to the address.
3. You should be directed to a .txt (plain text file), where the first 6 rows looks like this:
	> [["NAME","DP1_0001C","state"],
	> ["Alabama","5024279","01"],
	> ["Alaska","733391","02"],
	> ["Arizona","7151502","04"],
	> ["Arkansas","3011524","05"],
	> ["California","39538223","06"],

Congrats! You just pulled Census data using their API key. 

Let's look at the URL again to take a closer look at the dataset specification and variables:
`https://api.census.gov/data/2020/dec/dp?get=NAME&DP1_0001C&for=state:*&key=stringofcharactersandnumbers`

Table explaining variables
| Part | Phrase | Component  | Description |
|--|--|--|--|
| Specified Dataset | `"2020/dec/dp"` | `2020`|  Year |
| | | `dec` | Decennial Census |
| | | `dp`| Detailed Profile |
| Get? or Variable |  `"NAME"`  | `for=`| is a predicate clause |
| | | `NAME` | provides the name of the geographic area(s) that you are using to limit your search |
| Variable  |`"DP1_0001C"` | `DP1` | Data Profile Table 1 |
| | | `0001C` | 1 is column 1, and C is the third row. Total population count |
| Predicate  | `"for=state*"`| `for=` | is a predicate clause |
| | | `state*` | is a geography, which specifies the geographic area(s) of interest. The asterisk * returns all states. |

Here is what we find: 
-   Data set source specified by `2020/dec/dp` which refers to the 2020 Decennial Census Detailed Profile (hence, "dec" and "dp")
-  In this data set:
	- `NAME` provides the name of the geographic area(s) that you are using to limit your search.
	- `DP1_0001C` variable specifies total population. It is a count (not standardized or normalized). The letter C means we're on the 3rd row of a table containing a bunch of population observations (1 = men, 2 = women, I think)
	- `for=state*` has a couple things going on.
		- `for`  is a predicate clause, which specifies how variables should be filtered or limited (for example, for certain geographic areas).
			- The asterisk here returns all states. If you wanted to limit it to California, you would write 06.
		-  `state` is a geography, which specifies the geographic area(s) of interest.

Verify some numbers like for California.

Now, for the assignment! 

## Resources
* U.S. Census Bureau (July 30 2024). [Census Data API User Guide Website](https://www.census.gov/data/developers/guidance/api-user-guide.html)  or view the [PDF version](https://www.census.gov/content/dam/Census/data/developers/api-user-guide/api-user-guide.pdf). 
*   U.S. Census Bureau (February 2020). [Using the Census Data API With the American Community Survey: What Data Users Need to Know](https://www.census.gov/content/dam/Census/library/publications/2020/acs/acs_api_handbook_2020.pdf),  U.S. Government Printing Office, Washington, DC. 

> Written with [StackEdit](https://stackedit.io/).
