---
layout: post
title: "Programmers-올바른 괄호
"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 올바른 괄호 문제에 관한 설명입니다.<br>

---

### 문제

괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 

예를 들어

()() 또는 (())() 는 올바른 괄호입니다.<br>
)()( 또는 (()( 는 올바르지 않은 괄호입니다.<br>
'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때,<br>
문자열 s가 올바른 괄호이면 true를 return 하고,<br>
올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.

제한사항
문자열 s의 길이 : 100,000 이하의 자연수<br>
문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.<br>

입출력 예 1

()()	true
(())()	true
)()(	false
(()(	false

```java

import java.util.Stack;

public class Solution {
    public boolean solution(String s) {
        boolean answer = false;
        Stack<Character> parenthesis = new Stack<Character>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                parenthesis.add(s.charAt(i));
            } else {
                if (parenthesis.isEmpty()) {
                    return answer;
                }
                parenthesis.pop();
            }
        }
        if (parenthesis.isEmpty())
            answer = true;
        return answer;
    }
}

```

### 코드 설명

1.(가 들어왔는지를 확인합니다. (가 들어오면 스택에 추가합니다.

2.)가 들어온다면 pop을 시킵니다. pop이 불가능하거나 횟수가 맞지 않는다면 false가 return됩니다.

3.만약 올바르다면 answer는 true로 출력이 나타납니다.

위 방식대로 풀었을 때 효율성테스트 부분에서 같은코드임에도 통과, 통과X를 번갈아 하는 테스트케이스가 있었습니다.

그래서 아래의  stack을 사용하지 않고 변수로 풀어낸 문제방식을 이용했습니다. 

```java

public class Solution {
    public boolean solution(String s) {
    	
    	int ret = 0;
    	int len = s.length();
    	
    	for(int i = 0; i < len; i++) {
    		char curr = s.charAt(i);
    		
    		switch(curr) {
    			case '(':
    				ret++;
    				break;
    			case ')':  
    				ret--;
    				break; 
    		}
    		
    		if (ret < 0) break;
    		
    	}
    	return (ret == 0);
    }
}

```

---
