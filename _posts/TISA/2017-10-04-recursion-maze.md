---
layout: post
title: "ch02 - Maze(미로찾기)"
categories:
  - 알고리즘
tags:
  - recursion
---

## Maze - 미로찾기
### Recursive Thinking
* 현재 위치에서 출구까지 가는 경로가 있으려면
  1. 현재 위치가 출구이거나 혹은
  2. 이웃한 셀들 중 하나에서 현재 위치를 지나지 않고 출구까지 가는 경로가 있거나

### 미로찾기(Decision Problem) - 답이 yes or no 인 문제
* 스도코드
```
  boolean findPath(x,y)
    if (x,y) is the exit
      return true;
    else
      mark (x,y) as a visited cell;
      for each neighbouring cell(x', y') of (x,y) do
        if (x', y') is on the pathway and not visited
          if findPath(x', y')
            return true;
      return false;
```
* 스도코드2
```
  boolean findPath(x,y)
    if (x,y) is either on the wall or a visited cell
      return false;
    else if (x,y) is the exit
      return true;
    else
      mark (x,y) as a visited cell;
      for each neighbouring cell (x', y') of (x, y) do
        if findPath(x', y')
          return true;
        return false;
```

* 미로찾기 구현 코드
```
public class Maze {
	private static int N = 8;
	private static int[][] maze = {
			{ 0, 0, 0, 0, 0, 0, 0, 1},
			{ 0, 1, 1, 0, 1, 1, 0, 1},
			{ 0, 0, 0, 1, 0, 0, 0, 1},
			{ 0, 1, 0, 0, 1, 1, 0, 0},
			{ 0, 1, 1, 1, 0, 0, 1, 1},
			{ 0, 1, 0, 0, 0, 1, 0, 1},
			{ 0, 0, 0, 1, 0, 0, 0, 1},
			{ 0, 1, 1, 1, 0, 1, 0, 0}
	};

	private static final int PATHWAY_COLOR = 0;
	private static final int WALL_COLOR = 1;
	private static final int BLOCKED_COLOR = 2;
	private static final int PATH_COLOR = 3;

	public static boolean findMazePath(int x, int y) {
		if ( x<0 || y<0 || x>=N || y>=N) {
			return false;
		} else if (maze[x][y] != PATHWAY_COLOR) {
			return false;
		} else if (x==N-1 && y==N-1) {
			maze[x][y] = PATH_COLOR;
			return true;
		} else {
			maze[x][y] = PATH_COLOR;
			if (findMazePath(x-1, y) || findMazePath(x, y+1) || findMazePath(x+1, y) || findMazePath(x, y-1)) {
				return true;
			}

			maze[x][y] = BLOCKED_COLOR;
			return false;

		}
	}

	public static void main(String [] args) {
		printMaze();
		findMazePath(0,0);		
		printMaze();
	}

	private static void printMaze() {

	}

}

```
