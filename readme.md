# Discorder Murder Games

This is a mod to [Orteil's Murder Games](https://orteil.dashnet.org/murdergames/) that aims to improve the experience by adding Traits, Statuses, Items, and events linked to all of those, as well as adding new events for the existing content. This mod is made by and **for** the [DiscOrder of No Quarter](https://discord.gg/TRDrRdv) community, and will include a more specific injokes as well as generic updates.

## Using this Mod

This mod overrides the code of [https://orteil.dashnet.org/murdergames/](https://orteil.dashnet.org/murdergames/) in order to function. Follow these steps to replicate this.

### Google Chrome on Windows

1. From the button that says `<> Code` select "Download ZIP"
2. Go to your computer's Downloads folder and right click "murdergames-Better-instructions.zip" that you downloaded in step 1. Select "Extract All" and choose somewhere easily accessible and safe, such as your computer's Documents folder, to save it
3. Go to [https://orteil.dashnet.org/murdergames/](https://orteil.dashnet.org/murdergames/)
4. Open Developer Tools with `Crtl + Shift + I` or from the sidebar
5. Switch to the "Sources" tab at the top of the window that opens up. Next to `Page` and `Workspace` there will be a button labelled `>>`. Click it and select `Overrides`
6. Click "Select folder for overrides." Choose the "murdergames-Better-instructions" folder created in step 2. A popup will appear asking for access to that folder. Press "Allow"
7. Refresh the page. This will allow you to use the modified version of the game.


## Making your Own Mod

If you want to make your own version of the Murder Games, it's remarkably easy. In the code you downloaded above, navigate to the index.html file and follow these instructions, then save the file and refresh the page when you're done.

### Adding Traits, Statuses, and Items

Traits, Statuses, and Items, collectively called Perks, are listed starting at line 565. Find the block of commands that type you want to add, and paste the following code at the end, with \_\_your_name\_\_ and \_\_your_description\_\_ replaced with the name and description of the Perk you want to add.

    new G.perk({name:`__your_name__`,desc:`__your_description__`});`

### Adding Events

Events are listed starting at line 1531. I've attempted to comment on each block of events to label what kind of events they are, and for ease of access I recommend you try to add new events into a suitable spot or create a new spot if you have a lot of related events.

Idle events (events that do not change the state of the world) are added with the following code:

        new G.act(__chance__,'__triggers__',`__text__`);

This can become a bit complex, so I'll break down each of the 3 bits you'll be replacing one at a time.

**\_\_text\_\_**

The simplest of the 3, \_\_text\_\_ is merely the text you want displayed in the event. You're provided with a few escape sequences that allow the text to be personalized. All the escape sequences are surrounded by brackets ( [ ] ).

The first escape sequence is the character escape sequence: [1]. This will be replaced with the name of whoever is performing the event. For example:

    [1] is loafing about.

performed by Slaking becomes

    Slaking is loafing about.

Similarly there exists [2], [3], and so on to use the names of secondary actors. Not all events have secondary actors, and if they exist and who they are is decided by your \_\_trigger\_\_. More detail will be put into here later. 

The other type of escape seqeunce is to personalize pronouns. There are four built in for you.

- [1:they]
- [1:them]
- [1:their]
- [1:themself]

Both of these commands will grab the gender of the actor and display the appropriate pronoun. If you want to define your own set of gender-specific phrases, you can use

- [1:g|\_\_masculine\_\_|\_\_feminine\_\_|\_\_neutral\_\_]

with \_\_masculine\_\_, \_\_feminine\_\_, and \_\_neutral\_\_ replaced with the phrase you want to use in each instance. For all of these you can replace the 1 with a different number to use the pronouns of a secondary actor, like you switch it for their names.

The third type of escape seqeuence is the color sequences. This is one of [RED] or [BLUE] and is placed at the start of the text to set the color of the text box. If neither is used, the text box is the default grey. Typically [RED] is used for events where a player is killed or put into danger, while [BLUE] is used for events where a character gains an item or ends a dangerous condition.

The fourth type of escape sequence is the random selector. By putting multiple phrases inside of brackets separated by a pipe character ( | ), a random option will be selected each time the event appears.

The final escape sequence is \<br\>, which causes everything after it to appear on a new line. You don't want to have any spaces surrounding it.

Example:

    [1] asks [2] if [2:g|he has|she has|they have] any extra berries.<br>It turns out [2] does, and [1:they] take [2:their] [sitrus|oran] berry.

with Mr. Mime (masculine) and Unown (neutral) could become

    Mr. Mime asks Unown if they have any extra berries.
    It turns out Unown does, and he takes their sitrus berry.

or 

    Mr. Mime asks Unown if they have any extra berries.
    It turns out Unown does, and he takes their oran berry.

**\_\_chance\_\_**

\_\_chance\_\_ is replaced with a number representing how often the event happens. It typically ranges between 0.01 for very unlikely events and 1 for likely events. Here are some common chance values you may want to adhere to:

- 1: Simple idle event, Lunatic triggering
- 0.9-0.6: Kill with a deadly weapon
- 0.5: Complex idle event
- 0.4-0.2: Kill with a weapon, Rare idle event, Common active event
- 0.15-0.05: Barehanded kill, Very rare idle event, Rare active event
- 0.01: Very rare active event

Alternatively you can use FORCE for events that must happen when the trigger is met.

Finding items is specical because all item find chances are multiplied by the item find multiplier. You can increase or decrease this multiplier stored in the variable `itemFindMult` to make items appear more or less frequently. When defining the chance to find an item, you use `itemFindMult*__chance__`.

Each item has its own chance that is multiplied by the item find multiplier to make it more or less rare.

- 0.1: Common (useless items)
- 0.05: Uncommon (most armors and low-tech weapons)
- 0.02: Rare (advanced weaponry and tactical gear)
- 0.01: Legendary (magical or super rare gear)

**\_\_triggers\_\_**

\_\_triggers\_\_ has the most complexity to understand. The core idea is that the event only happens if there is a player (or players) that fit the trigger criteria. You keep the criteria inside a pair of brackets ( [ ] )and if there are multiple criteria it checks that a player exists that fulfills all of them. The criteria are defined starting at line 1303 (make sure to update this as it changes), but are reproduced here for ease of access.

>   'perk_name'
>
>       > must have the perk
>
>	'!perk_name'
>
>       > must not have the perk
>
>	'perk_name|other_perk|third_perk'
>
>		> must have either of those perks
>
>	'!perk_name|other_perk|third_perk'
>
>		> must not have any of those perks
>
>	'team'
>
>		> must be a team (otherwise must be a person)
>
>	'members:3' 'members:<3' 'members:>3'
>
>		> team must have the specified amount of members
>
>	'alive:3' 'alive:<3' 'alive:>3'
>
>		> this many people must be alive
>
>	'dead:3' 'dead:<3' 'dead:>3'
>
>		> this many people must be dead
>
>   'deaths:3' 'deaths:<3' 'deaths:>3'
>
>		> this many deaths must have happened
>
>	'teamalive:3' 'teamalive:<3' 'teamalive:>3'
>
>		> this many in the team must be alive
>
>	'teamdeaths:3' 'teamdeaths:<3' 'teamdeaths:>3'
>
>		> this many in the team must be dead
>
>	'+'
>
>		> must be the same team as the previous team/person selected
>
>	'-'
>
>		> must be a different team from the previous team/person selected
>
>	'#'
>
>		> must be a different team from the FIRST team/person in the event
>
>	'inTeam'
>
>		> must be in a team
>
>	'noTeam'
>
>		> must not be in a team
>
>	'!out'
>
>		   > must not have a perk marked .out (dead, sheep, comatose etc)
>
>	'!able'
>
>		> must have a perk marked .disable (trapped, shell-shocked etc)
>
>	'any'
>
>		> can be abled or disabled (still doesn't accept out)
>
>   'every'
>
>       > can be out or in

*'every' is not listed in index.html, but from reading the code I've figured out what it does*

While there are a lot, the most important ones are perk names, combination perk names, and the negation of those 2. These allow you to define events that trigger for only players with specific perks or combinations of perks. 

For example, if you wanted only players with the water type perk to have a swimming event, you could define the trigger as 

    ['water_type']

*notice that even though there is a space in the perk name, when we add it to the trigger we replace it with an underscore. This is because triggers use spaces to differentiate between perks*

Let's design a more complex trigger. Let's say we want players with the rock type or ground type perk to have an event where they're afraid of the water. Now, it'd be silly to have a player with both the rock type or ground type and water type perks to be able to have the swimming event and the afraid of water event, so we'll also say players with the water type perk can't have this event. This trigger would be

    ['ground_type|rock_type !water_type']

*notice the use of the pipe character to denote ground type or rock type. If there was a space instead, the event would only trigger for a player with both the ground type and rock type perks, rather than either one*

This is the core of making triggers for a single character. But what about when we want events that involve multiple characters?

In that case we create two or more single character triggers and separate them with a comma inside the brackets. For example, if we wanted an event where two players with the water type perk swam together, we would create the trigger

    ['water_type','water_type']

This creates a combination trigger that only happens if one player with the water type perk can be found, and then another player with the water type. However, we often want to limit events so that the secondary actors have to be on the same or different team. If we wanted to make sure our players only had the swim event with teammates, we would change the trigger to

    ['water_type','+ water_type']

where + denotes a member of the same team rather than being a perk name. We can also include more than three players, such as in an event where two players with water type on the same team bully a player on an enemy team with rock type or ground type for being unable to swim.

    ['water_type','+ water_type','- ground_type|rock_type !water_type']

You may notice that we're creating multiple triggers here, and wonder if that connects to the number inside the character escape sequence. In fact it does, with the number referring to the character that matches that trigger. In the example above, [1] would be our first player with water type, [2] their teammate with water type, and [3] their enemy with ground type or rock type. The player that fits the first trigger is the primary actor, and each player can only be a primary actor once per round.

You have to be careful with the teams, because + and - match a team in reference to the previous trigger rather than to the primary actor. This means that if we switch the order of our example to this:

    ['water_type','- ground_type|rock_type !water_type','+ water_type']

It now refers to a player with the water type, and enemy with the ground type or rock type, and a player on that enemy's team with the water type.

Finally I'd like to discuss 'any' and 'every'. Some perks, by base game only the statuses, can mark a player as disabled or out. A player that is disabled cannot match any event except one with 'any' in the trigger while a player that is out cannot match any event except one with 'every' in the trigger. This is used to create statuses that prevent a player from partaking in normal events, and then specifying events that they can partake in instead.

The other trigger qualities I find to be incredibly niche, and if you want to use them to create complex events you can study the provided reference.

Now that you've got the hang of passive events, you can program in active events, or events that change the state of the world. It uses a similar code snippet as before, but with an added piece.

    new G.act(__chance__,'__trigger__',`__text__`,p=>{__change__});

The added variable \_\_change\_\_ marks what kind of change occurs. There are four kinds.

- p[1].gain('\_\_perk\_\_');
- p[1].lose('\_\_perk\_\_');
- p[1].die(\`\_\_cause-of-death\_\_\`);
- p[1].kill(p[2],\`\_\_cause-of-death\_\_\`);

Gaining and losing simply add or remove perks, such as players finding or losing items or suffering from and being relieved of statuses, although it's perfectly add or remove traits from players as well. Simply replace \_\_perk\_\_ with the name of the perk you want the player to gain or lose (unlike in triggers do not replace spaces with underscores) and you're done. Of course, as always you can change the number inside the brackets in order to change which player it is applied to.

Die is also fairly simple, as the specified player dies. This adds in \_\_cause-of-death\_\_, which is simply a short description of how the player died for display purposes. As far as I know none of the escape sequences work inside this description. When a player dies with this command, they are automatically given the dead perk.

Kill is the most complex, but it should make sense if you've understood everything up to this point. The first player kills the second player, giving them the dead perk and themselve the has killed perk, with the cause of death message as the description. Of course you can switch around which player kills which, or even have a player kill themselves.

Multiple changes can be chained together in a single active event. Let's say we want an event where a player uses a burn heal to lose the burn status. We want them to lose both the status and the burn heal, so we would write

    p=>{p[1].lose('burn');p[1].lose('burn heal');}

and of course we could chain together as many changes as we wanted.

With that you should be armed with all the knowledge I have on how to mod Murder Games. If you've read this far, thanks! I spent 3 hours writing this tutorial instead of actually modding the game or doing anything useful. I hope it helps you make your very own version of Murder Games.
