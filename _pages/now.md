---
title:
layout: default
permalink: /now/
published: true
---

This is a <a href="https://nownownow.com/about">now page</a>. It was last updated on 1st December 2025.

<p style="margin-top: 0.15em; margin-bottom: 0.15em">
	<ul>
		<li><strong><em>reading:</em></strong> <a href="https://thealexandrian.net/so-you-want-to-be-a-game-master">So You Want to be a Game Master</a> by Justin Alexander </li>
		<li><strong><em>watching:</em></strong> <a href="https://www.youtube.com/watch?v=PssKpzB0Ah0">Stranger Things 5</a> </li>
		<li><strong><em>listening:</em></strong> <a href="https://en.wikipedia.org/wiki/Everybody_Scream">Everybody Scream</a> by Florence + The Machine</li>
		<li><strong><em>learning:</em></strong> <a href="https://docs.ansible.com">Ansible</a>, <a href="https://github.com/kimdre/doco-cd">dococd</a>, and <a href="https://www.youtube.com/watch?v=-0Ao4t_fe0I">Cerice</a></li>
		<li><strong><em>writing:</em></strong> A metal album, exploring feelings around transitioning; a new D&D campaign </li>
		<li><strong><em>playing:</em></strong> <a href="https://en.wikipedia.org/wiki/Prince_of_Persia:_The_Lost_Crown">Prince of Persia: The Lost Crown</a></li>
	</ul>
</p>

<p style="margin-top: 0.15em; margin-bottom: 0.75em; text-indent: 4ch;">
		December already, blimey. Christmas is upon us, and this year B— and I have agreed to drip-feed the season and decorations into the house, instead of last year's mad dash to get it all up in one day. So far it's quite nice, and it's doing me some good to ease into the season. Christmas playlists are on, and we're into the throes of Advent calendaring for the kids. I made one for M— this year, and she seems super pleased with it.
</p>
<p style="margin-top: 0.15em; margin-bottom: 0.75em; text-indent: 4ch;">
		I'm having to remind myself constantly that this time of year can be rough, mentally. I have a weekly reminder to check in with myself and think about how the season might be affecting me. That check-in is useful, and it encourages me to be a bit more fallow with my time. Adapting to being a full-time parent is still ongoing, and it feels important now more than ever to be able to just sit and accept that things won't get done until tomorrow. The noon sun is low, and so too is my energy. It's comforting to have accepted that, and I'm feeling much more able to relax into the season as a result.
</p>
<p style="margin-top: 0.15em; text-indent: 4ch;">
		It's also that time where I start thinking about my <span><a href="https://www.youtube.com/watch?v=NVGuFdX5guE">yearly theme</a></span>. A few have been floating around in my head, but I think the big one I keep coming back to is Year of Health. I'll be writing more about this in a post in January, as I might go a different direction.
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
