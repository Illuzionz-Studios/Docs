# Configuration
This page has extensive information on how to configure the different aspects of the plugin. All default files
in the plugin come with thorough comments to help get you started configuring. This page will break down
the sections of the config for more in-depth of what they do and help. Some text may have annotations
you can click on for more info, others may have comments that you can also read. To just view default files view [here](../default-files).

## Main Config
The first file to look over is the main file `config.yml`. This just contains basic options
that apply throughout the plugin.

``` yaml title="config.yml" linenums="1" 
settings:
    # The language file to use for the plugin
    # More language files (if available) can be found in the plugins locale folder.
    locale: en_US

fishing:
    # The chance of finding a custom reward while fishing
    # can be a decimal number.
    reward-chance: 100.0 # (1)
    # Amount of experience to give the player when they HAVEN'T found a custom reward.
    # "x to y" indicates a range between two numbers.
    # -1 indicates to not reward exp.
    exp-reward: 1 to 6 # (2)
    # The minimum amount of ticks to wait for a bite. This is before processing lure and other things
    # REQUIRES 1.16+
    min-wait-time: 100 # (3)
    # The maximum amount of ticks to wait for a bite. This is before processing lure and other things
    # REQUIRES 1.16+
    max-wait-time: 600 # (4)
```

1.  This represents the chance out of 100 of getting a custom reward. This means if this is
    set to 100, then every catch will be a custom fishing reward.

2.  This is the amount of experience points to give when NOT FINDING A CUSTOM REWARD.

## Reward Config
Each custom reward is defined in it's own unique file in the `/rewards` directory. This section will
go over each section in the reward config.

??? abstract "demo_reward.yml"
    ``` yaml
    ###################################################################
    # A custom reward that can be caught by fishing.                  #
    # This files defines all the options for a specific reward        #
    # identified by 'Name'. Change the options here as you like       #
    # to make your own reward. To add more than one catchable reward, #
    # copy this file and then change the options to your liking.      #
    #                                                                 #
    # Any queries join our discord at https://discord.gg/DbJXzWq      #
    ###################################################################

    # Placeholders
    # {player} : The player's name

    # The name or identifier of the reward. Make it something
    # relating to what the reward is.
    name: "Demo Reward"

    # A list of commands to run when this reward is caught.
    commands:
        - "msg {player} demo"

    # List of custom item rewards, leave the list blank
    # if you don't want any custom items or remove this section
    items:
        # The section for an item
        sword:
            # Chance out of 100 of getting this reward
            chance: 100
            # The custom display name of the item.
            # If an empty string has no custom name
            item-name: "&a&lMy Custom Sword"
            # The lore of the item. Set to null for no lore
            lore:
            - "&7Made from CustomFishing!"
            # The amount of this item
            amount: 1
            # The material of the item
            material: DIAMOND_SWORD
            # If has custom model data
            model-data: 0
            # A list of enchants as "ENCHANT:LEVEL". Set to null for no enchants
            enchants:
            - "SHARPNESS:5"
            # If the item appears as glowing like it's enchanted
            glowing: false
            # If the item is unbreakable
            unbreakable: false
            # If to hide flags like unbreakable and enchants
            hide-flags: false

    # A list of messages to send the player when this reward is caught.
    messages:
        - '&7&l(!) &aYou found an awesome sword!'

    # A list of messages to broadcast to the server when this reward is caught.
    broadcasts:
        - '&4&l(!) &c{player} &7found a &c&lLEGENDARY &7reward!'

    # Title to send to the player. Leave blank if none.
    title: "&a&lYou found a reward"

    # Subtitle to send to the player. Leave blank if none.
    sub-title: "&7&oFound: Sword"

    # The weight (chance) of this reward being found. The higher the weight
    # the more likely it is to be caught.
    weight: 4.5

    # If the player should still receive default minecraft rewards.
    # This means fish, enchanted books, enchanted items etc.
    # If set to false only the custom reward will be given
    vanilla-rewards: false

    # The amount of experience to give the player
    # as a range, the "x to y" meaning from x to y (inclusive)
    # Can just be a singular number
    exp-amount: "1 to 6"

    # The sound to play when this reward is caught. Leave blank
    # for no sound
    sound: ENTITY_FIREWORK_ROCKET_TWINKLE

    # Requirements if the player can find this reward
    requirements:
        # Check if the player has a certain permission
        permission:
            type: permission
            value: "customfishing.vip"
    ```

#### Placeholders
The placeholders, formatted as `{placeholder}`, can be used anywhere in the config and will be replaced with the appropriate value.

```
{player}: The player's name
```

### Name
This is simply the name of this reward. It is used as an identifier to tell rewards apart, isn't used in the actual physical or virtual reward. 

``` yaml
name: "Demo Reward"
```

### Commands
A list of commands that will be executed by the CONSOLE when the player finds this reward. For example, you can use the placeholder `{player}` to use the player's name. If this list is empty or set to null it won't execute any commands. This means you can remove this option from the config and no commands will be loaded.

``` yaml
commands:
  - "msg {player} demo"
```

### Items
A list of custom items are defined each by the options under a key. For example, here the options are set under "sword:", you can add as many you like under the "items:" key.

``` yaml
items:
  sword:
    chance: 100 # (1)
    item-name: "&a&lMy Custom Sword" # (2)
    lore: # (3)
      - "&7Made from CustomFishing!"
    amount: 1 # (4)
    material: DIAMOND_SWORD # (5)
    model-data: 0 # (6)
    enchants: # (7)
      - "SHARPNESS:5"
    glowing: false # (8)
    unbreakable: false # (9)
    hide-flags: false # (10)
```

1.  This is the chance that if this reward is found this particular item will be given. These chances are       independent of other items so if two items have a chance of 75, a calculation of 75 out of 100 will be calculated for each item.

2.  This is the custom display name of the item. If set to a blank string or set to null then no custom name will be used.

3.  This is the custom lore for the item. If the list is empty or set to null then no custom lore will be used.

4.  This is the amount of this item to give to the player.

5.  The material enum to use for the item. Can be found [here](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Material.html)

6.  If this item should have custom model data for texture packs then set that here. If set to null then this won't be set.

7.  These are the enchants to apply to the item. They are defined as ENCHANT_ENUM:LEVEL, if the enchant defined is invalid then that enchant won't be applied. If the list is empty or set to null then no enchants will be used. All the available enchant enums can be found [here](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/enchantments/Enchantment.html)

8.  A simple option to make the item have the enchanted glowing effect but any actual enchantments. Will automatically make the `hide-flags` option true.

9.  If this item cannot be broken and does not use up durability.

10. This indicates that flags such as enchants and unbreakable flags will not be displayed on the item.

### Broadcasts
A set of broadcasts to send to every online member. If this list is empty or set to null it won't send any messages. This means you can remove this option from the config and no broadcasts will be loaded.

``` yaml
broadcasts:
  - '&4&l(!) &c{player} &7found a &c&lLEGENDARY &7reward!'
```

### Titles
Titles to send to the player when they find this reward. `title` must be set in order for the `sub-title` to also work. If either is an empty string or null its message will be treated as unset.

``` yaml
title: "&a&lYou found a reward"
sub-title: "&7&oFound: Sword"
```

### Weight
This is the weight (chance) that this item has to be found out of all rewards that meet the player's requirements. How this works is it adds all the chances from rewards that can be found. For instance, if there are 3 rewards with a weight of 10, 2 with a weight of 3, and 1 with a weight of 0.5, the total is 36.5. So for the reward with weight 3, it has a 3/36.5 chance of being picked.

``` yaml
weight: 4.5
```

### Vanilla Rewards
This is if the default vanilla fishing loot table will also reward something when this reward is found. For example, if this is set to 'true', if you find a reward it will also give you a fish item or enchantment book etc. If it is set to 'false', then only a custom reward will be given and no default fishing rewards from vanilla.

``` yaml
vanilla-rewards: false
```

### Experience Amount
This is the number of experience points to award the player when they find this reward. The default vanilla is from 1 to 6. The format for this is as a range, from one number to another. This is formatted as `{number} to {number}`. It can also be a singular number in which case that amount of experience will be rewarded every time. If this is set to null then no experience will be rewarded.

``` yaml
exp-amount: "1 to 6"
```

### Sound
This is the sound that is played for the player when they find this reward. These can be found [here](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Sound.html). If this is set to null then no sound will be played.

``` yaml
sound: ENTITY_FIREWORK_ROCKET_TWINKLE
```

### Requirements
Fishing rewards can have certain requirements to set where they can be found. You can use
[PlaceholderAPI placholders](https://github.com/PlaceholderAPI/PlaceholderAPI/wiki/Placeholders) for the player catching the reward here. They
are defined with the following format below and then each type will be expained. 

``` yaml
requirement:
    type: <REQUIREMENT TYPE> # (1)
    value: <FIRST INPUT> # (2)
    input: <SECOND INPUT> # (3)
```

1.  This is where you specify the type of requirement check.

2.  This is the first input value. This is at least needed for every type.

3.  This is the secondary input value. Only some types need it and it is for more
    comparision checks like numbers or strings.

For a reward you can have multiple requirements by denoting each with an identifier like so.

``` yaml
requirements:
    requirement1:
        type: <REQUIREMENT TYPE>
        value: <FIRST INPUT>
        input: <SECOND INPUT>
    requirement2:
        type: <REQUIREMENT TYPE>
        value: <FIRST INPUT>
        input: <SECOND INPUT>
```

You can also have an inverted requirement type which means it checks if the condition is NOT TRUE, if so the
requirement passes. This can be done by adding 'not ' infront of the requirement type like so. Make sure there
is a space between `not` and the requirement type.

``` yaml
requirement:
    type: not <REQUIREMENT TYPE>
    value: <FIRST INPUT>
    input: <SECOND INPUT>
```

#### Permission
This checks if the player has the given permission.

``` yaml
requirement:
    type: permission
    value: <permission to check>
```

#### Region
This checks if the player is in a given WorldGuard region.

``` yaml
requirement:
    type: region
    value: <region to check>
```

#### Experience
This checks if the player has at least a certain amount of experience points.

``` yaml
requirement:
    type: exp
    value: <experience points>
```

#### Near
This checks if the player is within range of certain coordinates.
The format for `location` is `worldname,x,y,z`

``` yaml
requirement:
    type: near
    value: <location>
    input: <distance>
```

#### Near
This checks if the player is in a certain world.

``` yaml
requirement:
    type: world
    value: <world_name>
```

#### String Equals
This checks if a certain string is exactly equal to another. Pass in values through PlaceholderAPI.

``` yaml
requirement:
    type: string equals
    value: <input string>
    input: <string to compare to>
```

#### String Equals Ignorecase
This checks if a certain string is equal to another ignoring case. Pass in values through PlaceholderAPI.

``` yaml
requirement:
    type: string equals ignorecase
    value: <input string>
    input: <string to compare to>
```

#### String Contains
This checks if a certain string contains another string. Pass in values through PlaceholderAPI.

``` yaml
requirement:
    type: string contains
    value: <input string>
    input: <string to see if contains>
```

#### Regex
This checks if a regular expression matches the given text.

``` yaml
requirement:
    type: regex
    value: <the string to match with>
    input: <the regex expression>
```

#### Comparison Operators
This checks how certain numbers compare to each other. Avaiable operations are `==, >=, <=, !=, >, <`.
Pass in values through PlaceholderAPI.

``` yaml
requirement:
    type: ==, >=, <=, !=, >, <
    value: <first number to check>
    input: <second number to check>
```