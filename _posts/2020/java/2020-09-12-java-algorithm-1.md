---
title: JAVA 알고리즘 1 (Algorithm)
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- JAVA
description: 알고리즘이란 무엇인지 같이 공부해보겠습니다.
article_tag1: 자료구조
article_section: Structure
meta_keywords: 알고리즘, 보글게임, 완전 탐색, 순열
last_modified_at: '2020-09-12 14:00:00 +08000'
toc: true
toc_sticky: true
toc_label: 목차
---
# Step 1 : 알고리즘(Algorithm)란 무엇인가?
알고리즘은 어떠한 입력이 있다면 이 입력에 따라 명령을 명확하게 실행하고, 효과적으로 입력에 따른 결과물을 도출할 수 있다면 알고리즘으로 볼 수 있습니다. 
즉 특정 원하는 결과를 도출하기 위해 처리하는 의사결정 과정의 코드를 알고리즘이라 칭할 수 있습니다. 
알고리즘은 무궁무진하며 지금부터는 이 문제 저 문제 풀어보면서 알게 된 기본적인 알고리즘을 정리하겠습니다.

# 완전 탐색 ( Brute - Force )
완전 탐색이란 사람이 손으로 하기엔 오래 걸리는 일을 컴퓨터의 힘을 빌려 모든 경우의 수를 계산하여 원하는 결과를 탐색하는 방법입니다. 즉 무식하게 처리하지만 단순하고 강력한 방법이기도 합니다. 

## 재귀 함수
재귀 함수이란 컴퓨터가 수행할 작업 중 반복되는 것을 작업 단위로 쪼개어 한 작업을 실행 후 나머지 작업을 자기 자신에게 호출하는 하여 결과를 완성하는 것을 말합니다. 주로 완전탐색에서 자주 사용되는 방법입니다. 

### 순열 ( Permutation )
순열이란 n 개의 값 중에서 r 개를 순서대로 뽑는 경우를 말합니다. 순열을 풀기 위해서 2가지 방법이 존재합니다.

<p class="bold-text">1)중첩 for문 사용</p>
<p class="bold-text">2)재귀호출 사용해서 구현</p>

** String n 크기의 배열이 들어 왔을 때, 3개를 순서대로 뽑는 경우의 수를 한번 만들어 보겠습니다. 

```java
//ex 123, 124..
String[] people = {"1","2","3","4","5","6"};
result(people);
```

#### 단순 무식하게 중첩루프 사용
풀이는 간단합니다. 중첩 루프를 뽑는 개수만큼 만들고 한번 나왔던 것을 제거하고 도는 것 입니다. 
```java
private static void result( String[] people ) {
    int count = 0;
    for( int firstIndex = 0; firstIndex < people.length; firstIndex++ ) {
        for( int secondIndex = 0; secondIndex < people.length; secondIndex++ ) {
            
            if( firstIndex == secondIndex ) continue;
            
            for( int thirdIndex = 0; thirdIndex < people.length; thirdIndex++ ) {
                if( thirdIndex == secondIndex || thirdIndex == firstIndex ) continue;
                
                String first = people[firstIndex];
                String second= people[secondIndex];
                String third = people[thirdIndex];
                count++;
                System.out.println("( "+first +" " + second + " " + third +" )");
            }
        }
    }
    System.out.println("총 경우의 수 : " + count);
}

```



#### 재귀함수로 구현
위로 푼 방법과 유사합니다. 다만 좀 더 코드를 줄일 수 있는 장점이 있습니다. 미리 3개를 뽑는 것이니 크기가 3인 result 배열을 만들어 두고 앞에부터 채워가는데 기존 나왔던 것을 제외하고 채워가는 것입니다. 
```java
private static void result( String[] people ) {
    int r = 3;
    boolean[] isChecked = new boolean[people.length];
    String[] result = new String[r];
    ArrayList<String[]> totalList = new ArrayList<String[]>();
    
    permutation(people, isChecked, result, r, 0, totalList);
    
    for (String[] strings : totalList) {
        String temp = "";
        for( String text : strings ) {
            temp += " " + text;
        }
        System.out.println(temp);
    }
    System.out.println("총 경우의 수 : " + totalList.size());
}

private static void permutation( String[] people, boolean[] isChecked, String[] result, int endPoint, int dept, ArrayList<String[]> totalList ) {
    if( endPoint == dept ) {
        totalList.add(result.clone());
    } else {
        for ( int i = 0; i < people.length; i++ ) {
            if( !isChecked[i] ) {
                isChecked[i] = true; // 사용된 배열 위치
                result[dept] = people[i]; // 저장 
                permutation(people, isChecked, result, endPoint, dept + 1, totalList);
                isChecked[i] = false; // 사용된 것 다시 제자리
                result[dept] = ""; // 저장된 것 제자리
            }
        }
    }
}
```


### 조합
조합이란 n 개의 값 중에서 r 개를 뽑는 경우를 말합니다.( 123, 321은 같은 것입니다. ) 조합을 풀기 위해서 순열처럼 무식한 방법과 재귀를 이용한 방법이 존재합니다. 

#### 중첩 for문 사용
```java
private static void result( String[] people ) {
    int count = 0;
    for( int firstIndex = 0; firstIndex < people.length; firstIndex++ ) {
        for( int secondIndex = firstIndex+1; secondIndex < people.length; secondIndex++ ) {
            for( int thirdIndex = secondIndex+1; thirdIndex < people.length; thirdIndex++ ) {
                String first = people[firstIndex];
                String second= people[secondIndex];
                String third = people[thirdIndex];
                count++;
                System.out.println("( "+first +" " + second + " " + third +" )");
            }
        }
    }
    System.out.println(count);
  
}
```

#### 재귀호출 사용해서 구현
무한 루프에서 한 것을 똑같이 사용하는 것을 단지 재귀로 호출하는 방법입니다. 최종 결과를 저장할 result[]를 만들고 전달하였습니다. 
```java
private static void result( String[] people ) {
    int end = 3;
    int start  = 0;
    String[] tempResult = new String[end];
    int loopStartIndex = 0; 
    ArrayList<String[]> result = new ArrayList<String[]>();
    combination( people, end, start, tempResult, result, loopStartIndex );
    for (String[] strings : result) {
        String temp = "";
        for (String strings2 : strings) {
            temp += " " + strings2;
        }
        System.out.println(temp);
    }
    System.out.println(result.size());
    
}

private static void combination( String[] people, int end, int start, String[] tempResult, ArrayList<String[]> result, int loopStartIndex ) {
    if( end == start ) {
        result.add(tempResult.clone());
        return;
    }
    
    for( int index = loopStartIndex; index < people.length; index++ ) {
            tempResult[start] = people[index];
            combination(people, end, start+1, tempResult, result, index+1);
            tempResult[start] = "";
    }
}

```

### 보글게임
보글게임이란 n x n 격자에 한 단어씩 적혀있는 격자가 2차원 배열로 주어질 때, 해당 격자에서 원한는 단어를 찾을 수 있을 때, true를 찾을 수 없을 
때 false를 return하는 프로그램입니다. 직관적으로 특정 알고리즘 없이 완전 탐색으로 푼다면, 모든 격자를 시작점으로 인접 8칸에 원하는 단어가 
있는지 확인하는 것입니다.


```java
{% raw %}
public static void main(String args[]){
    String[][] word = {
                        {"N","N","N","N","S"},
                        {"N","E","E","E","N"},
                        {"N","E","Y","E","N"},
                        {"N","E","E","E","N"},
                        {"N","N","N","N","N"}
                    };

    System.out.println(findWord( word, "YES" ));
}
{% endraw %}
```

```java
private static boolean findWord(String[][] word, String findText) {
    //1. 종료 조건 현재 위치가 마지막이고, 마지막 단어와 매칭 여부
    //2. return 단어 존재 여부 boolean
    //3. 한번 온 곳은 다시 가면 안됨.
    boolean result = false;
    boolean[][] checking = new boolean[word.length][word.length];
    
    out : for (int yIndex = 0; yIndex < word.length; yIndex++) {
        for (int xIndex = 0; xIndex < word.length; xIndex++) {
            if( result ) {
                break out;
            } else {
                result = checking( word, findText, 0, yIndex, xIndex , checking);
            }
        }
    }
    
    
    return result;
}

private static boolean checking(String[][] word, String findText, int checkingIndex, int yIndex, int xIndex, boolean[][] checking) {
    boolean result = false;
    int[] yCheck = {-1, -1, -1, 0, 0, 1, 1, 1};
    int[] xCheck = {-1, 0, 1, -1, 1, -1, 0, 1};
    
    if( yIndex == word.length || yIndex < 0)  return false;
    if( yIndex == word.length || yIndex < 0) return false;
    if( xIndex == word.length || xIndex < 0) return false;
    if( checking[yIndex][xIndex] ) return false;
    
    String checkText = findText.substring(checkingIndex, checkingIndex+1);
    String wordText = word[yIndex][xIndex];
    
    if( !checkText.equals(wordText) ) {
        return false;
    }
    
    if( findText.length()-1 == checkingIndex ) {
       return checkText.equals(wordText);
    }
    
    // 8방면 검사 
    for (int cIndex = 0; cIndex < 8; cIndex++) {
        checking[yIndex][xIndex] = true;
        result = checking(word, findText, checkingIndex+1, yIndex+yCheck[cIndex], xIndex+xCheck[cIndex], checking);
        checking[yIndex][xIndex] = false;
        if( result ) {
            return true;
         }
    }
    
    return  result;
}
```

### 소풍
만약 n명의 사람이 소풍을 놀러가서 2명씩 짝을 지어야 하는데, 서로 친구라는 단서가 배열로 주어질 때, 서로 친구인 경우만 짝을 짓는 케이스를 출력하는 
프로그램을 짠다고 생각해봅시다.

```java
{% raw %}
String[] name = {"석진", "우리", "현식", "희범","성우","밥"};
int[][] friend = {{0,1},{0,2},{1,2},{1,3},{1,4},{2,3},{2,4},{3,4},{3,5},{4,5}};
{% endraw %}
check( name, friend );
```
완전 탐색을 통해 짝을 짓는 방법은 한명씩 짝을 구성하면서 둘이 친구인 지를 확인하여 친구인 경우만 결과에 추가하는 방법입니다. 
짝이 있는 경우는 넘어가면서 다음 짝을 찾는 경우를 찾으면 됩니다.

```java
private static void check(String[] name, int[][] friend) {
    // 사람 수와 친구 쌍이 주어졌을 때, 친구끼리 쌍을 짓는 경우의 수 도출
    // 2명을 짝을 짓고, 친구인지 확인, 친구인 경우 제거, 나머지로 반복한다. 전체를 다 돌았음에도 짝을 못짓는 경우는 불가능 하다.
    // 친구 여부를 확인하기 위한 2차원 boolean 배열 [0][1] = true일 시 둘은 친구이다. 
    boolean[][] isFriend = new boolean[name.length][name.length];
    for( int[] fi : friend ) {
        isFriend[fi[0]][fi[1]] = true;
        isFriend[fi[1]][fi[0]] = true;
    }
    
    // 종료 조건 친구 쌍을 다 찾았을 때 >> 남은 친구가 없을 때 사람수가 0일때.
    // return 친구 쌍 조합을 ArrayList
    // 확인 여부를 위한 캐쉬
    ArrayList<String> result = new ArrayList<>();
    boolean[] check = new boolean[name.length];
    StringBuilder temp = new StringBuilder();
    compair( name.length, result, check, temp, isFriend, name);
    System.out.println(result.size());
}

private static void compair( int length, ArrayList<String> result, boolean[] check, StringBuilder temp, boolean[][] isFriend, String[] name) {
    if( length == 0 ) {
        result.add(temp.toString());
        return;
    }
    int findIndex = 0;
    for( int index = 0; index < check.length; index++ ) {
        if( !check[index] ) {
            findIndex = index;
            break;
        }
    }
    // 자기 다음부터 찾아 사전식으로 구성하여 중복을 제거한다.
    for( int second = findIndex+1; second < name.length; second++  ) {
        if( !check[second] && isFriend[findIndex][second] ) {
            StringBuilder copy = new StringBuilder(temp.toString());
            check[findIndex] = true;
            check[second] = true;
            temp.append("( "+name[findIndex] + " ");
            temp.append(name[second] + " )");
            compair(length-2, result, check, temp, isFriend, name);
            check[findIndex] = false;
            check[second] =false;
            temp = copy;
        }
    }
}
```


**참고자료** <br> <br>
-- 인사이트 - 프로그래밍대회에서 배우는 알고리즘 문제해결 전략( 저자 - 구종만 ) ( C언어 ) <br> 
{: .notice--info}