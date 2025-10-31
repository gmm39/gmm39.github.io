# RequestGroups Schema Documentation

## Overview
This JSON schema defines a flexible system for creating request missions. Each mission configuration specifies what items players can provide to complete requests from villagers, with extensive customization options for item selection, quantities, quality requirements, and contextual restrictions.

## RequestGroups Structure

### **Basic Mission Metadata**
- **`Id`**: Unique numeric identifier for the mission
- **`Special`**: Special conditions like seasons, festivals, or birthdays (maps to Special enum. See below)
- **`Characters`**: Array of character IDs who can make this request
    - 0 specifies all characters

### **Item Requirement System**
The `Items` array contains one or more item sets that define what can fulfill the request:

#### **Items List**
- **`ItemIds`**: Array of identifiers that can represent:
  - Specific items (paired with ItemType 0)
  - Category numbers (paired with ItemType 1) 
  - Mission group numbers (paired with ItemType 2)
- **`ItemType`**: Specifies how to interpret each ItemId:
  - `0` = Individual item ID
  - `1` = Category ID (any item from that category)
  - `2` = Mission group ID (pre-defined item groups)
- **`ItemStack`**: Required quantity for each corresponding item
- **`ItemQuality`**: Quality requirements using a half-star system:
  - `1-14` = Specific quality levels (1 = 0.5 stars, 14 = 7 stars)
  - `-1` = Any quality acceptable
  - `-2` = Dynamic quality based on player progress
- **`Category`**: The item category this mission belongs to (maps to RequestCategory enum. See below)
- **`PickOne`**: When `true`, one item randomly chosen from the set is required
- **`DifficultChance`**: Probability (0.0-1.0) that the mission will be "difficult"
  - Difficult missions multiply ItemStack values and add quality requirements

### **Mission Text Content**
- **`MissionName`**: Display name shown to players
- **`MissionCondition`**: Brief objective text
- **`MissionCaption`**: Detailed description/flavor text
    - All 3 supports `{char}` placeholder. Instances will be replaced with the relevant character's name
- **`DebugInfo`**: Developer notes about the mission's purpose or other relevant information

## Notes

### **Flexible Item Selection**
- Support for individual items, entire categories, or pre-defined groups
- Multiple item sets can be defined for random selection
- If "PickOne" is false, entire item list is used for quest (limit of 3 items)
    - This can be used to create very specific quests (e.g. request for every flower in a bouquet)

### **Scalable Difficulty**
- Dynamic quality scaling based on player progression is encouraged
- Configurable chance for harder versions of missions
- Difficulty scaling amounts are based on user configuration

### **Contextual Restrictions** 
- Seasonal limitations via Special field
- Character-specific request pools
- Rewards are determined based on category (See RewardGroups)

# RewardGroups Schema Documentation

## Overview
This JSON schema defines reward group configurations for distributing items to players upon completing request missions. Each reward group specifies a collection of potential rewards that can be granted.

## RewardGroups Structure

### **Basic Group Metadata**
- **`Id`**: Unique numeric identifier for the reward group
- **`Category`**: The category this reward group belongs to (maps to RequestCategory enum. See below)

#### **Item Identification**
- **`ItemIds`**: Array of item id's that can be awarded
- **`ItemStack`**: Quantity of each corresponding item to award
    - `-2` = Dynamic quantity based on price of all required items
- **`ItemQuality`**: Determines the quality level of awarded items:
    - `1-14` = Specific quality levels using half-star increments (1 = 0.5 stars, 14 = 7 stars)
    - `-2` = Dynamic quality based on player progress

## Notes

### **Selection Behavior**
- RewardGroups are chosen based on the category value of the selected RequestGroup. The first matching RewardGroup is chosen (a single RewardGroup for each category should exist).
- A single item is chosen from the relevant item group

# RequestCategory Enum Cheat Sheet

## Enum Values

| Value | Name | Description |
|-------|------|-------------|
| 0 | `None` | No category / Default value |
| 1 | `AnimalProduct` | Products from animals (eggs, milk, wool, etc.) |
| 2 | `Bug` | Insects and other bugs |
| 3 | `Crop` | Farm crops and vegetables |
| 4 | `Fish` | Fish and aquatic creatures |
| 5 | `Flower` | Flowers and bouquet |
| 6 | `Food` | Prepared food items |
| 7 | `Forage` | Foraged items from the wild |
| 8 | `Gemstone` | Precious stones and gems |
| 9 | `Gift` | Gift items |
| 10 | `Ingredient` | Cooking ingredients |
| 11 | `Seed` | Seeds for planting |
| 12 | `Mineral` | Minerals and ores |
| 13 | `Misc` | Miscellaneous items not fitting other categories |

## Notes
- Used to group request types and for matching with future dialogue changes.
- Reward pools are chosen based on Category listed in RequestGroups

<br>

# Special Enum Cheat Sheet

## Enum Values

| Value | Name | Description |
|-------|------|-------------|
| 0 | `None` | No special condition |
| 1 | `Spring` | Spring season |
| 2 | `Summer` | Summer season |
| 3 | `Autumn` | Autumn season |
| 4 | `Winter` | Winter season |
| 5 | `FlowerFestival` | Flower Festival event |
| 6 | `AnimalShow` | Animal Show event |
| 7 | `HoneyDay` | Honey Day event |
| 8 | `CropsShow` | Crops Show event |
| 9 | `TeaParty` | Tea Party event |
| 10 | `PetShow` | Pet Show event |
| 11 | `HorseDerby` | Horse Derby event |
| 12 | `CookOff` | Cooking Competition event |
| 13 | `JuiceFestival` | Juice Festival event |
| 14 | `PumpkinFestival` | Pumpkin Festival event |
| 15 | `HearthDay` | Hearth Day event |
| 16 | `StarlightNight` | Starlight Night event |
| 17 | `NewYears` | New Year's Coutdown event |
| 18 | `BirthdayJules` | Jules' birthday |
| 19 | `BirthdayDerek` | Derek's birthday |
| 20 | `BirthdayLloyd` | Lloyd's birthday |
| 21 | `BirthdayGabriel` | Gabriel's birthday |
| 22 | `BirthdaySamir` | Samir's birthday |
| 23 | `BirthdayArata` | Arata's birthday |
| 24 | `BirthdaySophie` | Sophie's birthday |
| 25 | `BirthdayJune` | June's birthday |
| 26 | `BirthdayFreya` | Freya's birthday |
| 27 | `BirthdayMaple` | Maple's birthday |
| 28 | `BirthdayKagetsu` | Kagetsu's birthday |
| 29 | `BirthdayDiana` | Diana's birthday |
| 30 | `BirthdayFelix` | Felix's birthday |
| 31 | `BirthdayErik` | Erik's birthday |
| 32 | `BirthdayStuart` | Stuart's birthday |
| 33 | `BirthdaySonia` | Sonia's birthday |
| 34 | `BirthdayMadeleine` | Madeleine's birthday |
| 35 | `BirthdayMina` | Mina's birthday |
| 36 | `BirthdayWilbur` | Wilbur's birthday |
| 37 | `BirthdayClara` | Clara's birthday |
| 38 | `BirthdayKevin` | Kevin's birthday |
| 39 | `BirthdayIsaac` | Isaac's birthday |
| 40 | `BirthdayNadine` | Nadine's birthday |
| 41 | `BirthdaySylvia` | Sylvia's birthday |
| 42 | `BirthdayLaurie` | Laurie's birthday |
| 43 | `BirthdayMiguel` | Miguel's birthday |
| 44 | `BirthdayHarold` | Harold's birthday |
| 45 | `BirthdaySherene` | Sherene's birthday |

## Category Breakdown

### **Seasons (1-4)**
- Used for seasonal-specific requests/events
- Applys from a week before season start to a week before season end.
    - This is to protect against receiving a season specific request without enough time to fulfill it

### **Festivals & Events (5-17)**
- Used for event-specific requests (request persists after event is over)
- Applies a week before event start date
- Applies to all instances of events that occur multiple times a year (crop show, pet show, etc.)

### **Birthdays (18-45)**
- Used for birthday-specific requests or gifting
- All 28 character birthdays
