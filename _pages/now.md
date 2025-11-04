---
title:
layout: default
permalink: /now/
published: true
---

This is a <a href="https://nownownow.com/about">now page</a>. It was last updated on 4th November 2025.

<p style="margin-top: 0.15em; margin-bottom: 0.15em">
	<ul>
		<li><strong><em>reading:</em></strong> <a href="https://www.timminchin.com/you-dont-have-to-have-a-dream/">You Don't Have to Have a Dream</a> by Tim Minchin </li>
		<li><strong><em>watching:</em></strong> <a href="https://brennanleemulligan.com/fantasy-high/">Fantasy High</a> by Dimension 20</li>
		<li><strong><em>listening:</em></strong> <a href="https://en.wikipedia.org/wiki/From_the_Pyre">From the Pyre</a> by The Last Dinner Party</li>
		<li><strong><em>learning:</em></strong> <a href="https://docs.ansible.com">Ansible</a>, <a href="https://github.com/kimdre/doco-cd">dococd</a>, and <a href="https://www.youtube.com/watch?v=-0Ao4t_fe0I">Cerice</a></li>
		<li><strong><em>writing:</em></strong> A metal album, exploring feelings around transitioning </li>
		<li><strong><em>playing:</em></strong> <a href="https://en.wikipedia.org/wiki/Prince_of_Persia:_The_Lost_Crown">Prince of Persia: The Lost Crown</a></li>
	</ul>
</p>

<p style="margin-top: 0.15em; margin-bottom: 0.75em; text-indent: 4ch;">
		Time seems so elastic at the moment. There are days where I feel I barely get to stop and look around, much less pick up a guitar and do some writing. That said, I have really been enjoying the process of writing my own music again. I'm getting a little stuck on a few ideas, and I think I'm learning that my approach to writing doesn't always make the inclusion of lyrics/vocals very easy. So there's a whole lot of changing and editing and tweaking to do. Then, of course, there is the fact that I am not super at lyric-writing. I do my best, but it is definitely not where my forte lies. In any event, I quipped to a friend online recently that if I keep up the current pace of writing, I should have something to share with the world in around 2030 or so! That said, it's been so fun to pick up a guitar and write without pressure again. That's tremendously freeing.
</p>
<p style="margin-top: 0.15em; margin-bottom: 0.75em; text-indent: 4ch;">
		I have been steadily trying to overhaul my personal productivity systems, perhaps to my detriment. I find that my to-do list balloons and builds up with absurd ease, and if I am not careful I can enter a state of total paralysis and panic. On the other hand, there are some systems and routines that I find help: weekly and monthly reviews of the past period of time, with a view to better structuring the coming week or month. Carving out some time after work to do some guided stretches; increasing my flexibility and giving me time to myself between work and parenting for the evening. There's still a lot of tweaking and changing and restructuring that I want to do, and I want to try to reduce my load rather than increase it. Progress is slow, but steady, and there is always the "fuck it, I'm never actually going to do this" of it all.
</p>
<p style="margin-top: 0.15em; text-indent: 4ch;">
		The pace of wedding planning is picking up, and it's nice to be making some headway with our list of jobs. I'm in the process of building our wedding website, and the draft version we put together last night is looking pretty good! Once it's been hosted, we'll finally be able to get the save-the-dates out (which currently point to this non-existent website). We've sorted out venues, photographers, celebrants, handfasting cords, and more. It's feeling really good to get into the swing of organising the wedding, and the principles we figured out with each other at the beginning are holding out very well. I recently saw an <a href="https://www.instagram.com/reel/DQaM1e9EgbW">Instagram Reel</a> which talked about love not being a case of two becoming one, but one becoming two, and two becoming more. The more I plan a wedding with B—, the more connected I feel. This isn't about becoming one (and my vows will reflect that I think), but about us becoming the truest versions of ourselves, where we are allowed to flourish in our strengths, and be supported where we need. It's a heady feeling.		
</p>

<h1>Ongoing Projects</h1>
<div class="ProjectContainer" style="padding-top: 0.5em">

	<div class="gallery">


  {% for project in site.projects %}

  {% if project.redirect %}
  <div class="projectTile">
          <a href="{{ project.redirect }}" target="_blank">
          <span>
              <h2>{{ project.title }}</h2>
              <br/>
              <p>{{ project.description }}</p>
          </span>
          </a>
  </div>

  {% else %}

  <div class="projectTile">
          <a href="{{ project.url | prepend: site.baseurl | prepend: site.url }}">
          <span>
              <h2>{{ project.title }}</h2>
              <br/>
              <p>{{ project.description }}</p>
          </span>
          </a>
  </div>

  {% endif %}

  {% endfor %}

	</div>

</div>
