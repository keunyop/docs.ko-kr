---
title: '방법: PNG 이미지 인코딩 및 디코딩'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- PNG encoding [WPF]
- decoding PNG images [WPF]
- PNG decoding [WPF]
- encoding image formats [WPF]
- decoding image formats [WPF]
- encoding PNG images [WPF]
ms.assetid: 3d31d186-af73-47f0-b5a7-c26ae46409a6
ms.openlocfilehash: 46d4a7ffbfe7a6a620c26447cce30f3a0bd35adc
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59090855"
---
# <a name="how-to-encode-and-decode-a-png-image"></a>방법: PNG 이미지 인코딩 및 디코딩
다음 예제를 디코딩 및 인코딩하는 방법을 보여는 [!INCLUDE[TLA#tla_png](../../../../includes/tlasharptla-png-md.md)] 특정을 사용 하 여 이미지 <xref:System.Windows.Media.Imaging.PngBitmapDecoder> 고 <xref:System.Windows.Media.Imaging.PngBitmapEncoder> 개체입니다.  
  
## <a name="example"></a>예제  
 디코딩하는 방법을 보여 주는이 예제는 [!INCLUDE[TLA2#tla_png](../../../../includes/tla2sharptla-png-md.md)] 를 사용 하 여 이미지를 <xref:System.Windows.Media.Imaging.PngBitmapDecoder> 에서 <xref:System.IO.FileStream>합니다.  
  
 [!code-cpp[PngBitmapDecoderEncoder#1](~/samples/snippets/cpp/VS_Snippets_Wpf/PngBitmapDecoderEncoder/CPP/PngEncoderDecoder.cpp#1)]
 [!code-csharp[PngBitmapDecoderEncoder#1](~/samples/snippets/csharp/VS_Snippets_Wpf/PngBitmapDecoderEncoder/CSharp/PngEncoderDecoder.cs#1)]
 [!code-vb[PngBitmapDecoderEncoder#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PngBitmapDecoderEncoder/VB/PngEncoderDecoder.vb#1)]  
  
## <a name="example"></a>예제  
 인코딩하는 방법을 보여 주는이 예제는 <xref:System.Windows.Media.Imaging.BitmapSource> 에 [!INCLUDE[TLA2#tla_png](../../../../includes/tla2sharptla-png-md.md)] 사용 하 여 이미지를 <xref:System.Windows.Media.Imaging.PngBitmapEncoder>입니다.  
  
 [!code-cpp[PngBitmapDecoderEncoder#4](~/samples/snippets/cpp/VS_Snippets_Wpf/PngBitmapDecoderEncoder/CPP/PngEncoderDecoder.cpp#4)]
 [!code-csharp[PngBitmapDecoderEncoder#4](~/samples/snippets/csharp/VS_Snippets_Wpf/PngBitmapDecoderEncoder/CSharp/PngEncoderDecoder.cs#4)]
 [!code-vb[PngBitmapDecoderEncoder#4](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PngBitmapDecoderEncoder/VB/PngEncoderDecoder.vb#4)]  
  
## <a name="see-also"></a>참고자료

- [이미징 개요](imaging-overview.md)
