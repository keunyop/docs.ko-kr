---
title: System.ServiceModel.Channels.MsmqMessageDropped
ms.date: 03/30/2017
ms.assetid: 8b6e644d-fa68-4be7-abe9-3659671a37c1
ms.openlocfilehash: 3fa5ec62c5e8ac83f3f81fb406499b7e596b3dac
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59161339"
---
# <a name="systemservicemodelchannelsmsmqmessagedropped"></a>System.ServiceModel.Channels.MsmqMessageDropped
MSMQ에서 메시지를 삭제했습니다.  
  
## <a name="description"></a>설명  
 추적은 MSMQ 메시지가 삭제되었음을 나타냅니다. Windows Communication Foundation (WCF) (NetMsmqBinding 또는 MsmqIntegrationBinding을 사용 하 여 사용)를 처리할 수 없는 경우 MSMQ 메시지를 삭제할 수 있습니다. 이러한 메시지를 포이즌 메시지라고 합니다.  
  
 포이즌 메시지는 NetMsmqBinding 또는 MsmqIntegrationBinding의 `ReceiveErrorHandling` 속성을 `Drop`으로 설정할 경우 삭제됩니다. 삭제된 메시지는 큐에서 제거되고 더 이상 복구될 수 없습니다.  
  
 참조 [포이즌 메시지 처리](https://go.microsoft.com/fwlink/?LinkID=99546) 메시지는 포이즌 메시지가 될 경우 및이 적절히 처리 하도록 서비스를 구성 하는 방법에 대 한 자세한 내용은 합니다.  
  
## <a name="see-also"></a>참고자료

- [추적](../../../../../docs/framework/wcf/diagnostics/tracing/index.md)
- [추적을 사용하여 애플리케이션 문제 해결](../../../../../docs/framework/wcf/diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)
- [관리 및 진단](../../../../../docs/framework/wcf/diagnostics/index.md)
- [포이즌 메시지 처리](https://go.microsoft.com/fwlink/?LinkID=99546)
