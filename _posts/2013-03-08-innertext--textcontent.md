---
layout: post
title: "innerText !== textContent"
description: "Wherein I learn that innerText and textContent are quite different"
category: blog
tags: [javascript]
---
{% include JB/setup %}

I am currently tasked with a project that requires me to use <code>contentEditable</code> divs with the ability to save out the content of the div as a plain text string, but maintaining the line breaks.

	<div id="edit-box">
		I am a list of things
		<br>
		<br>
		That are in a contenteditable div
		<br>
		<br>
		Excellent!
	</div>

I develop in Chrome, and I used <code>document.getElementById('edit-box').innerText</code>, which on the div above gives me the following output

	I am a list of things

	That are in a contenteditable div

	Excellent!

Everything was fine.  I checked it in IE, again, everything was fine.  Then I tested Firefox, things were no longer ok.  Firefox doesn't support <code>innerText</code> of divs.  For Firefox, <code>textContent</code> is the attribute to use.  But this does not maintain line breaks like <code>innerText</code> and I end up with this

	I am a list of thingsThat are in a contenteditable divExcellent!

After messing around with <code>textContent</code> for a bit, I realized it just wasn't the way to go.

So now I have 1 problem, time to make it 2, I need a regex (or 2), and <code>innerHtml</code>.


	var nl = /<br>/g, // use this one to find all <br>'s' and replace them with \n
		html = /<(?:.|\n)*?>/gm, // used to strip out the rest of the html
		content = document.getElementById('edit-box').innerHTML
					.replace(nl, "\n")
					.replace(html, "");


When I run this code I end up with the exact same output as Chrome gave me with <code>innerText</code>.

So putting it all together I ended up with this


	var el = document.getElementById('edit-box'),
		nl = /<br>/g, // use this one to find all <br>'s' and replace them with \n
		html = /<(?:.|\n)*?>/gm, // used to strip out the rest of the html
		content;

	if (document.body.innerText) { // Chrome and IE here
		content = el.innerText;
	} else { // Firefox here
		content = document.getElementById('edit-box').innerHTML
					.replace(nl, "\n")
					.replace(html, "");
	}


