---
title: ICorDebugManagedCallback 인터페이스
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback
helpviewer_keywords:
- ICorDebugManagedCallback interface [.NET Framework debugging]
ms.assetid: b47f1d61-c7dc-4196-b926-0b08c94f7041
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: eacd10eecf2a8a2fc1b73a7f97eef5cb5eabafd4
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59133720"
---
# <a name="icordebugmanagedcallback-interface"></a>ICorDebugManagedCallback 인터페이스
디버거 콜백을 처리하는 메서드를 제공합니다.  
  
## <a name="methods"></a>메서드  
  
|메서드|설명|  
|------------|-----------------|  
|[Break 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-break-method.md)|디버거에 알립니다 때를 <xref:System.Reflection.Emit.OpCodes.Break> 코드 스트림의 명령이 실행 됩니다.|  
|[Breakpoint 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-breakpoint-method.md)|중단점에 도달할 때 디버거를에 알립니다.|  
|[BreakpointSetError 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-breakpointseterror-method.md)|CLR (공용 언어 런타임) 함수를 just-in-time (JIT) 컴파일 전에 설정 된 중단점을 정확 하 게 바인딩할 수 있음을 디버거에 알립니다.|  
|[ControlCTrap 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-controlctrap-method.md)|디버깅 중인 프로세스에서 CTRL + C가 트래핑는 디버거에 알립니다.|  
|[CreateAppDomain 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-createappdomain-method.md)|응용 프로그램 도메인이 만들어졌는지 디버거에 알립니다.|  
|[CreateProcess 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-createprocess-method.md)|프로세스에 연결 되거나 처음으로 시작 하는 경우 디버거를에 알립니다.|  
|[CreateThread 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-createthread-method.md)|스레드가 관리 코드 실행 시작 되도록 디버거에 알립니다.|  
|[DebuggerError 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-debuggererror-method.md)|CLR에서 이벤트를 처리 하는 동안 오류가 발생 했습니다는 디버거에 알립니다.|  
|[EditAndContinueRemap 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-editandcontinueremap-method.md)|더 이상 사용되지 않습니다. IDE에 매핑 변경 이벤트를 보냈음을 디버거에 알립니다.|  
|[EvalComplete 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-evalcomplete-method.md)|평가 완료 되었음을 나타내는 디버거에 알립니다.|  
|[EvalException 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-evalexception-method.md)|처리 되지 않은 예외를 사용 하 여 평가 종료 되었다는 디버거에 알립니다.|  
|[Exception 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-exception-method.md)|관리 코드에서 예외가 throw 되었음을 디버거에 알립니다.|  
|[ExitAppDomain 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-exitappdomain-method.md)|응용 프로그램 도메인 종료 되었습니다. 디버거에 알립니다.|  
|[ExitProcess 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-exitprocess-method.md)|프로세스가 종료 되었는지 디버거에 알립니다.|  
|[ExitThread 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-exitthread-method.md)|관리 되는 코드를 실행 하는 스레드가 종료 된 디버거에 알립니다.|  
|[LoadAssembly 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-loadassembly-method.md)|CLR 어셈블리를 성공적으로 로드 되었음을 디버거에 알립니다.|  
|[LoadClass 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-loadclass-method.md)|로드 된 클래스는 디버거에 알립니다.|  
|[LoadModule 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-loadmodule-method.md)|CLR 모듈을 성공적으로 로드 되었음을 디버거에 알립니다.|  
|[LogMessage 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-logmessage-method.md)|CLR의 관리 되는 스레드에서의 메서드를 호출에 디버거에 알립니다는 <xref:System.Diagnostics.EventLog> 클래스는 이벤트를 기록 합니다.|  
|[LogSwitch 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-logswitch-method.md)|관리 되는 CLR 스레드에서의 메서드를 호출에 디버거에 알립니다는 <xref:System.Diagnostics.Switch> 클래스를 만들기, 수정 또는 디버깅/추적 스위치를 삭제 합니다.|  
|[NameChange 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-namechange-method.md)|응용 프로그램 도메인 또는 스레드의 이름이 변경 되었습니다 디버거에 알립니다.|  
|[StepComplete 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-stepcomplete-method.md)|단계가 완료 되었음을 디버거에 알립니다.|  
|[UnloadAssembly 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-unloadassembly-method.md)|CLR 어셈블리 로드 되었음을 디버거에 알립니다.|  
|[UnloadClass 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-unloadclass-method.md)|클래스를 언로드되고 있음을 디버거에 알립니다.|  
|[UnloadModule 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-unloadmodule-method.md)|CLR 모듈 (DLL)이 로드 되었음을 디버거에 알립니다.|  
|[UpdateModuleSymbols 메서드](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-updatemodulesymbols-method.md)|CLR 모듈에 대 한 기호 변경 된 디버거에 알립니다.|  
  
## <a name="remarks"></a>설명  
 모든 콜백 직렬화 되, 동일한 스레드에서 호출 되 고 동기화 된 상태에서 프로세스를 사용 하 여 호출 합니다.  
  
 각 콜백 구현을 호출 해야 합니다 [icordebugcontroller:: Continue](../../../../docs/framework/unmanaged-api/debugging/icordebugcontroller-continue-method.md) 실행을 다시 시작 합니다. 하는 경우 `ICorDebugController::Continue` 콜백이 반환, 프로세스를 계속 중지 및 없습니다 자세한 이벤트 콜백을까지 발생 전에 호출 되지 않습니다 `ICorDebugController::Continue` 라고 합니다.  
  
 디버거를 구현 해야 합니다 [ICorDebugManagedCallback2](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback2-interface.md) .NET Framework 버전 2.0 응용 프로그램을 디버깅 하는 경우. 인스턴스의 `ICorDebugManagedCallback` 나 `ICorDebugManagedCallback2` 콜백 개체로 전달 됩니다 [icordebug:: Setmanagedhandler](../../../../docs/framework/unmanaged-api/debugging/icordebug-setmanagedhandler-method.md)합니다.  
  
> [!NOTE]
>  이 인터페이스는 크로스 시스템 또는 크로스 프로세스 원격 호출을 지원하지 않습니다.  
  
## <a name="requirements"></a>요구 사항  
 **플랫폼:** [시스템 요구 사항](../../../../docs/framework/get-started/system-requirements.md)을 참조하십시오.  
  
 **헤더:** CorDebug.idl, CorDebug.h  
  
 **라이브러리:** CorGuids.lib  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>참고자료

- [ICorDebug 인터페이스](../../../../docs/framework/unmanaged-api/debugging/icordebug-interface.md)
- [ICorDebugManagedCallback2 인터페이스](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback2-interface.md)
- [디버깅 인터페이스](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
