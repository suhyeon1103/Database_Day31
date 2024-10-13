# MongoDB 기초 및 개념

1. **MongoDB는 어떤 유형의 데이터베이스이며, 어떤 형식으로 데이터를 저장합니까? MongoDB의 스키마 특성에 대해 설명하세요.**
   문서 지향 데이터 베이스로 NoSQL, JSON, BSON, 스키마레스 형식의 문서로 저장한다.
   MongoDB의 스키마 특성은 유연한스키마, 동적스키마, 중첩문서, 배열지원, 인덱스 지원이 있다.
2. **MongoDB에서 문서(Document)와 컬렉션(Collection)의 개념을 설명하고, SQL에서 테이블(Table)과의 차이점을 비교하세요.**
   문서는 MongoDB의 기본 단위로 JSON과 유사한 BSON 형식으로 데이터를 저장하고 동적인 스키마를 지원하여 같은 컬렉션 내에서도 서로 다른 구조를 가진 문서들이 존재 할수 있고,
   컬렉션은 문서의 집합으로 SQL의 테이블에 해당한다.
   SQL에서의 테이블은 관계형 데이터베이스에서 데이터를 저장하는 기본 단위이며, 고정된 스키마를 가지고 있으며 모에 행은 동일한 구조를 가져야한다.
   -SQL 테이블 NoSQL 컬렉션 거의 흡사
   -SQL ROW(행)는 NoSQL 문서(Document) 흡사
3. **MongoDB에서 스키마리스(Schemaless) 데이터베이스의 장점과 단점을 설명하세요.**
   장점은 스키마를 미리 정의할 필요가 없기 때문에 데이터 모델을 쉽게 변경할수 있고, 빠른 개발이 가능하며, 다양한 데이터 형식을 저장할 수 있고, 대량의 데이터를 처리하는데 효과적이다.
   단점은 스키마가 없기 때문에 데이터의 일관성을 보장하기 어렵고 데이터 구조가 유동적이기 때문에 복잡한 쿼리를 작성할 때 어려움이 있을 수 있으며 성능 저하가 발생할 수 있고, 데이터 관리 및 유지보수가 어렵다.
4. **MongoDB의 주요 특징 중 수평적 확장성에 대해 설명하고, 샤딩(Sharding)이 어떻게 수평적 확장을 지원하는지 서술하세요.**
   수평적 확장은 데이터 크기와 트래픽이 증가함에 따라 서버를 추가하여 성능을 향상시키는 방식이며, 데이터를 여러 DB에 분할하여 수평적으로 확장시킨다.
5. **MongoDB에서 BSON(Binary JSON) 형식이 사용되는 이유와 이 형식의 주요 특징에 대해 설명하세요.**
   데이터의 효율적인 저장이 가능하며 다양한 데이터 타입을 지원하고, 인덱싱을 지원하여 검색 성능을 향상시킨다.
   동적 스키마를 지원하여 데이터를 쉽게 추가하거나 수정할 수 있고 다양한 쿼리 기능을 활용할 수 있다.

# MongoDB 트랜잭션 관련

1. **MongoDB에서 멀티 도큐먼트 트랜잭션(Multi-Document Transaction)이 무엇이며, 어떻게 작동하는지 설명하세요.**
   MongoDB에서 멍티 도큐먼트 트랜잭션은 여러 도큐먼트에 걸쳐 원자적인 작업을 수행할 수 있는 기능이다. 이 기능은 여러 도큐먼트를 동시에 업데이트하거나 삽입할 때, 모든 작업이 성공적으로 완료되거나, 하나라도 실패할 경우 모든 작업이 롤백되도록 보장한다.
2. **MongoDB 트랜잭션의 ACID 속성(Atomicity, Consistency, Isolation, Durability)에 대해 설명하고, 각 속성이 트랜잭션에서 어떤 역할을 하는지 서술하세요.**
   Atomicity (원자성) - 모든 작업이 성공적으로 완료되거나, 하나라도 실패하면 전체 트랜잭션이 취소되야함
   Consistency (일관성) - 트랜잭션이 완료된 후 데이터베리스가 일관된 상태를 유지해야 함
   Isolation (격리성) - 동시에 실행되는 트랜잭션들이 서로에게 영향을 미치지않도록 보장함
   Durability (지속성) - 트랜잭션이 성공적으로 완료된 후 그 결과가 영구적으로 저장되어야 함
3. **MongoDB 트랜잭션 사용 시, 데이터 일관성을 보장하기 위해 사용하는 `commitTransaction()`과 `abortTransaction()` 메서드의 역할을 설명하세요.**
   트랜잭션이 성공하면 commitTransaction()으로 완료하고, 실패하면 abortTransaction()으로 취소할 수 있다.
4. **MongoDB에서 트랜잭션을 사용할 수 있는 환경을 설명하고, 단일 인스턴스에서 트랜잭션이 가능한지 여부를 기술하세요.**
   트랜잭션은 기본적으로 복제세트(Replica Set)에서 지원되고, MongoDB 4.0 이상의 버전에서 트랜잭션을 지원한다. MongoDB에서는 샤딩된 클러스터에서도 트랜잭션을 지원한다.
   MongoDB의 트랜잭션은 복제세트 환경에서만 지원되기 때문에, 단일 인스턴스에서는 트랜잭션을 사용할 수 없다.
5. **MongoDB에서 트랜잭션을 구현한 예시 코드를 제시하고, 각 단계별로 동작 방식을 설명하세요.**
   const { MongoClient } = require('mongodb');

async function transferFunds(client, fromAccountId, toAccountId, amount) {
const session = client.startSession();

    //트랜잭션 시작
    try {
        session.startTransaction();

        const accountsCollection = client.db("bank").collection("accounts");

        // 출금
        const withdrawResult = await accountsCollection.updateOne(
            { _id: fromAccountId },
            { $inc: { balance: -amount } },
            { session }
        );

        if (withdrawResult.modifiedCount === 0) {
            throw new Error("출금 실패: 계좌가 존재하지 않거나 잔액 부족");
        }

        // 입금
        const depositResult = await accountsCollection.updateOne(
            { _id: toAccountId },
            { $inc: { balance: amount } },
            { session }
        );

        if (depositResult.modifiedCount === 0) {
            throw new Error("입금 실패: 수신 계좌가 존재하지 않음");
        }
        //트랜잭션 커밋(성공)
        await session.commitTransaction();
        console.log("트랜잭션이 성공적으로 완료되었습니다.");
    }
        //트랜잭션 롤백(오류발생)
        catch (error) {
        console.error("트랜잭션 중 오류 발생:", error);
        await session.abortTransaction();
    }
        //세션 종료
        finally {
        session.endSession();
    }

}

// MongoDB 연결 및 함수 호출 예시
(async () => {
const client = new MongoClient("mongodb://localhost:27017");
await client.connect();

    await transferFunds(client, "account1", "account2", 100);

    await client.close();

})();

# 샤딩(Sharding) 관련

1. **MongoDB에서 샤딩(Sharding)이란 무엇이며, 이를 통해 데이터베이스 성능을 어떻게 최적화할 수 있는지 설명하세요.**
   샤딩은 데이터를 여러 서버(노드)에 분산하여 저장하는 방식이며 데이터양이 많아지거나 트래이 증가할 경우, 더 많은 서버를 추가하여 데이터 처리 능력을 확장할 수 있다.(시스템 성능 유지 가능) 각 샤드는 전체 데이터베이스의 일부만 저장하며, 이를 통해 데이터의 일부만을 조회하거나 수정하는 작업이 분산 처리된다.(성능 최적화)
2. **MongoDB에서 샤드 키(Shard Key)의 역할과, 샤드 키를 선택할 때 고려해야 할 요소들을 설명하세요.**
   샤드 키는 샤딩에서 데이터를 분배하는 방식을 결정하는 것이고, 샤드키를 선택할 때 성능, 데이터 분포, 쿼리 패턴, 확장성, 관리 용이성, 데이터 유형을 고려해야 한다.
3. **MongoDB에서 샤딩 구조의 주요 구성 요소(샤드, 쿼리 라우터, 컨피그 서버)를 설명하고, 각 구성 요소가 어떻게 상호작용하는지 서술하세요.**
   샤드 - 실제 데이터를 저장하는 데이터 베이스. 각 샤드는 전체 데이처의 일부를 저장하며, 샤드는 자체적으로 레플리카 세트로 구성될 수 있다.
   쿼리 라우터 - MongoS라는 프로세스가 쿼리 라우터 역할을 함. 클라이언트가 MongoDB에 요청을 보낼 때, 쿼리 라우터는 어떤 샤드가 해당 데이터를 가지고 있는지 결정하고 쿼리를 적절한 샤드에 전달한다.
   컨피그 서버 - 클러스터에서 데이터가 각 샤드에 어떻게 분배되는지를 추적하는 메타데이터를 저장. 컨피그 서버는 각 샤드가 어떤 데이터 범위를 가지고 있는지 관리한다.
4. **MongoDB에서 Range-based Sharding과 Hash-based Sharding의 차이점을 설명하고, 각각의 장단점을 비교하세요.**
   Range-based Sharding : 샤드키의 값 범위에 따라 데이터 분할. 증설작업에 드는 비용이 크지 않음.
   Hash-based Sharding : 샤드 키 값을 해상하여 데이터를 분산. 이 방식은 데이터를 고르게 분산할 수 있으며, 특정 샤드에 집중되는 현상을 방지한다. 샤드를 추가하거나 삭제하기 쉬움.
5. **MongoDB에서 샤딩의 장점과 단점을 설명하고, 샤딩이 적합한 상황과 그렇지 않은 상황에 대해 논하세요.**
   샤딩의 장점은 대량의 데이터를 여러 서버에 분산하여 저장하므로, 데이터의 크기가 커지거나 트래픽이 증가할 때 시스템성을을 유지할 수 있고, 데이터를 분산하여 저장하기 때문에, 쿼리가 특정 샤드에만 접근하게되어 전체 시스템의 처리 성능이 향상된다. 일부 샤드에 장애가 발생해도 다른 샤드들이 정상적으로 작동하며, 전체 시스템의 가용성을 유지할 수 있다.
   단점은 샤딩을 설정하고 관리하는 데 복잡성이 따르며, 잘못된 샤드 키 선택은 성능 문제를 일으킬 수 있다. 샤딩을 사용할 경우 더 많은 서버가 필요하므로, 하드웨어와 유지 관리 비용이 증가할 수 있다.
   대규모 데이터베이스, 높은 트래픽 처리, 성능 최적화를 해야할 경우 샤딩을 사용.

# 레플리카 세트(Replica Set) 관련

1. **MongoDB에서 레플리카 세트(Replica Set)란 무엇이며, 이 기능을 사용하는 주된 목적에 대해 설명하세요.**
   레플리카 세트(Replica Set)는 동일한 데이터를 가진 여러 MongoDB 인스턴스를 말한다.
   서비스중인 MongoDB 인스턴스에 문제가 생겼을 때, 레플리카세트의 구성원중의 하나인 복제 노드가 장애 노드를 즉시 대처한다.
2. **MongoDB 레플리카 세트의 구성 요소인 Primary, Secondary, Arbiter의 역할과 동작 방식을 설명하세요.**
   Primary : 데이터의 읽기와 쓰기 작업을 처리하는 메인 서버
   Secondary : Primary 서버에서 발생한 모든 데이터를 복제하는 서버
   Arbiter : 데이터 복제를 저장하지 않지만, Primary와 Secondary간의 투표에서 동점을 방지하기 위해 존재
   _동작방식_
   데이터 복제 : Primary 서버에서 발생한 모든 변경 사항은 Secondary 서버로 복제. 이 복제 작업은 비동기 방식으로 이루어지지만, 거의 실시간으로 동기화됨.
   장애 발생 시 : Primary 서버가 다운되면, 레플리카 세트는 자동으로 하나의 Secondary 서버를 새로운 Primary 서버로 승격시킴. 클라이언트는 새롭게 승격된 Primary 서버로 자동으로 전환됨.
   읽기 작업 : 읽기 작업은 Primary에서 수행될 수도 있고, Secondary 서버에서 읽기 작업을 분산시킬 수도 있음. 읽기 성능을 최적화 하는데 유리함.
3. **MongoDB에서 레플리카 세트를 사용하는 경우, Primary 서버에 장애가 발생했을 때 어떤 과정으로 장애를 복구하는지 서술하세요.**
   Primary 서버가 다운되면, 레플리카 세트는 자동으로 하나의 Secondary 서버를 새로운 Primary 서버로 승격시킴. 클라이언트는 새롭게 승격된 Primary 서버로 자동으로 전환됨.
4. **MongoDB에서 Secondary 서버를 활용한 읽기 작업 분산이 무엇인지 설명하고, 이 방법이 읽기 성능에 어떤 영향을 미치는지 논하세요.**
   Secondary 서버를 읽기 작업 분산에 사용을 하면, 읽기 작업에서 보다 고가용성을 가져올 수 있음.
5. **레플리카 세트와 샤딩의 차이점을 설명하고, 두 기능을 함께 사용할 경우의 장점에 대해 서술하세요.**
   레플리카 세트는 고가용성 및 내결함성을 목적으로 읽기 성능이 향상되며, 샤딩은 수평적 확장을 목적으로 성능이 향상된다.
   두 기능을 함께 사용하면 높은 가용성과 성능을 최적화 시킬수 있다.
