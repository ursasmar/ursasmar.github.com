---
layout: post
title: "HTML5 datalist"
published: true
category: blog
tags: []
---

{% include JB/setup %}

Getting more than basic behavior from a datalist is a bit of a pain.  Especially since it is very limited in what it can, and it seems to throw no events.

#### The Situation
I have a form that needs to pull contact information.  User enters a name, and company name and address are populated automatically.

#### The Problem
The issue here is that one contact can have multiple companies that they work for.  I needed a way to show that user Bob works for both Acme and Ajax, and fill in the correct company information based on which Bob the user selects. 

#### The Idea
Display the contact and company info as one in the value of the datalist option. Add in the other data I need as data-attributes on the options.  When a user selects an item from the drop down, grab the data-attributes, and fill in the fields.

#### The Solution
First we need to get our data in a usable form, namely, with contact and company name as a single field:

<pre class="prettyprint lang-sql">
SELECT CONCAT(company_contact, ' | ', company_name) as info, 
       company_contact, company_name, company_address
FROM companies
WHERE company_contact LIKE $chars.'%'
</pre>



<pre class="prettyprint lang-js">
$('#el').on('input', function() {
	console.log('hi there!');
});
</pre>