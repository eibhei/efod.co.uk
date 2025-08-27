---
title:
layout: default
permalink: /now/
published: true
---

This is a <a href="https://nownownow.com/about">now page</a>. It was last updated on 27th August 2025.

<p style="margin-top: 0.15em; margin-bottom: 0.15em">
	<ul>
		<li><strong><em>reading:</em></strong> <a href="https://en.wikipedia.org/wiki/Life_and_Death%3A_Twilight_Reimagined">Life and Death: Twilight Reimagined</a> by Stephanie Meyer </li>
		<li><strong><em>watching:</em></strong> <a href="https://brennanleemulligan.com/fantasy-high/">Fantasy High</a> by Dimension 20</li>
		<li><strong><em>listening:</em></strong> to <a href="https://en.wikipedia.org/wiki/Random_Hand#Random_Hand">Random Hand</a> by Random Hand</li>
		<li><strong><em>learning:</em></strong> <a href="https://docs.ansible.com">Ansible</a>, <a href="https://github.com/kimdre/doco-cd">dococd</a>, and <a href="https://www.youtube.com/watch?v=-0Ao4t_fe0I">Cerice</a></li>
		<li><strong><em>writing:</em></strong> New music</li>
		<li><strong><em>playing:</em></strong> <a href="https://en.wikipedia.org/wiki/Root_%28board_game%29">Root</a></li>
	</ul>
</p>

<p style="margin-top: 0.15em; margin-bottom: 0.75em; text-indent: 4ch;">
		The Year of Clearing the Decks. I <a href="https://www.facebook.com/eyupmaiden/posts/pfbid02hKhVmCxfwVXLcZLN36R8iwxwvoZG7kZ9zJoqdPo5hEHD7P9xNzy6bSaJbH7iCNp8l">retired from Ey Up Maiden</a> at the start of August, and I've wound down a few other regular bits and bobs. What has been nice is things organically arising to take up some more of the free time. I'll be playing in a game of Mothership in September, and I've been able to put more time into reading and writing music. There are still assumptions, systems, and things in my life which I would like to wrap up as well, but the pace remains steady and slow. It's a struggle sometimes to remember that I don't need to just gut everything all at once, and smaller, iterative changes can be just as much a clearing of the decks.
</p>
<p style="margin-top: 0.15em; margin-bottom: 0.75em; text-indent: 4ch;">
    Something that isn't going away, and may even be getting more complex and involved are my various self-managed digital systems. I recently removed comments from my blog, as the Bluesky API was only intermittently available. My intention is to switch to using Mastodon for this purpose, but it's not necessarily going to be an easy switch to flip. Looking at what's out there, I suspect I might be about to dust off my coding skills to have exactly what I want. Elsewhere, I've been rationalising my homelab and Docker situations, and am in the process of introducing a CI/CD pipeline for my Docker Compose environment, to enable me to do testing before rolling out production-level changes.
</p>
<p style="margin-top: 0.15em; text-indent: 4ch;">
    My discipline for writing and regularly publishing continues to be absolutely atrocious. Part of the problem is the activation energy of pushing out changes to GitHub. I've been looking into whether or not I can schedule posts through Jekyll with CI/CD in GitHub, or similar. I find myself having lots of ideas and fewer and fewer opportunities to make good on them. The perennial "when X is there, I'll do it" feeling is hard to beat. I just need to get comments right. I just need to change the favicon. I just need to set up analytics. Ad nauseam. I'm working through the need to put things off until X is absolutely perfect, albeit slowly.
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
