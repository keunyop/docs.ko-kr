---
title: '방법: 양식 수준에서 키보드 입력 처리'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- keyboard input [Windows Forms], at form level
- Windows Forms, handling keyboard input
- keyboards [Windows Forms], form-level input
ms.assetid: d7f8b390-dc91-42d2-ae0f-2ffa388127ad
ms.openlocfilehash: fbb6587dde53592a94887c1ea19562e06c15afe3
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61803280"
---
# <a name="how-to-handle-keyboard-input-at-the-form-level"></a>방법: 양식 수준에서 키보드 입력 처리
Windows Forms에서는 메시지가 컨트롤에 도달하기 전에 폼 수준에서 키보드 메시지를 처리하는 기능을 제공합니다. 이 항목에서는 다음 작업을 수행하는 방법에 대해 설명합니다.  
  
### <a name="to-handle-a-keyboard-message-at-the-form-level"></a>폼 수준에서 키보드 메시지를 처리하려면 다음을 수행합니다.  
  
-   시작 폼의 <xref:System.Windows.Forms.Control.KeyPress> 또는 <xref:System.Windows.Forms.Control.KeyDown> 이벤트를 처리하고 키보드 메시지가 폼의 컨트롤에 도달하기 전에 폼이 이 메시지를 수신하도록 폼의 <xref:System.Windows.Forms.Form.KeyPreview%2A> 속성을 `true`로 설정합니다. 다음 코드 예제에서는 모든 숫자 키를 감지하고 '1', '4', '7'을 사용하여 the <xref:System.Windows.Forms.Control.KeyPress> 이벤트를 처리합니다.  
  
     [!code-cpp[System.Windows.Forms.KeyboardInputForm#10](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/cpp/form1.cpp#10)]
     [!code-csharp[System.Windows.Forms.KeyboardInputForm#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/CS/form1.cs#10)]
     [!code-vb[System.Windows.Forms.KeyboardInputForm#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/VB/form1.vb#10)]  
  
## <a name="example"></a>예제  
 다음 코드 예제는 위 예제에 대한 전체 애플리케이션입니다. 응용 프로그램에는 <xref:System.Windows.Forms.TextBox>에서 포커스를 이동할 수 있도록 하는 여러 가지 다른 컨트롤과 함께 <xref:System.Windows.Forms.TextBox>가 포함됩니다. 기본 <xref:System.Windows.Forms.Form>의 <xref:System.Windows.Forms.Control.KeyPress> 이벤트는 '1', '4', '7'을 사용하고 <xref:System.Windows.Forms.TextBox>의 <xref:System.Windows.Forms.Control.KeyPress> 이벤트는 '2', '5', '8'을 사용하면서 나머지 키를 표시합니다. 포커스가 <xref:System.Windows.Forms.TextBox>에 있을 때 <xref:System.Windows.Forms.MessageBox> 출력과 포커스가 다른 컨트롤의 하나에 있는 동안 숫자 키를 누를 때 <xref:System.Windows.Forms.MessageBox> 출력을 비교합니다.  
  
 [!code-cpp[System.Windows.Forms.KeyBoardInputForm#0](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/cpp/form1.cpp#0)]
 [!code-csharp[System.Windows.Forms.KeyBoardInputForm#0](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/CS/form1.cs#0)]
 [!code-vb[System.Windows.Forms.KeyBoardInputForm#0](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.KeyboardInputForm/VB/form1.vb#0)]  
  
## <a name="compiling-the-code"></a>코드 컴파일  
 이 예제에는 다음 사항이 필요합니다.  
  
-   System, System.Drawing 및 System.Windows.Forms 어셈블리에 대한 참조  
  
 Visual Basic 또는 Visual C#에 대 한 명령줄에서이 예제를 빌드하는 방법에 대 한 내용은 [명령줄에서 빌드](../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md) 하거나 [csc.exe를 사용한 명령줄 빌드](../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)합니다. 또한 새 프로젝트에 코드를 붙여 넣어 Visual Studio에서이 예제를 빌드할 수 있습니다.  

## <a name="see-also"></a>참고자료

- [Windows Forms 응용 프로그램의 키보드 입력](keyboard-input-in-a-windows-forms-application.md)
