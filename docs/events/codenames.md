# Bear Codenames

This game is codenames. Rules available online.
Use the included script to generate the word grid.
The script outputs one concealed-word-affiliation grid, for the players, and one team-revealed grid, for the teams' agents.

Ofc, cheating is very possible, try to dissuade it.

```
                             `  .:    
                         .yo    :No
                         -my   .yh.    o.
                     -+`-hNd: -dMMm.   ds
                    sN``NMMMN`mMMMM: :+h/
                   .dh..MMMMN`NMMMh-mMMM- -/
                  .NMMN.+NMm: .ss/`mMMMM- +m
                  +MMMM: .sdNNNmy/`mMMN/.:yy
                  .dMMs`oMMMMMMMMMN::::dMMMs
                   `:/omMMMMMMMMMMMs `NMMMM+
                 .dMMMMMMMMMMMMMMMMMo.dMMh:
  ============== :MMMMMMMMMMMMMMMMMMMNy- ================
                  +NMMMMMMMMMMMMMMMMMMMN
                   .---..-+ymMMMMMMMMN/
                                 .+shdh
<b>                   Bear Codenames
<b>                        
            7pm EST, Weds 6th
             Bear Clan Cave

I'm not gonna try to retheme this one. It's Codenames!

Codenames is a word-guessing game played between two teams. Each team's agent has to craft clever one-word clues to guide their team to the right words.

Rules explained on the day, but included below for reference

1m split between the winning team members -

Catch us in Bear Falls at 7pm EST Weds 6th, half way between Kugnae's north gate and the arena!

<b>                                             [//]   


======================================================== 

<b> Rules

For each game, we'll generate a 5x5 grid of words. Some of the words are for your team, some are neutral, some are for the enemy team, and one word is lethal!

Only each team's agent know which words are on which team.

On your team's turn, your agent will think up a one-word clue to describe one or more words your team needs to guess. This word can't be a word on the grid. Your agent will also say a number, describing how many words in the grid relate to their clue.

Your team will discuss, and choose words to reveal one by one.
 
- If you guessed right, you earn a point and keep going. 
- If you reveal a neutral card, your turn ends.
- If you reveal an enemy card, your turn ends and they get the point!
- ...But if you reveal the assassin card, your entire team instantly loses!

You're allowed to guess +1 more words than your agent says, to help you catch up with any words you missed from previous clues.

First team to reveal all of their own words wins!
```

Grid generation script:

```
import random

# master word list for codenames - rich with multiple meanings and connections
MASTER_WORD_LIST = [
    # words with strong noun/verb duality
    "BANK",
    "BRIDGE",
    "COURT",
    "DUCK",
    "WAVE",
    "RING",
    "TRAIN",
    "BEAR",
    "STOCK",
    "BOARD",
    "CLUB",
    "STAFF",
    "FACE",
    "FAIR",
    "FIELD",
    "FIRE",
    "FRAME",
    "GROUND",
    "GUARD",
    "HAND",
    "HEAD",
    "HIDE",
    "HOOK",
    "IRON",
    "JACK",
    "LAND",
    "LEAD",
    "LIGHT",
    "MATCH",
    "MINE",
    "PANEL",
    "PARK",
    "PLANT",
    "PLAY",
    "POUND",
    "PRESS",
    "PUMP",
    "RACE",
    "ROLL",
    "ROUND",
    "SCALE",
    "SEAL",
    "SHIP",
    "SHOCK",
    "SHOP",
    "SLIP",
    "SOUND",
    "SPRING",
    "SQUARE",
    "STAKE",
    "STAND",
    "STICK",
    "STRIKE",
    "SWITCH",
    "TABLE",
    "TOWER",
    "TRACK",
    "TRADE",
    "TRIP",
    "TURN",
    "WATCH",
    "WIND",
    "WORK",

    # similar but distinct pairs and clusters
    "BARK",
    "BITE",
    "BRANCH",
    "TRUNK",
    "ROOT",
    "LEAF",
    "SHADE",
    "FOREST",
    "OCEAN",
    "SEA",
    "LAKE",
    "RIVER",
    "STREAM",
    "CURRENT",
    "TIDE",
    "SHORE",
    "MOUNTAIN",
    "HILL",
    "PEAK",
    "VALLEY",
    "CLIFF",
    "CAVE",
    "STONE",
    "ROCK",
    "GOLD",
    "STEEL",
    "KING",
    "QUEEN",
    "PRINCE",
    "NOBLE",
    "PALACE",
    "THRONE",
    "CROWN",
    "SWORD",
    "COLD",
    "HOT",
    "WARM",
    "COOL",
    "HEAT",
    "FROST",
    "ICE",
    "SNOW",
    "SHARP",
    "DULL",
    "POINT",
    "EDGE",
    "BLADE",
    "CUT",
    "SLICE",
    "CRUSHED",

    # rich conceptual words
    "SHADOW",
    "LIGHT",
    "DARK",
    "BRIGHT",
    "GLOW",
    "SPARK",
    "FLAME",
    "SMOKE",
    "THUNDER",
    "LIGHTNING",
    "STORM",
    "CLOUD",
    "RAIN",
    "SUN",
    "MOON",
    "STAR",
    "CIRCLE",
    "LINE",
    "CURVE",
    "ANGLE",
    "CENTER",
    "BORDER",
    "CORNER",
    "HEART",
    "SOUL",
    "MIND",
    "SPIRIT",
    "DREAM",
    "HOPE",
    "FEAR",
    "JOY",
    "TIME",
    "SPACE",
    "PLACE",
    "MOMENT",
    "HISTORY",
    "FUTURE",
    "PAST",
    "WORD",
    "VOICE",
    "SONG",
    "MUSIC",
    "NOTE",
    "KEY",
    "PITCH",
    "RHYTHM",
    "DANCE",
    "STEP",
    "MOVE",
    "FLOW",
    "GRACE",
    "POWER",
    "FORCE",
    "ENERGY",

    # special bonus words
    "BUYA",
    "KUGNAE",
    "NAGNANG",
    "HAUSSON",
    "MYTHIC",
    "VALE",
    "FERRY",
    "POTION",
    "SCROLL",
    "CHARM",
    "SPIKE",
    "BLOOD",
    "SURGE",
    "ORB",
    "ELDER",
    "MOUNT",
    "ACORN",
    "LIVER",
    "HERB",
    "PIPE",
    "WHIRLWIND",
    "MIGHT",
    "WILL",
    "MANA",
    "VITALITY",
    "DAMAGE",
    "SHOUT",
    "SAGE",
    "WOOD",
    "WOOL",
    "AMBER",
    "DIADEM"
]


def get_random_word_list(num_words=25):
    """Generate a random selection of words from the master list."""
    if num_words > len(MASTER_WORD_LIST):
        raise ValueError(
            f"Requested {num_words} words but only {len(MASTER_WORD_LIST)} available"
        )

    return random.sample(MASTER_WORD_LIST, num_words)


def create_codenames_grid(word_list):
    """
    Creates both unmarked and marked 5x5 grids from a 25-word list.
    
    Args:
        word_list: List of 25 words for the grid
    
    Returns:
        tuple: (unmarked_grid, marked_grid) as strings
    """
    if len(word_list) != 25:
        raise ValueError("Word list must contain exactly 25 words")

    # team assignments for standard codenames
    # 9 for starting team, 8 for other team, 7 neutral, 1 assassin
    teams = ['W'] * 9 + ['B'] * 8 + ['N'] * 7 + ['!!!'] * 1
    random.shuffle(teams)

    # calculate max word length for consistent column width
    max_length = max(len(word) for word in word_list)
    col_width = max(max_length + 2, 8)  # minimum width of 8

    # create horizontal rule and blank row templates
    rule_parts = ["-" * col_width] * 5
    horizontal_rule = "+".join(rule_parts)

    blank_parts = [" " * col_width] * 5
    blank_row = "|".join(blank_parts)

    # build both grids
    unmarked_parts = []
    marked_parts = []

    for i in range(5):
        row_start = i * 5
        row_words = word_list[row_start:row_start + 5]
        row_teams = teams[row_start:row_start + 5]

        # create word row
        word_row = ""
        for j, word in enumerate(row_words):
            padded_word = word.center(col_width)
            if j > 0:
                word_row += "|"
            word_row += padded_word

        # create indicator row for marked grid
        indicator_row = ""
        for j, team in enumerate(row_teams):
            if team == 'N':
                indicator = " " * col_width  # blank for neutrals
            else:
                indicator = team.center(col_width)
            if j > 0:
                indicator_row += "|"
            indicator_row += indicator

        # add rows to unmarked grid
        unmarked_parts.append(blank_row)  # blank padding above
        unmarked_parts.append(word_row)  # word row
        unmarked_parts.append(blank_row)  # blank padding below

        # add rows to marked grid
        marked_parts.append(indicator_row)  # team indicators above
        marked_parts.append(word_row)  # word row
        marked_parts.append(blank_row)  # blank padding below

        # add horizontal rule between sections (except after last)
        if i < 4:
            unmarked_parts.append(horizontal_rule)
            marked_parts.append(horizontal_rule)

    unmarked_grid = "\n".join(unmarked_parts)
    marked_grid = "\n".join(marked_parts)

    return unmarked_grid, marked_grid


def print_grids(word_list):
    """Print both unmarked and marked grids with headers."""
    try:
        unmarked, marked = create_codenames_grid(word_list)

        print("UNMARKED GRID:")
        print("=" * 50)
        print(unmarked)
        print()

        print("MARKED GRID:")
        print("=" * 50)
        print(
            "W White Team (9)  B Black Team (8)  Neutral (7)  !!! Assassin (1)"
        )
        print()
        print(marked)

    except ValueError as e:
        print(f"Error: {e}")


def generate_random_grid():
    """Generate a complete codenames grid with random words from the master list."""
    words = get_random_word_list()
    print_grids(words)


# example usage
if __name__ == "__main__":
    # generate a random grid from the master word list
    print("RANDOM CODENAMES GRID:")
    print("=" * 60)
    generate_random_grid()

    # example of how to use with your own word list
    print("\n" + "=" * 60)
    print("To use with your own words:")
    print("my_words = ['WORD1', 'WORD2', ..., 'WORD25']")
    print("print_grids(my_words)")
    print("\nTo generate another random grid:")
    print("generate_random_grid()")
```