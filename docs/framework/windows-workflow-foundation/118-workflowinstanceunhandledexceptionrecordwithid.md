---
title: 118 - WorkflowInstanceUnhandledExceptionRecordWithId
ms.date: 03/30/2017
ms.assetid: 2ce4b193-e141-4cc4-86a3-2e8c984c110d
ms.openlocfilehash: eb69fc4760cd89294e24680b5aab83fcd058feb0
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62009879"
---
# <a name="118---workflowinstanceunhandledexceptionrecordwithid"></a>118 - WorkflowInstanceUnhandledExceptionRecordWithId
## <a name="properties"></a>속성  
  
|||  
|-|-|  
|ID|118|  
|키워드|HealthMonitoring, WFTracking|  
|수준|Error|  
|채널|Microsoft-Windows-응용 프로그램 서버-응용 프로그램/분석|  
  
## <a name="description"></a>설명  
 워크플로 인스턴스가 WorkflowInstanceUnhandledExceptionRecord를 내보내면 ETW 추적 참가자가 이 이벤트를 내보냅니다.  
  
## <a name="message"></a>메시지  
 TrackRecord = WorkflowInstanceUnhandledExceptionRecord, InstanceID = %1, RecordNumber = %2, EventTime = %3, ActivityDefinitionId = %4, SourceName = %5, SourceId = %6, SourceInstanceId = %7, 소스 형식 이름 = %8, 예외 = %9, 주석, %10, ProfileName = = %11, WorkflowDefinitionIdentity = %12  
  
## <a name="details"></a>설명  
  
|데이터 항목 이름|데이터 항목 형식|설명|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|워크플로의 인스턴스 ID|  
|RecordNumber|xs:long|내보낸 레코드의 시퀀스 번호|  
|EventTime|xs:dateTime|이벤트를 내보낸 시간(UTC)|  
|ActivityDefinitionId|xs:string|워크플로의 루트 활동 이름|  
|SourceName|xs:string|오류로 인해 unhandledException을 일으킨 소스 활동의 이름|  
|SourceId|xs:string|오류 소스 활동의 활동 ID|  
|SourceInstanceId|xs:string|오류 소스 활동의 활동 인스턴스 ID|  
|SourceTypeName|xs:string|오류로 인해 unhandledException을 일으킨 소스 활동의 형식 이름|  
|예외|xs:string|처리되지 않은 예외에 대한 예외 정보|  
|상태|xs:string|워크플로의 현재 상태|  
|주석|xs:string|이 이벤트에 추가된 주석입니다. 값 형식으로 xml 요소에 저장 됩니다 \<항목 >\< 항목 이름 = "annotationName" type = "> annotationValue\</> \< />입니다. 주석을 지정 하지 않으면 문자열을 포함 하는 경우 \<항목 / >입니다. ETW 이벤트 크기는 ETW 버퍼 크기 또는 ETW 이벤트의 최대 페이로드에 따라 제한됩니다. 이벤트 크기가 ETW 제한을 초과 하면 주석을 삭제 하 고 주석 값을 대체 하 여 이벤트 잘립니다 경우 \<항목 >...  \< />입니다.|  
|ProfileName|xs:string|이 이벤트를 내보낸 이름 또는 추적 프로필|  
|WorkflowDefinitionIdentity|xs:string|워크플로 정의 ID입니다.|  
|AppDomain|xs:string|AppDomain.CurrentDomain.FriendlyName에서 반환되는 문자열입니다.|
