---
title: ICorDebugHeapEnum::Next 메서드
ms.date: 03/30/2017
api_name:
- ICorDebugHeapEnum.Next
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapEnum::Next
helpviewer_keywords:
- ICorDebugHeapEnum::Next method [.NET Framework debugging]
- Next method, ICorDebugHeapEnum interface [.NET Framework debugging]
ms.assetid: 2221fd06-9e27-4113-972e-2530db8c3594
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 4bd993eb26f26818117a20d376c3331f88c46b26
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59214353"
---
# <a name="icordebugheapenumnext-method"></a>ICorDebugHeapEnum::Next 메서드
지정 된 개수를 가져옵니다 [COR_HEAPOBJECT](../../../../docs/framework/unmanaged-api/debugging/cor-heapobject-structure.md) 관리 되는 힙의 개체에 대 한 정보가 포함 된 인스턴스.  
  
## <a name="syntax"></a>구문  
  
```  
HRESULT Next(  
    [in] ULONG celt,    [out, size_is(celt), length_is(*pceltFetched)] COR_HEAPOBJECT  objects[],   
    [out] ULONG *pceltFetched  
);  
```  
  
## <a name="parameters"></a>매개 변수  
 celt  
 [in] 검색할 개체 수입니다.  
  
 개체  
 [out] 각각 가리키는 포인터 배열을 [COR_HEAPOBJECT](../../../../docs/framework/unmanaged-api/debugging/cor-heapobject-structure.md) 관리 되는 힙의 개체에 대 한 정보를 제공 하는 개체입니다.  
  
 pceltFetched  
 [out] 개수에 대 한 포인터 [COR_HEAPOBJECT](../../../../docs/framework/unmanaged-api/debugging/cor-heapobject-structure.md) 개체에 실제로 반환 된 `objects`합니다. `celt`가 1이면 이 값은 `null`일 수 있습니다.  
  
## <a name="remarks"></a>설명  
 `COR_HEAPOBJECT.type` 필드는 중첩된 참조 수가 계산되는 COM 인터페이스의 식별자입니다. 이 참조는 `ICorDebugHeapEnum::Next` 호출자가 해제해야 합니다.  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하십시오.  
  
 **헤더:** CorDebug.idl, CorDebug.h  
  
 **라이브러리:** CorGuids.lib  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>참고자료

- [ICorDebugHeapEnum 인터페이스](../../../../docs/framework/unmanaged-api/debugging/icordebugheapenum-interface.md)
- [디버깅 인터페이스](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
