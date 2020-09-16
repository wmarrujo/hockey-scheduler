# Model

This model is to be solved using an integer programming solver such as [Cbc](https://projects.coin-or.org/Cbc) used in this program.

## Notes

Ice Times are the combination of a specific time and a rink to be played on. Thus, you can have two teams with the same time reserved, but on different rinks.

## Assumptions

- no two teams can have the same ice time. (each team owns their ice times)

## Sets

- $T$ = teams
- $I$ = ice times (the possible times that games can be played)

## Values

- $H_{t,i}$ = does the ice time $i$ belong to team $t$? ($\in [0,1]$)
- $A_{t,i}$ = is team $t$ available to play on ice time $i$? ($\in [0,1]$)
- $D_i$ = days since first ice time of ice time $i$. ($\in \Bbb{Z}^+$)
- $W_i$ = weeks since first ice time of ice time $i$. (week number splits on Sunday → Monday) ($\in \Bbb{Z}^+$)

## Variables

- $P_{t,i}$ = whether team $t$ plays on ice time $i$ ($\in [0,1]$)

## Objective

Try to play all games as early as possible

Minimize:
$$
\sum_{(t,i) \in T \times I} (P_{t,i} * D_i)
$$

## Constraints

### Conservation of Teams

Teams can only play a maximum of 1 game per day. Also, teams can only play when they're available.

$$
\sum_{n \in I : D_i = D_n}(P_{t,n}) \leq A_{t,i}
\quad \forall (t,i) \in T \times I
$$

### Matchup

Teams can only host one other team at a time. This also means that games can only be played when there is a host, and that that host must have ice time then.

$$
P_{h,i} = \sum_{a \in T : h \neq a}(P_{a,i})
\quad \forall (h,i) \in T \times I : H_{h,i}
$$

### Round-Robin

Each team must host each other team at least once.

$$
\sum_{i \in I : H_{h,i} = 1}(P_{a,i}) \geq
\quad \forall (h,a) \in T \times T : h \neq a
$$

### Covid

Only 2 games can be played in the same week.

$$
\sum_{n \in I : W_i = W_n}(P_{t,n}) \leq 2
$$