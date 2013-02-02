---
layout: post
title: "HTML5 datalist"
published: true
category: blog
tags: []
comments: false
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
First I need to get our data in a usable form, namely, with contact and company name as a single field:

	SELECT CONCAT(company_contact, ' | ', company_name) as info, 
	       company_contact, company_name, company_address
	FROM companies
	WHERE company_contact LIKE $chars.'%'

Now that I have that, I need a clean way to get this.  A fairly simple function should do the trick:

	/* 
	I pass in the element that I want my datalist to be attached to
	The url for the ajax call
	And the returned dbField name I want to show as the text in the datalist
	*/
	function showDropdown(el, url, dbField) {
		// create the datalist object
		var datalist = $('<datalist id="drops"/>');

		// make the ajax get passing in the value of the element,
		// return the data as drops
		$.get(url, {'chars': el.val()}, function(drops) {
			for (var drop in drops) {
				var option = $(document.createElement('option')),
				    j = 0;

				// skip any blank fields returned from the db
				if (typeof drops[drop][dbField] === "undefined") {
					continue;
				}

				// set the value of the option to the dbField that
				// was passed in
				option.attr('value', drops[drop][dbField]);

				// for all other items assign them to data-attributes
				// of the option
				for (var item in drops[drop]) {
					option.attr('data-'+item, drops[drop][item]);
				}

				datalist.append(option);
			}
			// now attach it all to the element
			el.attr('list', 'drops').siblings('datalist').remove();
			el.after(datalist);
		});
	}

All that's left is setting up the input event on the element that gets the datalist

	$('#company-contact').on('input', function(e) {
		// I have this here so that showDropdown() is only called
		// after 3 or more characters are entered
		if ($(this).val().length > 2) {
			var _this = this,
			    list = $('#drops').children();

			showDropdown($(_this), '/get_customer_info', 'info');

			// loop through each list item
			list.each(function(index, el) {
				var _el = $(el),
				    companyName = _el.attr('data-company_name'),
				    companyAddress = _el.attr('data-company_address'),

				// I am looking for the list item that matches exactly
				// what is in the company-contact input
				// If these match it is a pretty good indicator that
				// the user selected something in the datalist
				// Or just typed out exactly what was in the datalist
				if (_this.value == _el.val()) {
					$('#company-name').val(companyName);
					$('#company-address').val(companyAddress);
				}
			});		
		}
	});

And that's all she wrote.  It's not the cleanest of solutions, but it seems necessary due to datalists not throwing any events.  If there is a better way to do this, I would love to see it.  This is just what I was able to come up with.