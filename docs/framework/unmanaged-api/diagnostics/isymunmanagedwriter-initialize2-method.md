---
title: ISymUnmanagedWriter::Initialize2 메서드
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.Initialize2
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::Initialize2
helpviewer_keywords:
- ISymUnmanagedWriter::Initialize2 method [.NET Framework debugging]
- Initialize2 method [.NET Framework debugging]
ms.assetid: 93de56b6-4ae8-4cca-acdc-25a434623509
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: e645f79018d4ad41451faa07eba860e68b917539
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59187020"
---
# <a name="isymunmanagedwriterinitialize2-method"></a>ISymUnmanagedWriter::Initialize2 메서드
이 작성기가 연결 될 하는 메타 데이터 내보내기 인터페이스를 설정 하 고 디버깅 기호를 쓸 출력 파일 이름을 설정 합니다. 또한이 메서드는 프로그램 데이터베이스 (PDB) 파일의 최종 위치를 설정할 수 있습니다.  
  
## <a name="syntax"></a>구문  
  
```  
HRESULT Initialize2(  
    [in] IUnknown     *emitter,  
    [in] const WCHAR  *tempfilename,  
    [in] IStream      *pIStream,  
    [in] BOOL         fFullBuild,  
    [in] const WCHAR  *finalfilename);  
```  
  
## <a name="parameters"></a>매개 변수  
 `emitter`  
 [in] 메타 데이터 내보내기 인터페이스 포인터입니다.  
  
 `tempfilename`  
 [in] 에 대 한 포인터를 `WCHAR` 는 디버깅 기호가 쓰여진 파일 이름이 들어 있는입니다. 파일 이름을 사용하지 않는 작성기에 대해 파일 이름이 지정되면 이 매개 변수는 무시됩니다.  
  
 `pIStream`  
 [in] 기호 작성기에 대 한 기호를 내보내는 지정 하는 경우를 지정 <xref:System.Runtime.InteropServices.ComTypes.IStream> 에 지정 된 파일 대신는 `filename` 매개 변수입니다. `pIStream` 매개 변수는 선택적 요소입니다.  
  
 `fFullBuild`  
 [in] `true` 전체를 다시 빌드해야 합니다;이 경우 `false` 는 증분 컴파일을 경우.  
  
 `finalfilename`  
 [in] 에 대 한 포인터를 `WCHAR` PDB 파일의 최종 위치에 경로 문자열입니다.  
  
## <a name="return-value"></a>반환 값  
 메서드가 성공 하면 s_ok이 고 그렇지 않으면 E_FAIL 또는 일부 다른 오류 코드입니다.  
  
## <a name="requirements"></a>요구 사항  
 **헤더:** CorSym.idl, CorSym.h  
  
## <a name="see-also"></a>참고자료

- [ISymUnmanagedWriter 인터페이스](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-interface.md)
- [Initialize 메서드](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-initialize-method.md)
