---
title: COM+ 및 ServiceModel의 트랜잭션 비교
ms.date: 03/30/2017
ms.assetid: e493bcdd-b91a-4486-853f-83dbcd1931b7
ms.openlocfilehash: 4a47fe1686dff2e705b06b001d7d5e4ea6e8c5f2
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33488649"
---
# <a name="comparing-transactions-in-com-and-servicemodel"></a>COM+ 및 ServiceModel의 트랜잭션 비교
이 항목에서는 트랜잭션 COM + 서비스의 Windows Communication Foundation (WCF) 특성을 사용 하 여 동작을 시뮬레이션 하는 방법을 설명의 <xref:System.ServiceModel> 네임 스페이스를 제공 합니다.  
  
## <a name="emulating-com-using-servicemodel-attributes"></a>ServiceModel 특성을 사용하여 COM+ 에뮬레이트  
 다음 표에서 비교는 <xref:System.EnterpriseServices.TransactionOption> 를 만드는 데 사용 하는 열거는 `EnterpriseServices` 트랜잭션과 상호 관련 된 방식을 WCF 특성은 <xref:System.ServiceModel> 네임 스페이스를 제공 합니다.  
  
|COM+ 특성|WCF 특성|  
|---------------------|------------------------------------------------------------------------|  
|RequiresNew|<xref:System.ServiceModel.TransactionFlowAttribute>이 <xref:System.ServiceModel.TransactionFlowOption.NotAllowed>로 설정됩니다.<br /><br /> <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A>가 `true`인 경우<br /><br /> 바인딩 요소의 `TransactionFlow` 특성이 `false`인 경우|  
|필수|<xref:System.ServiceModel.TransactionFlowAttribute>이 <xref:System.ServiceModel.TransactionFlowOption.Allowed>로 설정됩니다.<br /><br /> <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A>가 `true`인 경우<br /><br /> 바인딩 요소의 `TransactionFlow` 특성이 `true`인 경우|  
|지원함|직접 대응하는 특성이 없습니다. 일반적으로 `Required`에 지정된 동작을 대신 사용해야 합니다.|  
|NotSupported|<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A>가 `false`인 경우<br /><br /> 바인딩 요소의 `TransactionFlow` 특성이 `false`인 경우|  
|Disabled|직접 대응하는 특성이 없습니다. 일반적으로 `NotSupported`에 지정된 동작을 대신 사용해야 합니다.|
