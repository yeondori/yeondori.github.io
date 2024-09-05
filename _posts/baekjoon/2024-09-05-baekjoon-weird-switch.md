---
title: "[Baekjoon/Programmers] 25376 이상한 스위치"
excerpt: "프로그래머스 문제 풀이"
categories: [Baekjoon/Programmers]
tags: [Baekjoon/Programmers]
date: 2024-09-05
last_modified_at: 2024-09-05
render_with_liquid: false
---

[25376 이상한 스위치](https://www.acmicpc.net/problem/25376) 문제 해결 과정.

--- 

# 문제
스위치는 켜져 있는 상태와 꺼져 있는 상태로 존재한다. 여러분은 꺼져 있는 스위치만 눌러 스위치를 켤 수 있다. 켜져 있는 스위치를 직접 끌 수 없음에 유의하자.

그런데, 회로를 잘못 연결해버린 나머지 어떤 스위치를 직접 켤 때, 해당 스위치가 영향을 주는 다른 스위치들의 상태는 모두 반전된다. 즉, 영향을 받는 스위치는 켜져 있었다면 꺼지고, 꺼져있었다면 켜진다. 영향을 받아 간접적으로 켜지는 스위치는 직접 켜지는 스위치가 아니다.

스위치마다 영향을 주는 스위치의 목록이 주어지고 각 스위치의 초기 상태가 주어질 때, 모든 스위치를 켜기 위해 스위치를 눌러야 하는 최소 횟수를 출력하자.

# 해결

BFS + 비트마스킹  

1. 스위치 조작 상태를 bit 로 관리 
2. 스위치 On/Off 조작 -> XOR 연산 수행 
3. 현재 스위치 상태와 조작 횟수를 기반으로 BFS 수행
4. 스위치 조작시 연관된 모든 스위치 함께 조작

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class BOJ_25376 {

   static int switchNumber;
   static int switchStatus;
   static List<Integer>[] switches;

   static class Choice {
      int idx;
      int cnt;

      Choice(int idx, int cnt) {
         this.idx = idx;
         this.cnt = cnt;
      }
   }

   public static void main(String[] args) throws IOException {
      BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
      StringTokenizer st;

      switchNumber = Integer.parseInt(br.readLine());

      st = new StringTokenizer(br.readLine());
      for (int i = 0; i < switchNumber; i++) {
         if (st.nextToken().equals("1")) {
            switchStatus |= 1 << i;
         }
      }
      
      switches = new ArrayList[switchNumber];
      for (int i = 0; i < switchNumber; i++) {
         st = new StringTokenizer(br.readLine());
         int num = Integer.parseInt(st.nextToken());

         switches[i] = new ArrayList<>();
         for (int j = 0; j < num; j++) {
            int next = Integer.parseInt(st.nextToken()) - 1;
            switches[i].add(next);
         }
      }
      
      System.out.println(solve(switchStatus));
   }

   private static int solve(int start) {
      boolean[] visit = new boolean[1 << switchNumber];
      Queue<Choice> q = new LinkedList<>();
      q.add(new Choice(start, 0));
      visit[start] = true;

      while (!q.isEmpty()) {
         Choice cur = q.poll();
         int curBit = cur.idx;
         int curCnt = cur.cnt;

         if (curBit == (1 << switchNumber) - 1) {
            return curCnt;
         }

         for (int i = 0; i < switchNumber; i++) {
            if ((curBit & (1<< i)) != 0) continue;

            int next = curBit;
            next ^= (1 << i);
            for (int adj : switches[i]) {
               next ^= (1 << adj);
            }

            if (visit[next]) continue;
            q.add(new Choice(next, curCnt + 1));
            visit[next] = true;
         }
      }
      return -1;
   }
}

```
