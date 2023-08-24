# ⚠️ Not Actively Maintained
Not actively maintained. I no longer play indoor competitively so I haven't used or updated this program in a hot minute. 

If I were to make some improvements I would probably use an AST parser instead of manually parsing like I did here. Have to say though, I am proud of freshmen year me for making this after taking only one CS course and continuing to improve it throughout my sophmore year. Had a good run with this and maybe you will too :)

![markus-winkler-IrRbSND5EUc-unsplash](https://github.com/ryanbyrne30/volleyball-scouting-reporter/assets/33855634/809b5396-06b7-4e16-8f68-457bd822d8f8)


# Volleyball_Stats_and_Analytics
A volleyball scouting tool

Keeps track of:
- hitting locations of each player for each position (ex. bic vs outside vs rightside) as well as outcomes (kills/errors/playable) 
- serving types and locations for each player
- setting tendencies for each rotation (including dumps)
- passing ratings for each player based on serve type

# Format

## Rotations
Start off a report by adding the starting rotation at the top of the file. You can comment it out if it makes it easier.
```
# 1 - Designates starting in rotation 1
```

The program will automatically keep track of the rotations of the team based upon the stats taken. If a line designates the team was receiving and the next line describe the team as serving, the program will increment the rotation automatically so you don't have to.

**If you need to specifically define which rotation it is in your report**, such as missing some rallys or to correct some data, you can add a line with an astericks follow by a number. This will reset the program rotation counter to the number you specify (0-6). Rotations will resume incrementing automatically from this point.
```
* 4 - Tells the program to start the following entries from rotation 4
```


## Receiving
When the team is receiving, the starting sequence starts with the serve type...
**Server Type**
```
- f: Float serve
- s: Spin/Jump serve
- h: Hybrid serve
```

The next characters that follow are the 2-digit number of the player.
**If the player's number is 1-digit, prefix it with a 0.**

The next character is the rating of the pass 0-3...
```
- 0: shank/ace
- 1: bad
- 2: aight
- 3: dime/money
```

After the initial pass is considered continue the line with the "Main Guts" sequence until the play is finished.

## Serving
When the team is serving, start the sequence with the 2-digit number of the player serving.

The next character is the type of serve...
```
- f: Float
- s: Spin/Jump
- h: Hybrid
```

The next character is the spot that the server served to...
```
- 1
- 2
- 3
- 4
- 5
- 6
- a: 1, 6 seam
- b: 5, 6 seam
```

The next character is the result of the serve...
```
- k: ace
- e: error
- a: neither, play continues
```

If the play continues, proceed with the "Main Guts" sequence until the play is terminated.


## Main Guts
The main format of the program is a succession of 6 characters. This sequence repeats itself until the end of the play where a newline denotes the end of the play. The first character is where the setter sets the ball...
**Choice of Set**
```
- oh: Outside
- rs: Rightside
- m1: Middle quick
- m2: 2-ball
- m3: Middle shoot
- 32: Outside 32
- bc: Bic (backrow-quick)
- db: D-Ball
- b1: Back-1
- sd: Slide
```

The next character is the sequence is the shot that the hitter makes...
**Shot Choice**
```
- l: Line
- c: Cross
- s: Straight
```

The next character is the result of the shot...
**Result of Shot**
```
- k: Kill
- e: Error
- a: Play continues
```

The last two characters in the sequence denote the player's number that hit the ball.

_The reason the player's number comes at the end of the sequence is to give the scouter time to see the
number on the players jersey when the ball is in play for the other team. The game moves too quickly for the scouter to find the players number first then remember the play they did after they log the number._

### Exceptions
During play some exceptions to the standard "bump, set, swing" action may occur. Typically this is either a dump by the setter or a free ball is given. To denote these actions...
#### Dump
For a dump by the setter, it is the same sequence as a swing
```
dp[l/c/s][k/e/a][0-9][0-9]

in other words

dp[SHOT][OUTCOME][PLAYER_NUM][PLAYER_NUM]
```

#### Free Ball or No Action
To denote a free ball or no action simple type 'no' and continue with the next sequence. No need to specify placement or outcome.
```
no
```

### Example
Player 16 serves a jump serve to spot 6 which results in the other team playing it out. The team then digs the ball and the setter sets 8 on the outside where he hits a line shot resulting in a kill.
```
16s6aohlk08
```

Player 4 is served a hybrid serve and passes a 2 to the setter. The setter (14) dumps the ball down the line. The play continues and following a dig, the setter sets the middle (18) a shoot where he hits cross court for an error.
```
h042dpla14m3ce18
```

## Invalid Lines
For invalid lines in the report, the program will mark the line as invalid, disregard the line and continue.
![Screenshot from 2023-08-24 15-10-55](https://github.com/ryanbyrne30/volleyball-scouting-reporter/assets/33855634/75a47714-7de7-4ded-8084-10336e491220)

## Example Response
For the very simple example
```
# 1
f103ohlk10
f101rsce10
```

This produces the following:
```
{
  "hitting": {
    "attempts": 2,
    "kills": 1,
    "errors": 1,
    "players": {
      "10": {
        "attempts": 2,
        "kills": 1,
        "errors": 1,
        "position": {
          "oh": {
            "attempts": 1,
            "kills": 1,
            "errors": 0,
            "shot": {
              "l": {
                "attempts": 1,
                "kills": 1,
                "errors": 0
              }
            }
          },
          "rs": {
            "attempts": 1,
            "kills": 0,
            "errors": 1,
            "shot": {
              "c": {
                "attempts": 1,
                "kills": 0,
                "errors": 1
              }
            }
          }
        }
      }
    }
  },
  "setting": {
    "total": 2,
    "rows": {
      "1": {
        "total": 2,
        "position": {
          "oh": {
            "total": 1,
            "track": 1
          },
          "rs": {
            "total": 1,
            "track": -1
          }
        },
        "rating": {
          "3": {
            "total": 1,
            "position": {
              "oh": {
                "total": 1,
                "track": 1
              }
            }
          },
          "1": {
            "total": 1,
            "position": {
              "rs": {
                "total": 1,
                "track": -1
              }
            }
          }
        }
      }
    }
  },
  "serving": {
    "attempts": 0,
    "track": 0,
    "aces": 0,
    "errors": 0,
    "spots": {},
    "players": {}
  },
  "passing": {
    "attempts": 2,
    "track": 4,
    "players": {
      "10": {
        "servetype": {
          "f": {
            "attempts": 2,
            "track": 4
          }
        },
        "attempts": 2,
        "track": 4
      }
    }
  }
}
```

As you can image, these reports can produce a lot of in depth data for an entire team. I commented out the portion of the program that delivers these reports to Google Sheets (API key is no longer active).
