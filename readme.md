# Discorder Murder Games

This is a mod to [Orteil's Murder Games](https://orteil.dashnet.org/murdergames/) that aims to improve the experience by adding Traits, Statuses, Items, and events linked to all of those, as well as adding new events for the existing content. This mod is made by and **for** the [DiscOrder of No Quarter](https://discord.gg/TRDrRdv) community, and will include a more specific injokes as well as generic updates.

A Google Drive exists with a [list](https://docs.google.com/document/d/1lU5NvP_yMKFLUt4bB2rJFOGVZc2SBDWSGu4Fvhl__Pc/edit?usp=sharing) of all the events in the game, which you can use for reference or comment improvements you think could be made.

## Using this Mod

In order to use this mod, you need to go to [https://orteil.dashnet.org/murdergames/](https://orteil.dashnet.org/murdergames/).

### Desktop/Laptop Chrome

Open Developer Tools with `Crtl + Shift + I` or from the sidebar. Switch to the "Sources" tab at the top of the window that opens up, and in "Page" subtab navigate to `top/orteil.dashnet.org/murdergames/murdergames/` (there will be two, one of which is empty. Choose the one that has text written in it). In the file viewer, right click and choose "Override Content." If you've never done this before, you'll be asked to choose a folder to store the content in; choose a place that you'll be able to get to again easily. You may want to create a new folder to store this in.

Open the folder you chose or created and open folders until you find a file called `index.html`. Delete it and replace it with the `index.html` in this github. Refresh the Murder Games page, and then you should be able to play the DiscOrder version of Murder Games.

### Others

If you use this mod on a platform not listed, feel free to leave a comment on the process so that I can add it to the document for others.

## Making your Own Mod

If you want to make your own version of the Murder Games, it's remarkably easy. Download a copy of index.html to modify the code and follow these simple instructions, then when you're done follow the steps in the section above but with the copy you changed.

### Adding Traits, Statuses, and Items

Traits, Statuses, and Items, collectively called Perks, are defined starting at line 551. Find the type you want to add, and paste the following code with \_\_your_name\_\_ and \_\_your_description\_\_ replaced with the name and description of the Perk you want to add.

    new G.perk({name:`__your_name__`,desc:`__your_description__`});`

### Adding Events

I'm still working on how to add events, but I'll update this when I get it down. 

The events start at line 1521 (update as this changes).

The rules for how event triggers are defined is written starting at line 1293 (make sure to update this as it changes), but are reproduced here for ease of access.


> returns one or more people/team who fit the given conditions
>
> the "me" parameter specifies which person/team the first condition must match
>
> ie. ['shotgun lunatic','- meek'] will return a person with the "shotgun" and "lunatic" perks, and a person who is "meek" and on a different team
>
>   syntax :
>
>		'perk_name'
>
>		    > must have the perk
>
>		'!perk_name'
>
>			> must not have the perk
>
>		'perk_name|other_perk|third_perk'
>
>			> must have either of those perks
>
>		'!perk_name|other_perk|third_perk'
>
>			> must not have any of those perks
>
>		'team'
>
>			> must be a team (otherwise must be a person)
>
>		'members:3' 'members:<3' 'members:>3'
>
>			> team must have the specified amount of members
>
>		'alive:3' 'alive:<3' 'alive:>3'
>
>			> this many people must be alive
>
>		'dead:3' 'dead:<3' 'dead:>3'
>
>			> this many people must be dead
>
>	    'deaths:3' 'deaths:<3' 'deaths:>3'
>
>			> this many deaths must have happened
>
>		'teamalive:3' 'teamalive:<3' 'teamalive:>3'
>
>			> this many in the team must be dead
>
>		'teamdeaths:3' 'teamdeaths:<3' 'teamdeaths:>3'
>
>			> this many in the team must be dead
>
>		'+'
>
>			> must be the same team as the previous team/person selected
>
>		'-'
>
>			> must be a different team
>
>		'inTeam'
>
>			> must be in a team
>
>		'noTeam'
>
>			> must not be in a team
>
>		'!out'
>
>		    > must not have a perk marked .out (dead, sheep, comatose etc)
>
>		'!able'
>
>			> must have a perk marked .disable (trapped, shell-shocked etc)
>
>		'any'
>
>			> can be abled or disabled (still doesn't accept out)
