Android Companion App to Dark Souls the board game: https://play.google.com/store/apps/details?id=com.dsdungeons
Web Version: http://bit.ly/dsdungeons
Custom Encounter Guide: https://github.com/dejawho/darksoulscampaigns/tree/master/reference

About Custom Campaigns:

Actually there is not an editor to create custom campaigns, so you need to create manually a text file with the json definition for the campaign. The challange is write the dungeon map, because it could be confusing, but there is a trick to export a random generated map and use it here. I will describe this later.

In this repository there is a folder "reference" which contains an encounter file, containing all the encounters embedded into the application, and some screenshots of all the tiles with their filename. The filename is important because it is also the id (without the extension) of the tile inside the application. The id is used in the dungeon map to identify a specific tile.

On the root of the repository you can see three campaigns: the coiled sword, the first journey (that are the campaign defined in DS:TBG manual) and a custom campaign created by me for the iron king setting.
Opening one of this campaign you will find a json file with everything the campaign need to be run. There is the title and the description of the campaign, pretty self explanatory.
Then there are the sets, these are the set the user should need to run this campaign. The encounters section contains the custom encounter defined only for this campaign.
At the end there is the strong part, the dungeons. This is a json array where every entry is a dungeon and it is represented with a matrix 30x30 . Each cell in the matrix is a tile if defined or an empty space if it is null. At the center of this matrix there is always the bonfire, so if we took the iron king campaign and look at position 15,15 you will find

{"tile":{"id":"bonfire2_a"},"exits":[{"direction":"LEFT","id":"left"},{"direction":"TOP","id":"top"}]}

You will see the tile id that match with one of the tile described in the reference folder, and the exit from this tile. The direction and the id of every exit are the same here but could be different. The direction is the direction where you see the exit, considering the tile could be rotated, the id is the id of that exit on the unrotated tile. Let's suppose we have a tile that unrotated has an exit on the left and one on the right, but to connect to the board it was rotated by 90 degree, doing this left became top and right bottom, so its exits will be on with direction TOP with id LEFT and one with direction BOTTOM with id RIGHT. Also two tiles are connected when they have an opposite direction for each other. Two tiles side by side (one on the left and one on the right) are connected when one have the exit with direction LEFT and the other with direction RIGHT.

Some tiles could have a fixed encounter also, and the boss tile has also a special attribute isBossRoom set to true.

{"tile":{"id":"miniboss2_a","encounterId":15},"exits":[{"direction":"RIGHT","id":"bottom"}],"isBossRoom":true}

every tile can have an encounter id, this means the player will always found that encounter in the tile. Instead isBossRoom is an attribute only of the room with the boss and there should be only one of them on every single dungeon. The encounter id is optional in the common tiles, if unspecified then an encounter will be chosen randomly, but it is mandatory on the boss room.

As you can see it could be annoying defining a dungeon without a visual editor, but as said there is a trick. A new icon was added in developer mode, this icon will show the current board data, and you can simply share and copy paste it. So for now the easy way to generate a dungeon is start a new game, you can now select the exact number of tiles you want, configure also the set, biomes and enable the developer variant. Once the map is generated explore it to see if it match your desires. If not there is no need to start a new game, enable the randomized dungeon variant an generate new dungeon by resting at the bonfire. Once you found a design that match your taste simply use the new button to see and share the dungeon data. Then copypaste this data in your campaign file. This data will contains also the encounter id for each room, if you want randomized encounter simply delete the encounter id for the rooms, but remember the boss room always need an encounter.

A little note, ie on the coiled sword campaign all the boss encounter were redefined, they are pretty much the same of the one embedded into the app, what is changing is the value "encountersDifficulty", this are the chances of find level 1,2,3 encounters in the room when the difficulty is set to automatic and when the encounters are chosen randomly. For example [0, 0.4, 0.6, 0] means there is 0% chance of lv1 encounter, 40% of lv2 and 60% of level 3. There is an entry for lv4 but it is actually not used.
