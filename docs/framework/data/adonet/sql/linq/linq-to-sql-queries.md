---
title: LINQ to SQL 쿼리
ms.date: 03/30/2017
ms.assetid: f4897aaa-7f44-4c20-a471-b948c2971aae
ms.openlocfilehash: 49106502dc58eef36ea0c910c627c9cf397f419e
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61902878"
---
# <a name="linq-to-sql-queries"></a>LINQ to SQL 쿼리
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]에서 동일한 구문을 사용하여  [!INCLUDE[vbteclinq](../../../../../../includes/vbteclinq-md.md)] 쿼리를 정의합니다. 유일한 차이점은 쿼리에서 참조된 개체가 데이터베이스의 요소에 매핑된다는 것입니다. 자세한 내용은 [LINQ 쿼리 소개(C#)](~/docs/csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)를 참조하세요.  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]에서는 작성한 쿼리를 해당 SQL 쿼리로 변환하고 SQL Server에 전달하여 처리합니다. 다시 말해서 응용 프로그램에서는 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] API를 사용하여 쿼리 실행을 요청합니다. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 공급자는 쿼리를 SQL 텍스트로 변환하고 ADO 공급자에게 실행을 위임합니다. ADO 공급자는 `DataReader`의 결과로 쿼리를 반환합니다. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 공급자는 ADO 결과를 사용자 개체의 <xref:System.Linq.IQueryable> 컬렉션으로 변환합니다.  
  
> [!NOTE]
>  [!INCLUDE[dnprdnshort](../../../../../../includes/dnprdnshort-md.md)] 기본 제공 형식에서 대부분의 메서드와 연산자는 SQL로 직접 변환됩니다. [!INCLUDE[vbteclinq](../../../../../../includes/vbteclinq-md.md)]에서 변환할 수 없는 일부는 런타임 예외를 발생시킵니다. 자세한 내용은 [SQL-CLR 형식 매핑](../../../../../../docs/framework/data/adonet/sql/linq/sql-clr-type-mapping.md)합니다.  
  
 다음 테이블에서는 [!INCLUDE[vbteclinq](../../../../../../includes/vbteclinq-md.md)]와 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 쿼리 항목의 유사점과 차이점에 대해 설명합니다.  
  
|항목|LINQ 쿼리|[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] Query|  
|----------|----------------|----------------------------------------------------------------------|  
|시퀀스를 반환하는 쿼리에 대한 쿼리를 보관하는 지역 변수의 반환 형식|제네릭 `IEnumerable`|제네릭 `IQueryable`|  
|데이터 소스 지정|사용 하 여 `From` (Visual Basic) 또는 `from` (C#) 절|왼쪽과 같음|  
|필터링|사용 하 여 `Where` / `where` 절|왼쪽과 같음|  
|그룹화|사용 하 여 `Group…By` / `groupby` 절|왼쪽과 같음|  
|선택(프로젝팅)|사용 하 여 `Select` / `select` 절|왼쪽과 같음|  
|지연된 실행과 즉시 실행 비교|참조 [LINQ 쿼리 소개 (C#)](~/docs/csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)|왼쪽과 같음|  
|조인 구현|사용 하 여 `Join` / `join` 절|사용할 수는 `Join` / `join` 절을 사용 하지만 보다 효과적으로 <xref:System.Data.Linq.Mapping.AssociationAttribute> 특성입니다. 자세한 내용은 [관계 간 쿼리](../../../../../../docs/framework/data/adonet/sql/linq/querying-across-relationships.md)합니다.|  
|원격 실행과 로컬 실행 비교||자세한 내용은 참조 하세요. [원격 vs. 로컬 실행](../../../../../../docs/framework/data/adonet/sql/linq/remote-vs-local-execution.md)합니다.|  
|스트리밍 쿼리와 캐시된 쿼리 비교|지역 메모리 시나리오에서는 사용할 수 없습니다.||  
  
## <a name="see-also"></a>참고자료

- [LINQ 쿼리 소개(C#)](~/docs/csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)
- [기본 LINQ 쿼리 작업](~/docs/csharp/programming-guide/concepts/linq/basic-linq-query-operations.md)
- [LINQ 쿼리 작업의 형식 관계](~/docs/csharp/programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)
- [쿼리 개념](../../../../../../docs/framework/data/adonet/sql/linq/query-concepts.md)
