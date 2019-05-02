---
title: ML.NET의 모델 학습에 대한 기본 개념
description: 이 문서에서는 ML.NET의 *모델 학습* 시나리오에 대한 기본 개념을 설명합니다. 일부 개념은 보다 간단한 *기존 모델을 사용한 예측* 시나리오와 관련이 없습니다.
ms.date: 03/05/2019
ms.topic: overview
ms.custom: mvc
ms.openlocfilehash: 78554cfd1dfdb3dba82a325d47245aeb313bb6fe
ms.sourcegitcommit: 58fc0e6564a37fa1b9b1b140a637e864c4cf696e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57675565"
---
# <a name="basic-concepts-for-model-training-in-mlnet"></a>ML.NET의 모델 학습에 대한 기본 개념

이 문서에서는 ML.NET의 *모델 학습* 시나리오에 대한 기본 개념을 설명합니다. 일부 개념은 보다 간단한 *기존 모델을 사용한 예측* 시나리오와 관련이 있습니다.

이 문서와 관련 샘플에서는 현재 **ML.NET 버전 0.10**을 사용하고 있습니다. 자세한 내용은 [dotnet/machinelearning GitHub 리포지토리](https://github.com/dotnet/machinelearning/tree/master/docs/release-notes)에서 릴리스 정보를 참조하세요.

## <a name="model-training-concepts"></a>모델 학습 개념

이 문서에서는 ML.NET의 모델 학습과 관련된 다음 개념을 다룹니다.

- [데이터(*Data*)](#data)는 `IDataView` 인터페이스로 표시됩니다.
  - ML.NET에서 데이터는 SQL 보기와 유사합니다. 지연 평가되고, 변경할 수 없고, 커서가 가능하고, 형식이 다르며, 스키마화된 데이터 세트입니다. 
  - 데이터 인터페이스에 대한 훌륭한 문서는 [IDataView 디자인 원칙](https://github.com/dotnet/machinelearning/blob/master/docs/code/IDataViewDesignPrinciples.md)입니다.
- [변환기(*Transformer*)](#transformer)는 `ITransformer` 인터페이스로 표시됩니다.
  - 변환기는 데이터를 가져다가, 데이터에 작업을 수행하고, 변환된 새로운 데이터를 반환하는 구성 요소입니다.
  - 예를 들어 기계 학습 모델은 기능을 사용하고 예측을 반환하는 변환기로 생각할 수 있습니다.
  - 다른 예로, 텍스트 토크나이저는 단일 텍스트 열을 가져와서 텍스트에서 개별 단어를 추출하여 벡터 열을 출력합니다.
- [데이터 판독기(*Data reader*)](#data-reader)는 `IDataReader<T>` 인터페이스로 표시됩니다.
  - 데이터 판독기는 데이터를 만드는 ML.NET 구성 요소입니다. `T` 인스턴스를 가져다가 데이터를 반환합니다. 
  - 예를 들어, *TextLoader*는 `IDataReader<IMultiStreamSource>`입니다. 소스를 사용하여 데이터를 생성합니다. 
- [평가자(*Estimator*)](#estimator)는 `IEstimator<T>` 인터페이스로 표시됩니다.
  - 데이터를 통해 학습하는 개체입니다. 학습의 결과는 변환기(*transformer*)입니다.
  - 기계 학습 알고리즘(*algorithm*)은 데이터를 학습하고 기계 학습 모델(*model*)을 생성하는 평가자라고 생각할 수 있습니다.
- [예측 함수(*Prediction function*)](#prediction-function)는 `PredictionEngine<TSrc, TDst>` 클래스로 표시됩니다.
  - 예측 함수는 예측 시간과 같은 변환기를 한 행에 적용하는 머신으로 볼 수 있습니다.

## <a name="data"></a>데이터

ML.NET에서 데이터는 SQL 보기와 유사합니다. 지연 평가되고, 커서가 가능하고, 형식이 다르며, 스키마화된 데이터 세트입니다.

- 데이터 뷰의 열에 대한 정보가 포함된 스키마(*Schema*)(`ISchema` 인터페이스의 인스턴스)가 있습니다.
  - 각 열에는 이름(*Name*), 유형(*Type*) 및 이와 관련된 임의의 메타데이터(*metadata*) 세트가 있습니다.
  - 열의 메타데이터(*metadata*)에는 벡터 열의 슬롯 이름 등과 같은 정보가 있습니다. 메타데이터 자체는 각 열에 고유한 또 다른 한 행 데이터(*data*)로 표시됩니다.
  - 데이터 뷰는 커서(*cursors*)의 소스입니다. SQL 커서를 생각해보면: 커서는 한 번에 한 행씩 데이터를 반복하고 사용 가능한 데이터를 표시하는 개체입니다.
  - 물론 데이터에는 필요한 만큼 활성 커서가 많이 있을 수 있습니다. 데이터 자체는 변경할 수 없기 때문에 커서는 완전히 독립적입니다.
  - 커서는 일반적으로 열의 하위 집합에만 액세스합니다. 효율성을 높이기 위해 ML.NET은 커서에 필요한 열의 값만 계산합니다.

## <a name="transformer"></a>변환기

변환기는 데이터를 가져다가, 데이터에 작업을 수행하고, 변환된 새로운 데이터를 반환하는 구성 요소입니다.

ML.NET에서 대부분의 변환기는 한 번에 하나의 입력 열(*input column*)에 작동하고 출력 열(*output column*)을 생성하는 경향이 있습니다. 예를 들어 `new HashTransformer("bar", "foo")`는 "foo" 열의 값을 사용하여, 해시를 지정하고, "bar" 열에 넣습니다. 

또한, 입력 및 출력 열 이름이 동일합니다. 이런 경우 기존 열이 새 열로 바뀝니다. 예를 들어 `new HashTransformer("foo")`는 "foo" 열의 값을 사용하여, 해시를 지정하고, "foo" 열에 다시 넣습니다. 

`Transform`이 호출될 때 모든 변환기는 새로운 데이터 보기를 생성합니다. 데이터 보기는 변경할 수 없다는 것을 기억하십시오.

또 다른 주요 고려 사항은 데이터가 지연 평가되기 때문에 *변환기도 지연*된다는 것입니다. 특히, 호출 후에

```csharp
var newData = transformer.Transform(oldData)
```

실제 계산 수행되지 않습니다. `newData`에서 커서를 가져와서 값을 사용하기 시작한 후에야 `newData`는 `transformer` 변환 논리를 호출합니다(그럼에도 요청된 열을 생성하기 위해 해당 `transformer`가 필요한 경우에만 발생합니다).

### <a name="transformer-chains"></a>변환기 체인

변환기의 유용한 속성은 *변환기의 순차적인 적용을 다른 변환기로 빌드할 수 있다는 점입니다*.

```csharp
var fullTransformer = transformer1.Append(transformer2).Append(transformer3);
```

이 속성은 ML.NET에서 일반적으로 사용됩니다. 일반적으로, 학습된 ML.NET 모델은 모든 의도와 목적을 위한 변환기(*transformer*)의 연결인 변환기 체인입니다.

## <a name="data-reader"></a>데이터 판독기

데이터 판독기는 데이터를 '생성'하는 ML.NET 구성 요소입니다. `T` 인스턴스를 사용하여 데이터를 반환합니다.

흥미로운 점은, 기존 데이터 판독기에 변환기를 연결하여 데이터 판독기를 만들 수 있습니다. 이러한 방식으로 변환 동작이 베이킹된 판독기를 사용 수 있습니다.

```csharp
var newReader = reader.Append(transformer1).Append(transformer2)
```

변환기와 유사한 또 다른 점은 데이터가 지연 평가되기 때문에 판독기가 지연된다는 점입니다. `reader.Read()`를 호출하면 실제로 '읽기'가 발생하지 않습니다(또는 최소로 발생함). 커서가 결과 데이터에 요청된 경우에만 판독기 작업이 시작됩니다.

## <a name="estimator"></a>평가자

평가자(*estimator*)는 데이터를 통해 학습하는 개체입니다. 학습의 결과는 변환기(*transformer*)입니다.

ML.NET에서는 이 속성을 사용하여 다른 평가자를 연결하는 학습 파이프라인을 만듭니다.

```csharp
MLContext mlContext = new MLContext(seed: 0);

var pipeline = mlContext.Transforms.CopyColumns("Label","FareAmount")
    .Append(mlContext.Transforms.Categorical.OneHotEncoding("VendorId"))
    .Append(mlContext.Transforms.Categorical.OneHotEncoding("RateCode"))
    .Append(mlContext.Transforms.Categorical.OneHotEncoding("PaymentType"))
    .Append(mlContext.Transforms.Concatenate("Features","VendorId","RateCode","PassengerCount","TripTime","TripDistance","PaymentType"))
    .Append(mlContext.Regression.Trainers.FastTree("Label","Features"));

var model = pipeline.Fit(dataView);
```

평가자의 중요한 특성 중 하나는 지연되지 않고 즉시 수행된다는 점입니다.`Fit`을 호출할 때마다 학습이 발생하며 이러한 작업은 잠재적으로 시간이 오래 걸리는 작업입니다.

## <a name="prediction-function"></a>예측 함수

예측 함수는 한 행에 변환기를 적용하는 구성 요소로 볼 수 있습니다.

모델(`Fit()`을 통해 학습된 또는 다른 곳에서 로드한 변환기(*transformer*))을 얻은 후에는 `model.Transform(data)`에 대한 일반적인 호출을 사용하여 예측을 수행할 수 있습니다. 하지만 실제 시나리오에 이 모델을 사용하면 예측할 사례가 많지 않습니다. 대신, 한 번에 하나의 예제가 있으며, 적절한 시기에 즉시 예측을 수행해야 합니다.

물론 이러한 작업을 일괄 처리 예측으로 줄일 수 있습니다.

- 정확히 하나의 행으로 데이터 뷰를 만듭니다.
- `model.Transform(data)`을 호출하여 예측된 데이터 뷰를 가져옵니다.
- 결과 데이터에 대해 커서를 가져옵니다.
- 커서를 한 단계 전진하여 첫 번째(유일한) 행을 얻습니다.
- 여기에서 예측된 값을 추출합니다.

앞의 알고리즘은 다음과 같이 두 개의 사용자 정의 개체(`InputExample` 및 `OutputPrediction`)를 사용하여 [스키마 이해를](https://github.com/dotnet/machinelearning/blob/master/docs/code/SchemaComprehension.md) 사용하여 구현할 수 있습니다.

```csharp
var inputData = mlContext.Data.ReadFromEnumerable(new InputExample[] { example });
var outputData = model.Transform(inputData);
var output = mlContext.CreateEnumerable<OutputPrediction>(outputData,reuseRowObject:false).First();
```

하지만 이 작업은 번거롭고 성능 비용이 발생합니다. 대신, 예측 함수 개체는 확장 메서드, `CreatePredictionEngine`을 통해 동일한 작업을 더 빠르고 편리하게 수행합니다.

```csharp
var predictionEngine = model.CreatePredictionEngine<InputExample,OutputPrediction>(mlContext);
var prediction = predictionEngine.Predict(example);
```

예측 함수는 재진입하지 않고/스레드로부터 안전합니다. 여러 스레드에서 동시에 예측을 수행하려면 스레드당 예측 함수가 있어야 합니다.
