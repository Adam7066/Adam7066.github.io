---
slug: "Computer_programming_2-09"
title: "程式設計(二)-09：111-2 HW0204 參考作法"
date: 2023-04-25T01:56:39+08:00
tags:
    - C
    - Go
categories:
    - 程式設計(二)
---

# 題目
- {{< download pName="HW02" fPath="./hw02.pdf" dName="HW02.pdf" >}}
- {{< download pName="players_20.csv" fPath="./players_20.csv" dName="players_20.csv" >}}

# 參考作法
## C 語言
- 在 `main()` 中讀取 `file` 跟 `inSquad` 時偷懶，所沒用動態記憶體配置。
- 但在讀取 CSV 內容時，有使用動態記憶體配置呦！
- 然後可以的話，建議可以直接使用 `mmap()` 來讀取檔案，比較簡單，但當時同學還沒上到這個，就沒使用了。（攤～

### Makefile
- 因為有使用到 `Glib`，所以要加上一些東西。
```makefile
all:
	gcc hw0204.c `pkg-config --cflags --libs glib-2.0` -o hw0204
```

### 程式碼
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>
#include <ctype.h>
#include <glib.h>
#define bufSize 256

static char abilityStr[27][5] = {
    "ls", "st", "rs", "lw", "lf",
    "cf", "rf", "rw", "lam", "cam",
    "ram", "lm", "lcm", "cm", "rcm",
    "rm", "lwb", "ldm", "cdm", "rdm",
    "rwb", "lb", "lcb", "cb", "rcb",
    "rb", "gk"
};

typedef struct {
    char *sofifa_id, *short_name;
    double value_eur, wage_eur;
    char *club_name;
    GHashTable *ability;
    bool selected;
} Player;

static Player *player = NULL;
static size_t playerNum = 0;
static size_t playerSize = 0;
static char *cmpPosition;
static double budget[11] = {0};

void printTeam(Player *p, char *position) {
    printf("%s/%s/", p->short_name, p->club_name);
    double ability = GPOINTER_TO_INT(g_hash_table_lookup(p->ability, position));
    g_print("%g/", ability);
    printf("%d/%d\n", (int)(p->value_eur), (int)(p->wage_eur));
}

double getAbility(char *str) {
    double a = 0, b = 0;
    if(strchr(str, '+') != NULL) sscanf(str, "%lf+%lf", &a, &b);
    else sscanf(str, "%lf", &a);
    return a + b;
}

void parseLine(Player *p, char *token, int cnt) {
    if(cnt == 0 || cnt == 2 || cnt == 14) {
        char *str = calloc(strlen(token) + 1, sizeof(char));
        strcpy(str, token);
        if(cnt == 0) p->sofifa_id = str;
        else if(cnt == 2) p->short_name = str;
        else if(cnt == 14) p->club_name = str;
    }
    else if(cnt == 7 || cnt == 8) {
        char **endPtr = NULL;
        bool spaceFlag = true;
        size_t tokenLen = strlen(token);
        for(int i = 0; i < tokenLen; ++i) {
            if(!isspace(token[i])) {
                spaceFlag = false;
                break;
            }
        }
        double eur = spaceFlag ? -1 : strtod(token, endPtr);
        if(cnt == 7) p->value_eur = eur;
        else if(cnt == 8) p->wage_eur = eur;
    }
    else if(cnt >= 78 && cnt <= 104) {
        g_hash_table_insert(p->ability, abilityStr[cnt - 78], GINT_TO_POINTER(getAbility(token)));
    }
}

void getPlayer(FILE *fp) {
    char *line = calloc(bufSize, sizeof(char));
    size_t lineCnt = 0;
    while((line = fgets(line, bufSize, fp)) != NULL) {
        size_t len = strlen(line);
        if(len == bufSize - 1 && line[len - 1] != '\n') {
            int newLen = len << 1;
            line = realloc(line, newLen);
            if(!line) {
                printf("realloc failed\n");
                exit(EXIT_FAILURE);
            }
            while(fgets(line + len, newLen - len, fp) != NULL) {
                len = strlen(line);
                if(line[len - 1] == '\n') break;
                newLen = len << 1;
                line = realloc(line, newLen);
                if(!line) {
                    printf("realloc failed\n");
                    exit(EXIT_FAILURE);
                }
            }
        }

        char *find = strstr(line, ",,");
        while(find != NULL) {
            char *tmp = calloc(strlen(line) + 2, sizeof(char));
            tmp[strlen(line) + 1] = '\0';
            strncpy(tmp, line, find - line + 1);
            tmp[find - line + 1] = ' ';
            strncat(tmp + (find - line) + 2, find + 1, strlen(find + 1));
            free(line);
            line = tmp;
            find = strstr(line, ",,");
        }

        if(lineCnt == 0) {
            ++lineCnt;
            continue;
        }
        if(playerNum == playerSize) {
            playerSize = playerSize == 0 ? 1 : playerSize << 1;
            player = realloc(player, playerSize * sizeof(Player));
            if(!player) {
                printf("realloc failed\n");
                exit(EXIT_FAILURE);
            }
            for(size_t i = playerNum; i < playerSize; ++i) {
                player[i].ability = g_hash_table_new(g_str_hash, g_str_equal);
                player[i].selected = false;
            }
        }
        int cnt = 0;
        char *token = strtok(line, ",");
        while(token != NULL) {
            if(token[0] == '\"') {
                char *tmp = calloc(strlen(token) + 1, sizeof(char));
                strncpy(tmp, token + 1, strlen(token));
                tmp[strlen(tmp)] = ',';
                token = strtok(NULL, "\"");
                tmp = realloc(tmp, strlen(tmp) + strlen(token) + 1);
                strncat(tmp, token, strlen(token));
                tmp[strlen(tmp)] = '\0';
                parseLine(&player[playerNum], tmp, cnt);
                free(tmp);
            }
            else parseLine(&player[playerNum], token, cnt);
            token = strtok(NULL, ",");
            ++cnt;
        }
        ++lineCnt;
        ++playerNum;
    }
    free(line);
}

int cmpDouble(const double a, const double b, bool flag) {
    if(flag) return (a > b) ? -1 : (a < b) ? 1 : 0; // greater
    return (a < b) ? -1 : (a > b) ? 1 : 0; // less
}

int cmpString(const char *a, const char *b, bool flag) {
    if(flag) return strcmp(b, a);
    return strcmp(a, b);
}

int cmpAbility(const void *a, const void *b) {
    const Player *c = (Player *)a;
    const Player *d = (Player *)b;
    return cmpDouble(
               GPOINTER_TO_INT(g_hash_table_lookup(c->ability, cmpPosition)),
               GPOINTER_TO_INT(g_hash_table_lookup(d->ability, cmpPosition)),
               true
           );
}

int cmpBudget(const void *a, const void *b) {
    Player *c = (Player *)a;
    Player *d = (Player *)b;
    return cmpDouble(c->value_eur, d->value_eur, false);
}

int cmpSofifaId(const void *a, const void *b) {
    Player *c = (Player *)a;
    Player *d = (Player *)b;
    return cmpString(c->sofifa_id, d->sofifa_id, false);
}

void getTeam(char *position, int pos) {
    qsort(player, playerNum, sizeof(Player), cmpBudget);
    Player *tmpPlayer = NULL;
    size_t tmpPlayerNum = 0;
    size_t tmpPlayerSize = 0;
    for(int i = 0; i < playerNum; ++i) {
        if(!player[i].selected && player[i].value_eur >= 0 && player[i].value_eur <= budget[pos]) {
            if(tmpPlayerNum == tmpPlayerSize) {
                tmpPlayerSize = tmpPlayerSize == 0 ? 1 : tmpPlayerSize << 1;
                tmpPlayer = realloc(tmpPlayer, tmpPlayerSize * sizeof(Player));
                if(!tmpPlayer) {
                    printf("realloc failed\n");
                    exit(EXIT_FAILURE);
                }
            }
            tmpPlayer[tmpPlayerNum++] = player[i];
        }
    }
    if(tmpPlayerNum == 0) {
        printf("No player can be selected\n");
        return;
    }
    cmpPosition = position;
    qsort(tmpPlayer, tmpPlayerNum, sizeof(Player), cmpAbility);
    size_t sameAbilityNum = 1;
    double maxAbility = GPOINTER_TO_INT(g_hash_table_lookup(tmpPlayer[0].ability, position));
    for(int i = 1; i < tmpPlayerNum; ++i) {
        double ability = GPOINTER_TO_INT(g_hash_table_lookup(tmpPlayer[i].ability, position));
        if(ability == maxAbility) ++sameAbilityNum;
        else break;
    }
    if(sameAbilityNum == 1) printTeam(&tmpPlayer[0], position);
    else {
        qsort(tmpPlayer, sameAbilityNum, sizeof(Player), cmpBudget);
        size_t sameBudgetNum = 1;
        double minBudget = tmpPlayer[0].value_eur;
        for(int i = 1; i < sameAbilityNum; ++i) {
            if(tmpPlayer[i].value_eur == minBudget) ++sameBudgetNum;
            else break;
        }
        if(sameBudgetNum == 1) printTeam(&tmpPlayer[0], position);
        else {
            qsort(tmpPlayer, sameBudgetNum, sizeof(Player), cmpSofifaId);
            printTeam(&tmpPlayer[0], position);
        }
    }
    for(int i = 0; i < playerNum; ++i) {
        if(strcmp(player[i].sofifa_id, tmpPlayer[0].sofifa_id) == 0) {
            player[i].selected = true;
            break;
        }
    }
    free(tmpPlayer);
}

int main() {
    char file[100];
    printf("Please enter the dataset: ");
    fgets(file, 100, stdin);
    if(file[strlen(file) - 1] == '\n') file[strlen(file) - 1] = '\0';
    FILE *fp = fopen(file, "rt");
    if(!fp) exit(EXIT_FAILURE);
    getPlayer(fp);
    fclose(fp);

    char inSquad[100];
    printf("Please enter the squad: ");
    fgets(inSquad, 100, stdin);
    if(inSquad[strlen(inSquad) - 1] == '\n') inSquad[strlen(inSquad) - 1] = '\0';
    char **squad = calloc(11, sizeof(char *));
    int squadNum = 0;
    char *token = strtok(inSquad, " ");
    while(token != NULL) {
        squad[squadNum] = calloc(strlen(token) + 1, sizeof(char));
        strncpy(squad[squadNum], token, strlen(token));
        ++squadNum;
        token = strtok(NULL, " ");
    }
    if(strcmp(squad[10], "gk") != 0) {
        printf("gk must be the last one of the team.\n");
        exit(EXIT_FAILURE);
    }

    printf("Budget:\n");
    for(int i = 0; i < 11; ++i) {
        printf("\t%s: ", squad[i]);
        scanf("%lf", &budget[i]);
    }

    printf("Team:\n");
    for(int i = 0; i < 11; ++i) {
        printf("\t%s: ", squad[i]);
        getTeam(squad[i], i);
    }
    return 0;
}
```

## Golang
### 程式碼
```go
package main

import (
	"encoding/csv"
	"fmt"
	"os"
	"sort"
	"strings"
)

var abilityStr = []string{
	"ls", "st", "rs", "lw", "lf",
	"cf", "rf", "rw", "lam", "cam",
	"ram", "lm", "lcm", "cm", "rcm",
	"rm", "lwb", "ldm", "cdm", "rdm",
	"rwb", "lb", "lcb", "cb", "rcb",
	"rb", "gk",
}

type Player struct {
	sofifa_id, short_name string
	value_eur, wage_eur   int
	club_name             string
	ability               map[string]int
	selected              bool
}

var players []Player

func readCSV(csvFile string) {
	f, err := os.Open(csvFile)
	if err != nil {
		panic(err)
	}
	defer f.Close()
	csvReader := csv.NewReader(f)
	title, err := csvReader.Read()
	if err != nil {
		panic(err)
	}
	for {
		record, err := csvReader.Read()
		if err != nil {
			break
		}
		player := Player{}
		player.ability = make(map[string]int)
		for i, v := range record {
			if title[i] == "sofifa_id" {
				player.sofifa_id = v
			} else if title[i] == "short_name" {
				player.short_name = v
			} else if title[i] == "value_eur" {
				if v != "" {
					fmt.Sscanf(v, "%d", &player.value_eur)
				} else {
					player.value_eur = -1
				}
			} else if title[i] == "wage_eur" {
				if v != "" {
					fmt.Sscanf(v, "%d", &player.wage_eur)
				} else {
					player.wage_eur = -1
				}
			} else if title[i] == "club_name" {
				player.club_name = v
			}
			for _, ability := range abilityStr {
				if title[i] == ability {
					var a, b int
					if strings.Contains(v, "+") {
						fmt.Sscanf(v, "%d+%d", &a, &b)
						player.ability[ability] = a + b
					} else if v != "" {
						fmt.Sscanf(v, "%d", &a)
						player.ability[ability] = a
					} else {
						player.ability[ability] = -1
					}
				}
			}
		}
		players = append(players, player)
	}
}

func printPlayer(squad string, player Player) {
	fmt.Println(
		squad, ": ",
		player.short_name, "/",
		player.club_name, "/",
		player.ability[squad], "/",
		player.value_eur, "/",
		player.wage_eur,
	)
}

func main() {
	var csvFile string
	fmt.Scanf("%s", &csvFile)
	readCSV(csvFile)
	var squad []string
	for i := 0; i < 11; i++ {
		var id string
		fmt.Scanf("%s", &id)
		squad = append(squad, id)
	}
	var budget []int
	for i := 0; i < 11; i++ {
		var b int
		fmt.Scanf("%d", &b)
		budget = append(budget, b)
	}
	for i, id := range squad {
		tmpPlayers := make([]Player, 0)
		for _, player := range players {
			if !player.selected && player.value_eur >= 0 && player.value_eur <= budget[i] {
				tmpPlayers = append(tmpPlayers, player)
			}
		}
		if len(tmpPlayers) == 0 {
			fmt.Println("No player can be selected")
		}
		sort.Slice(tmpPlayers, func(i, j int) bool {
			return tmpPlayers[i].ability[id] > tmpPlayers[j].ability[id]
		})
		cntSameAbility := 0
		maxAbility := tmpPlayers[0].ability[id]
		for _, player := range tmpPlayers {
			if player.ability[id] == maxAbility {
				cntSameAbility++
			} else {
				break
			}
		}
		if cntSameAbility == 1 {
			printPlayer(id, tmpPlayers[0])
		} else {
			sort.Slice(tmpPlayers[:cntSameAbility], func(i, j int) bool {
				return tmpPlayers[i].value_eur < tmpPlayers[j].value_eur
			})
			cntSameBudget := 0
			minBudget := tmpPlayers[0].value_eur
			for _, player := range tmpPlayers[:cntSameAbility] {
				if player.value_eur == minBudget {
					cntSameBudget++
				} else {
					break
				}
			}
			if cntSameBudget == 1 {
				printPlayer(id, tmpPlayers[0])
			} else {
				sort.Slice(tmpPlayers[:cntSameBudget], func(i, j int) bool {
					return tmpPlayers[i].sofifa_id < tmpPlayers[j].sofifa_id
				})
				printPlayer(id, tmpPlayers[0])
			}
		}
		for j := 0; j < len(players); j++ {
			if players[j].sofifa_id == tmpPlayers[0].sofifa_id {
				players[j].selected = true
			}
		}
	}
}
```
