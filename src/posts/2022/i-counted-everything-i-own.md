---
title: I Counted Everything I Own
date: 2022-01-25T19:34:18+01:00
author: Stefan Imhoff
description: As a minimalist I’m interested in how much stuff I own. I counted all the things I own.
charts: true
tags:
  - minimalism
  - personal
---

When I started to be interested in [Minimalism](/minimalism/) in 2017, I decided to count all my stuff.

The average European citizen owns 10,000 items and I wanted to know how much I own. I counted 2,490 items five years ago.

Last week Minimalist and filmmaker [Matt D’Avella](https://www.mattdavella.com/) released a new video titled [I counted everything I own as a minimalist](https://youtu.be/BB8o8-EdZY0). He also created other videos showing how little stuff he owns when he showed his [apartment](https://youtu.be/W2oU4bhaHqU), his [wardrobe](https://youtu.be/DSHsIOIhjJY), or what is in his [pockets](https://youtu.be/KZxI0-y3hew).

After watching his video I decided to recount my items and this time have everything neatly organized in a spreadsheet with columns for amount, room, category, date of buying, and price (if known).

## Rules for Counting

Back in 2017, I counted way too strict, I even counted each dental brush as one item. Matt had some good ideas for “what’s a thing”:

- A box of screws is one thing
- All loose screws are one thing
- All sandwich bags inside a box are one thing
- A hard drive and its cable is one thing because it needs it to operate
- A pair of socks is one thing
- Counting all consumables (except food)

## The Items I Own

Matt D’Avella counted 1,641 items in his home (together with his wife); 1363 things, and 278 consumables which is not a lot. He owns 498 items, his wife the rest.

After counting my flat and cellar on the weekend the results are finally in. I own **2541** items, including 162 consumables. _Matt won_.

I still own 652 books and way too many coffee cups (I even don’t drink coffee) 🤷‍♂️ And I still have way too many clothes, but even though I don’t wear my dress shirts very often, I still love them too much to get rid of them.

### Things vs. Consumables

I did the same as Matt and checked all my items if they are a thing or consumable. 339 items are consumables, the rest are things.

```chart
{
  "type": "doughnut",
  "data": {
    "labels": ["Things", "Consumables"],
    "datasets": [
      {
        "label": "Things vs. Consumables",
        "data": [2202, 339],
        "backgroundColor": ["#D7B98E", "#0C0C0C"]
      }
    ]
  },
  "options": {
    "plugins": {
      "title": {
        "display": true,
        "text": "Things vs. Consumables",
        "font": {
          "family": "SecuelaVariable, Arial, sans-serif",
          "size": "18",
          "weight": "900"
        }
      },
      "legend": {
        "position": "bottom"
      }
    }
  }
}
```

### Rooms

Most of my items are in the living room, followed by the bedroom, kitchen, cellar, corridor, bathroom, and balcony last.

```chart
{
  "type": "doughnut",
  "data": {
    "labels": [
      "Living Room",
      "Bedroom",
      "Kitchen/Dining Room",
      "Cellar",
      "Corridor",
      "Bathroom",
      "Balcony"
    ],
    "datasets": [
      {
        "label": "Rooms",
        "data": [1178, 674, 303, 178, 131, 64, 10],
        "backgroundColor": [
          "#FAD689",
          "#939650",
          "#DC9FB4",
          "#91AD70",
          "#2EA9DF",
          "#77428D",
          "#3C2F41"
        ]
      }
    ]
  },
  "options": {
    "plugins": {
      "title": {
        "display": true,
        "text": "Rooms",
        "font": {
          "family": "SecuelaVariable, Arial, sans-serif",
          "size": "18",
          "weight": "900"
        }
      },
      "legend": {
        "position": "bottom"
      }
    }
  }
}
```

### Categories

My top category is “books”, even though I recently gave 150 books away. Followed by kitchen tools … I need to get rid of my coffee mugs and the dozens of Asian noodle bowls. Then clothes, probably because I recently bought a bunch of socks.

```chart
{
  "type": "doughnut",
  "data": {
    "labels": [
      "Books",
      "Kitchen",
      "Clothing",
      "Electronics",
      "Entertainment",
      "Stationery",
      "Survival Gear",
      "Furniture",
      "Personal Care",
      "Decor",
      "Organization",
      "Sports Equipment",
      "Paper/Documents",
      "Clearning Supplies",
      "Plants/Plant Accessories",
      "Tools",
      "Medicine",
      "Travel"
    ],
    "datasets": [
      {
        "label": "Categories",
        "data": [
          652, 270, 259, 195, 179, 175, 140, 133, 108, 95, 74, 64, 48, 46, 40,
          35, 17, 11
        ],
        "backgroundColor": [
          "#CB4042",
          "#B9887D",
          "#B07736",
          "#B1B479",
          "#3A8FB7",
          "#6E75A4",
          "#CB1B45",
          "#F6C555",
          "#BEC23F",
          "#986DB2",
          "#FFBA84",
          "#967249",
          "#26453D",
          "#77969A",
          "#005CAF",
          "#535953",
          "#9F353A",
          "#ECB88A"
        ]
      }
    ]
  },
  "options": {
    "plugins": {
      "title": {
        "display": true,
        "text": "Categories",
        "font": {
          "family": "SecuelaVariable, Arial, sans-serif",
          "size": "18",
          "weight": "900"
        }
      },
      "legend": {
        "position": "bottom"
      }
    }
  }
}
```

## Conclusion

Now I have a good list where I can add new items I buy and remove items I sell or throw away. Additionally, the list is useful for traveling or in case I need to move. I can compare if a category or a room has too many things quickly. And with the new purchase price column, I can calculate how much I buy each year.
