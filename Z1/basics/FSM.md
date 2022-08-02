---
sort: 4
---
# 有限状态机

机械臂的一个状态机，即对应着一个功能。z1机械臂的状态机说明如下表格：

|State|KeySwitch|Switchable|
|:-:|:-:|:-:|
|BACKTOSTART|~|1 2|
|PASSIVE|1|~ 2 3 =|
|JOINTCTRL|2|~ 1 3 4 5 6 7 8 9 0 -|
|CARTESIAN|3|~ 1 2 4 5 6 9|
|MoveJ|4|~ 1 2 3 5 6 9|
|MoveL|5|~ 1 2 3 4 6 9|
|MoveC|6|~ 1 2 3 4 5 9|
|TEACH|7|~ 1 2|
|TEACHREPEAT|8|automatically switched to 2|
|SAVESTATE|9|automatically switched to 2|
|TOSTATE|0|automatically switched to 2|
|TRAJECTORY|-|~ 1 2|
|CALIBRATION|=|automatically switched to 2|

## BACKTOSTART

所有电机返回初始位置
## PASSIVE
所有电机进入阻尼状态（上电后的默认状态）
## JOINTCTRL

## CARTESIAN

## MoveJ

## MoveL

## MoveC

## TEACH

## TEACHREPEAT

## SAVESTATE

## TOSTATE

## TRAJECTORY

## CALIBRATION