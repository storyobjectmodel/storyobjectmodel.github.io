# storyobjectmodel.github.io

The public website for the Story Object Model, served at https://storyobjectmodel.com by GitHub Pages.

Plain static HTML and CSS, no build step. `schema/1.0/` carries copies of the seven normative JSON Schema files from `storyobjectmodel/som`, so that each schema's `$id` resolves. The repository `som` is the source; the copies here are regenerated from it and never edited by hand.

To swap the sign-up form for the Tally embed: in `index.html`, replace the `<form id="follow">` element (and the small script at the foot of the page that builds the mailto) with the Tally snippet. Fields stay identical: name, organisation, email, I am, I want to, the optional note, and the consent line.

`assets/` holds the two figures as standalone SVG (used by the page) and 2x PNG exports (for reuse in Claude Design or slides): `bus` (the story bus diagram) and `chat` (the newsroom group chat).

`emulator/index.html` is Morag McIntosh's SOM dashboard, the single-file build (`dist/som-dashboard.html`) from `storyobjectmodel/somdashboard`, with two skill-card labels changed from "playbook" to "declares". `scenarios/` at the site root holds the three scenario files it fetches. To update: rebuild in that repo, copy the new `dist/som-dashboard.html` here as `emulator/index.html`, and reapply the label change.
