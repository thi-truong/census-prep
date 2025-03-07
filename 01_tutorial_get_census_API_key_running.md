# Tutorial: How to get a Census API Key up and running
By Thi Truong

> **Note**:  If you are viewing this in Canvas, you can also just <a href="https://github.com/thi-truong/census-prep/blob/main/01_tutorial_get_census_API_key_running.md" target="_blank">view the Github page</a> instead.

## Introduction

This is a tutorial on how to get a Census API key and use it with the URL feature. It is written with UCI folks in mind, but it should work for anyone.

 **Why?**

The U.S. Census Bureau offers a lot of public data online. You could view Quick Fact Sheets, use their Data Explorer tool, use file transfer protocol etc. Neverthelesss, gathering raw Census data is often tedious. If it wasn't, [these kinds of fiverr gigs](https://www.fiverr.com/s/EgYBQG9) wouldn't exist. :weary:

Thankfully, there are ways to access the data via an Application Programming Interface ("**API**") key[^A]. Once you have an API key, you can extract raw statistical data in machine-readable format from all sorts of Census Bureau surveys and programs[^C]. You can use it with tools you might already use, like JSON (ArcGIS uses REST APIs), R ([tigris](https://github.com/walkerke/tigris)), Python ([Pygris](https://walker-data.com/pygris/)), the list goes on.

But...do you need to use all these tools to use the API? **No!** You can specify your request as a query string in a URL and get the data that you're looking for, all in your web browser[^B]. If you have ever edited a URL, then you have already practiced this many times before. This is the method we are using today.

**Estimated time**: 10 minutes

**What you'll need**:
- [x] Access to the Internet
- [ ] Mozilla Firefox OR Google Chrome browser
	- According to [this 2020 Census guide](https://www.census.gov/content/dam/Census/library/publications/2020/acs/acs_api_handbook_2020.pdf), these two browsers provide functionality to view the results from API queries. Other users may not be able view.
- [ ] A working email address

**What you'll do**:
1. Request Census API Key _(Already have a working API key? skip to [Step 3](#step-3-use-the-key))_
2. Activate Census API Key
3. Use Census API key in a query to get data
4. Complete an assignment based on the query results

## Step 1. Request a key

![Screenshot of US Census Bureau website. Request a U.S. Census Data API Key. Form fields for organization Name, email address, and a checkbox for "I agree to the terms of service". Submit button to request key](images/API_request.png)
_Figure 1: Web Form to Request a Census Data API Key_  

1. Go to [Request a U.S. Census Data API Key](https://api.census.gov/data/key_signup.html).
2. Read the [Terms of service](https://www.census.gov/data/developers/about/terms-of-service.html) linked in the last field before the submit button.
3. Fill out the form
	1. I entered  `University of California, Irvine` for organization
	2. Any email address should work. I used my UCI email address
	3. Check the box to agree. 
4. Press "Request Key".

You should see a success message, "Your request for a new API key has been successfully submitted. Please check your email. In a few minutes you should receive a message with instructions on how to activate your new key."

After submitting, you should almost immediately receive an email with subject line, "Census Data API Key Request".

## Step 2. Activate the key

![Screenshot of email from the Census Bureau API Team. Subject line, "Census Data API Key request. Email body includes the API key which has been censored, followed by a link to activate the key](images/email_API_key_string.png)
_Figure 2: Screenshot of activation email from Census Bureau. The "Have Fun" sign off is...a choice._  

I censored the part with my own key, but it a 40-character string of numbers and letters, which I'll refer to henceforth as  `stringofcharactersandnumbers`.

Until you click the link in this email, the key will not work yet!

Click "Click here to activate your key".

Did you get a success message? Congrats! Proceed to Step 3.

### If you got an error message...

If you got an error message saying something like this...
> You've attempted to validate an unknown key. If it has been more than 48 hours since you submitted your request for this API key then the request has been removed from the system. Please request a new key and activate it within 48 hours.

Then, there might be a possible explanation for the error related to your institutional email settings.

<p>If you used your UCI email address (or similar institution's address), the problem might be the activation link was due to <a href="https://www.oit.uci.edu/services/communication-collaboration/proofpoint/">Proofpoint</a>. The process is shown in Fig. 3, where an example original address to Reddit is redirected to a new address. </p>

<figure>
<img src="images/proofpoint_emails_process_edited.svg" alt="Sequence diagram of a link to Reddit.com sent to UCI recipient, which is deemed malicious by Proofpoint. Link is rerouted with URL defense and the result is a link with a bunch of extra crap added to it. Example of link to https://www.reddit.com gets 120 characters appended to it"/><br/>
 <figcaption><em>Figure 3: Example sequence of a link getting modified through Proofpoint email security process. Note: the result URL is similar to the real output, but this is fake and for demonstration purposes.</em> </figcaption></figure><br/><br/>
 
<p> However, even you, the recipient without the original link, you <em>can</em> still identify the original link within the mess upon a closer look. So, try to apply the observation to the link sent to your email:</p>
<ol>
	<li>Right click the link text "click here to activate your key". Select "Copy link address"</li>
	<li>Paste the URL in a text editor and assess: If the address been modified, you may be able to find the original URL, which should begin with <code>https....</code> and end with a string of numbers and letters that matches the key in your email (before <code>__;!!</code>). Can you identify it?
		<ul>
			<li>Yes &rarr; Highlight and copy the original URL. Paste into your browser's address bar and go. You should see the success message now. Proceed to next step.</li>
			<li>No &rarr; Email/Slack me for help. Make sure to include what browser/version you are using. </li>
		</ul>
</ol>

## Step 3. Use the key

:warning: Keep your API key secure. It is best to make sure it is kept securely and treat it like a password. If you share your code or a result with someone, make sure to delete the key if it is present, and replace with a placeholder like `[your-key-here]`, etc. Remember the Terms of Service when getting information from Census Bureau data-- attribute the data whenever you use it and do not use the data to violate geoprivacy.

Now that your API key has been activated, let's test out a query:

1. Copy this link 
`https://api.census.gov/data/2020/dec/dp?get=NAME&DP1_0001C&for=state:*&key=stringofcharactersandnumbers`
2. Paste the link into your web browser, but do not press enter/go.
3. Replace `stringofcharactersandnumbers` with your API key. Make sure not to delete too much of the URL, e.g. `&key=`.
4. Press enter/navigate to the address. You should be directed to plain text formatted data.

### If it didn't work...

If you activated the key successfully but you receive an error message like "Invalid Key" or the URL doesn't work at all...
- Are you speedrunning through this tutorial? Wait a few more minutes, and then try the query again. There is a little bit of processing time needed before the query works.
- If more than a few minutes have passed, then I recommend trying to go through the process again with the same email address. If you still run into issues, try requesting the key with a different email address (preferably from a different domain as the first, in case that is an issue).

### Check your work

Below are the first 6 printed rows of the query result for `get=NAME&DP1_0001C&for=state:*`:

```
[["NAME","DP1_0001C","state"],
["Alabama","5024279","01"],
["Alaska","733391","02"],
["Arizona","7151502","04"],
["Arkansas","3011524","05"],
["California","39538223","06"],
```

Does your result match this?
- If **Yes**, then congrats! You just pulled Census data with the API key. Proceed with the explanation and assignment.
- If **No**, then reach out to me and we can try to troubleshoot. Do not include the full URL with API key when contacting me.

### What did we just do?

We just printed results for **Total Population Count** for each U.S. State in the 2020 Decennial Census. 

Let's take a closer look at the dataset specification and variables to see if we can decode this information just from the URL. 

The schematic below breaks down the components of the API URL query. The variable list includes the variable(s) you are requesting. 

![API URL address components include Census Data API (https://api.census.gov/data/), dataset (2020/dec/dp), query string (?), get function (get=) followed by variable list (NAME, DP1_0001C, for=state:*), including separators (ampersands), ending with your API key at the end](images/API_key_explainer_edit.svg)

_Figure 4: Decoding an API URL address: Components of a Census Data API URL query_  

Table explaining variables and components to build the API request, step-by-step:

| Part | Phrase | Component | Description |
|--|--|--|--|
| Specified Dataset | `"2020/dec/dp"` | `2020`| Year |
| | | `dec` | Decennial Census |
| | | `dp`| Data Profile |
| Query/Variables | `"NAME"` | `for=`| A predicate clause, which specifies how variables should be filtered or limited |
| | | `NAME` | Provides the name of the geographic area(s) that you are using to limit your search |
| Query/Variables  |`"DP1_0001C"` | `DP1` | Data Profile Table 1 |
| | | `0001C` | 001 is column 1. Letter C means we're on the 3rd row of a table containing many population observations. In total, this variable specifies total population count |
| Query/Variables | `"for=state*"`| `for=` |  A predicate clause, which specifies how variables should be filtered or limited |
| | | `state*` | A geography, which specifies the geographic area(s) of interest. The asterisk * returns all states. If you wanted to limit it to California, you would write 06. |

You can view the [full list of variables available through this specific survey](https://api.census.gov/data/2020/dec/dp/variables.html). The 11th row shows the variable `DP1_0001C` we just used. I admit this kind of page can be pretty overwhelming to navigate. However, once you are familiar with the different data products (e.g. Detailed Profile), searching for variables is pretty quick.

## Closing out

Congratulations on finding Census data results with the API key via URL query! Let's reflect on some of the advantages and disadvantages of using this method to gather census data.

:heavy_check_mark: Pros of API Query URL method:
- You can search within exact years, data products, for the exact variables and geographies that you're looking for
- There is no need to store any data or make intermediate products to narrow down to the results you want
- If you retain the URL query, it is a reproducible data search (in contrast, for example, with selecting a bunch of variables in different dropdown menus or search tools)
- You can access way more data than what you can find on data.census.gov, and you don't have to use the file transfer protocol (FTP) either.

:x: Cons of API Query URL method:
- Have to familiarize oneself with available data products, years, variables, available geographies ahead of time before doing the search
- Need another step to save the data to a format for another use
- More of a manual, tedious process, compared to other tools which do this work behind-the-scenes

## Assignment Part 1

(These instructions are repeated on the [Canvas Week 6 Assignment page](https://canvas.eee.uci.edu/courses/66109/assignments/1538937) under heading "Part 1")

**Background**:
- In Tutorial [Step 3](#step-3-use-the-key), we performed a search for the total population count with the `DP1_0001C` variable, which is described as `Count!!SEX AND AGE!!Total population` in the [full list of variables](https://api.census.gov/data/2020/dec/dp/variables.html) for the 2020 Decennial Census Data Profile.
- For this assignment, you will use your API key and the same URL query method to return results for a different variable for all states in the same dataset (2020 Decennial Census Detailed Profile).

**Task**:
- Find the percent of the population under 5 years old for all states, based on the 2020 Decennial Census results.
- Submit a text entry of the first six rows of results (up to California).
- *Hint: Find the appropriate variable in the [full list](https://api.census.gov/data/2020/dec/dp/variables.html). It should be not very many rows away from `DP1_0001C`.* 

---

[^A]: U.S. Census Bureau (July 30 2024). [Census Data API User Guide Website](https://www.census.gov/data/developers/guidance/api-user-guide.html)  or view the [PDF version](https://www.census.gov/content/dam/Census/data/developers/api-user-guide/api-user-guide.pdf). 
[^B]:  U.S. Census Bureau (February 2020). [Using the Census Data API With the American Community Survey: What Data Users Need to Know](https://www.census.gov/content/dam/Census/library/publications/2020/acs/acs_api_handbook_2020.pdf),  U.S. Government Printing Office, Washington, DC. 
[^C]: [Transcript, Demystifying the Census API Transcript](https://www2.census.gov/about/training-workshops/2020/2020-07-22-cedcsi-transcript.pdf) (pdf). 22 July 2020.
[^D]:  U.S. Census Bureau (December 2024). [Census ACS Data Table IDs explained](https://www.census.gov/programs-surveys/acs/data/data-tables/table-ids-explained.html).

This is a one page course template was made with [Docsify-This.net](https://docsify-this.net/#/). Markdown draft in [StackEdit](https://stackedit.io/).

Tutorial by Thi Truong is licensed under [CC BY 4.0](http://creativecommons.org/licenses/by/4.0).
![CC BY](images/cc_by.png)

Last updated March 7, 2025. Incorporated revisions based on feedback from Tatiana Flores, Victoria Nguyen, and Robert Garcia.
