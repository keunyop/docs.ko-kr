---
title: 호스팅 예외
ms.date: 03/30/2017
ms.assetid: ad9e14f8-fa17-4d59-b365-fe0e7ec17c98
ms.openlocfilehash: f2bc7d0ca09faa2598990437295d1abf0cff3c45
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33469439"
---
# <a name="hosting-exceptions"></a>호스팅 예외
이 항목에서는 호스팅에 의해 생성된 모든 예외를 보여 줍니다.  
  
## <a name="exception-list"></a>예외 목록  
  
|리소스 코드|리소스 문자열|  
|-------------------|---------------------|  
|Hosting_AddressIsAbsoluteUri|전체 URI가 허용되지 않습니다. ServiceHostingEnvironment.EnsureServiceAvailable API에는 전체 URI가 허용되지 않습니다. 해당 서비스의 가상 경로를 사용하십시오.|  
|Hosting_BuildProviderCouldNotCreateType|서비스를 컴파일하는 동안 지정된 CLR 형식을 로드할 수 없습니다. 이 형식이 응용 프로그램에 있는 소스 파일에 하거나 정의 된 확인 \\\App_Code 디렉터리에 있는 응용 프로그램의 컴파일된 어셈블리에 포함 된 \\\bin 디렉터리나에 설치 된 어셈블리에는 전역 어셈블리 캐시 합니다. 형식 이름은 대/소문자를 구분합니다. 와 같은 디렉터리 \\\App_Code 및 \\\bin 응용 프로그램의 루트 디렉터리에 있어야 합니다. \\\App_Code 및 \\\bin 디렉터리에 하위 디렉터리에 중첩 될 수 없습니다.|
