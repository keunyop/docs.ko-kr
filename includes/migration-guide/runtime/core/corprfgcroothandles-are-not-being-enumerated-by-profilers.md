---
ms.openlocfilehash: 8433899058c6f569e380999800557dbe8ed0a169
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59235453"
---
### <a name="corprfgcroothandles-are-not-being-enumerated-by-profilers"></a>프로파일러에서 COR_PRF_GC_ROOT_HANDLE을 열거하지 않습니다.

|   |   |
|---|---|
|세부 정보|.NET Framework v4.5.1에서 프로파일링 API <code>RootReferences2()</code>가 <code>COR_PRF_GC_ROOT_HANDLE</code>를 절대 반환하지 않습니다(대신 <code>COR_PRF_GC_ROOT_OTHER</code>로 반환). 이 문제는 .NET Framework 4.6부터 해결되었습니다.|
|제안 해결 방법|이 문제는 .NET Framework 4.6에서 해결되었으며, 해당 버전의 .NET Framework로 업그레이드하여 해결할 수 있습니다.|
|범위|부|
|버전|4.5.1|
|형식|런타임|
