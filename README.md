phpsdl
======

This is a Simple Declarative Language (SDL) parser for PHP. It uses the internal
PHP tokenizer to parse files and resolves it into tags compliant with SDL 1.2.

## What is SDL?

If you have no clue what SDL is, it could for example be this file:

    recipe "Noccy" size="4dl"{
        ingredient "Bacardi Limon" amt="10cl"
        ingredient "Orange Juice" amt="10cl"
        ingredient "Sprite or 7-up" amt="20cl"
        notes "Add a few cubes of ice and a slice of lemon"
    }

You can think of this as XML, but each node can have multiple values. It kinda
works for the above example tho if we make the name into an attribute:

    <recipe size="4dl" name="Noccy">
        <ingredient amt="10cl">Bacardi Limon</ingredient>
        <ingredient amt="10cl">Orange Juice</ingredient>
        <ingredient amt="20cl">Sprite or 7-up</ingredient>
        <notes>Add a few cubes of ice and a slice of lemon</notes>
    </recipe>

For more information, see SDL.md in the project folder.

## Parsing with PHPSDL

After including the three files, you are good to go.

There are two ways to parse a file:

    $root = new SdlTag();
    $root->loadFile("drinks.sdl");

Or:

    $root = SdlTag::createFromFile("drinks.sdl");

Querying the drinks from the above list is simple with the recursive query
function:

    $rcp = $root->queryOne("/recipe[Noccy]");
    printf("To make a %s (%s) you need:\n",$rcp[0],$rcp->size);
    $cts = $rcp->query("ingredient");
    foreach($ingredient as $tag) {
        printf("  %-6s %s\n", $tag->amt, $tag[0]);
    }

Outputting the SDL of a tag is as simple as calling `encode()` on the tag. For
the root note, you want to call `encodeChildren()` instead if you are writing
the output to a file.
