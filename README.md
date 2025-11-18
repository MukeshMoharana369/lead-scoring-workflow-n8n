
First I start with the webhook, because I don't have Apollo API.
So I use webhook as a replacement of Apollo.
Whenever I paste the mock data and run the workflow, it works like a new lead is coming from Apollo.

After that I use the Set node.
In the Set node I only take the important fields like first name, last name, email, title, company, industry and date.
Because Apollo gives a lot of extra data, so I remove everything else and keep only the useful fields.

Then I use the Code node for scoring.
In the Code node I put the scoring logic like:

If the title is CEO, Founder, Owner → give 15 points

If the title is VP, Vice President, Director → give 10 points

If Manager or Head of → give 5 points

Else 0 points

And same for industries:

Insurance, InsurTech → 20 points

Logistics, Transportation, Freight, Shipping → 15 points

Private Equity, PE Portfolio, PE-backed → 15 points

Manufacturing, Industrial → 10 points

Others 0 points

The Code node then gives the result like titleScore, industryScore, totalScore and tier (hot, warm, standard).

After scoring I use a Switch node.
In the Switch node I check the tier.
If the tier is hot then it goes to the hot branch.
If not hot, then it goes to next part where I use an IF node.
In the IF node I check if the score is greater than or equal to 20.
If yes then it is warm.
If no then it is standard.

Then in each branch I store the leads separately.
For hot leads I send a Slack alert and also store the data in the Hot Google Sheet.
For warm leads I only store them in the Warm Google Sheet.
For standard leads I store them in the Standard Google Sheet.
Slack alert is only for hot leads because they are important




My Improvement what I think here
Right now hot leads get an instant Slack alert, but warm leads don't get any notification.
Warm leads are not urgent, but still important, so they should not be ignored for the whole day.

So the improvement I want to add is:

Daily Warm Lead Report

Every warm lead goes into the Warm Google Sheet.

At the end of the day (one fixed time), n8n will collect all the warm leads of that day from the sheet.

Then n8n will create a simple list/report of these warm leads.

After that, n8n will send that report once per day to the team on Slack or Email.

This way:

The team doesn't get spammed instantly like Hot leads.

But they still get all warm leads together in one clean summary.

Warm leads don't get missed or forgotten.

This is the improvement I want to add.
