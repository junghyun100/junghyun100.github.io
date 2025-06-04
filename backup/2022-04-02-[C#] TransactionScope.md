---
layout: post
title: "[C#] TransactionScope"
tags: [C#]
comments: true

---

C#에서의 암시적으로 트랜잭션 처리를 할 수 있도록 도와주는

TrasactionScope에 대해 정리한다.

---

# TransactionScope

업무 중 분산DB 환경에서 DB1에 update 작업 성공 후 API를 통해 DB2에 반영을 하는 과정에서 Fail이 발생했다. 그럴 경우 DB1에는 값이 update는 성공 되었으나, DB2에는 반영이 되지 않는 케이스가 나타난다. ****Transaction****를 적절히 사용하면 이것을 막을 수 있다.

JAVA는 스프링에서는 `@Transactional`어노테이션을 이용한 선언적 트랜잭션 처리를 지원한다. 

> 이 부분에 대해서는 따로 더 공부를 해야한다.

C#에서는 Transaction 클래스가 있다.

```java
public class Transaction : IDisposable, System.Runtime.Serialization.ISerializable
```

네임스페이스: System.Transactions를 사용하며Transaction 클래스 기반의 명시적 프로그래밍 모델과 TransactionScope 클래스를 사용하는 암시적 프로그래밍 모델을 둘 다 제공한다.

TransactionScope 클래스는 코드 블록을 트랜잭션에 참여하는 것으로 트랜잭션 범위는 자동으로 트랜잭션을 선택하고 관리할 수 있다. 사용하기 쉽고 효율적이므로 트랜잭션 애플리케이션을 개발할 때는 TransactionScope 클래스를 사용하는 것이 좋다.

```java
static public int CreateTransactionScope(
    string connectString1, string connectString2,
    string commandText1, string commandText2)
{
    int returnValue = 0;
    System.IO.StringWriter writer = new System.IO.StringWriter();

    try
    {
				 // 트랜잭션 범위 시작
         using (TransactionScope scope = new TransactionScope())
        {
						// doSomething Start
            using (SqlConnection connection1 = new SqlConnection(connectString1))
            {
                connection1.Open();
                SqlCommand command1 = new SqlCommand(commandText1, connection1);
                returnValue = command1.ExecuteNonQuery();
                writer.WriteLine("Rows to be affected by command1: {0}", returnValue);
                using (SqlConnection connection2 = new SqlConnection(connectString2))
                {
                    connection2.Open();
                    returnValue = 0;
                    SqlCommand command2 = new SqlCommand(commandText2, connection2);
                    returnValue = command2.ExecuteNonQuery();
                    writer.WriteLine("Rows to be affected by command2: {0}", returnValue);
                }
            }
						// doSomething End
            scope.Complete();
        }
    }
    catch (TransactionAbortedException ex)
    {
        writer.WriteLine("TransactionAbortedException Message: {0}", ex.Message);
    }
    Console.WriteLine(writer.ToString());
    return returnValue;
}
```

### 범위 설정

새 TransactionScope 개체를 만들면 트랜잭션 범위가 시작된다. 
예시처럼 `using`을 사용하여 범위를 설정하는 것이 좋다. 

### 범위 완료 알리기

애플리케이션이 트랜잭션에서 수행할 작업을 모두 완료하면 Complete메서드를 호출하여 트랜잭션 커밋이 허용됨을 트랜잭션 관리자에게 알려야 한다. Complete 메서드를 호출한 후에는 더 이상 Current속성을 통해 앰비언트 트랜잭션에 액세스할 수 없으며 액세스를 시도하면 예외가 throwe된다.

### 롤백

트랜잭션을 롤백하려면 트랜잭션 범위 내에서 Complete 메서드를 호출하면 안 된다. 예를 들어 범위 내에서 예외를 throw할 수 있다. 범위가 참여하는 트랜잭션이 롤백된다.

### 참고링크 : 

<a href="https://docs.microsoft.com/ko-kr/dotnet/framework/data/transactions/implementing-an-implicit-transaction-using-transaction-scope">MS - .Net Framework 공식문서</a>

---
