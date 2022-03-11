---
layout: post
title: Qdrant
subtitle: Vector Search Engine
tags: [MACHINE_LEARNING, DEVELOP, STUDY]
cover-img: /assets/img/qdrant.png
comments: true
---

Deep Learning 모델의 작동 과정에서는 보통, 갖가지 방법으로 Human Readable 데이터로부터 **특징**을 추출하여 Vector 형태로 표현한다. 이미지와 같은 정적 데이터에서는 `Feature Extracting`라고 하고, 텍스트나 오디오 같은 Sequential 데이터에서는 `Embedding`이라고 한다. 이렇게 변환된 Vector는 입력 데이터의 중요 특징을 숫자로 표현한다. 

![](https://www.dropbox.com/s/h0e45hfwti79uy9/hearingseminar.gif?raw=1)

Deep Learning 모델에 기반한 검색 시스템을 만들 때에는 새로운 Query를 Pre-trained 모델으로 Vectorize한 후, DB내에 미리 Vectorize한 결과들과 Vector Similarity를 비교하여 가장 가까운 결과들을 리턴한다. Similarity는 가장 간단하게, [Cosine Similarity](https://en.wikipedia.org/wiki/Cosine_similarity)나 [Euclidean Distance](https://en.wikipedia.org/wiki/Euclidean_distance)를 이용한다. 비교 대상이 얼마 없을 때에는 N번 Cosine Similarity를 계산해도 별 문제 없을 수 있지만, 비교 대상이 점점 많아진다면 자연스레 검색에 요구되는 Cost가 높아지고, 검색 소요 시간이 길어진다.

검색 대상을 Vectorize하면서 미리 검색 Index를 만들어서 검색 Cost의 선형적 증가를 최대한 억제할 수는 없을까? Vector Search Engine이 그 일을 대신 해준다. Stand Alone 앱으로는 [Qdrant](https://github.com/qdrant/qdrant), [milvus](https://github.com/milvus-io/milvus)등의 프로젝트가 있고, Library도 [Fiass](https://github.com/facebookresearch/faiss)등 여러가지가 있다. [awesome-vector-search](https://github.com/currentsapi/awesome-vector-search)에 여러 프로젝트를 정리해놓았으니 참조하면 좋다.

# 🚀 Qdrant

본 포스트에서 다룰 Qdrant에서는 [Qdrant Documentation](https://qdrant.tech/documentation/)의 내용을 간략하게 정리한다. Qdrant는 `Semantic Search`, `Similar Image / Audio / Video Search`, `Recommendation Systems`에 적합하게 사용할 수 있다.

## 📥 Install 

Docker를 이용하면 아주 쉽게 Stand Alone Qdrant를 설치하고 실행할 수 있다.

```bash
docker pull qdrant/qdrant
```

```bash
docker run -p 6333:6333 -v $(pwd)/path/to/data:/qdrant/storage qdrant/qdrant
```

위의 명령어로 `/path/to/data`에 모든 데이터를 저장하며, 6333 포트를 이용하는 Qdrant Instance를 실행할 수 있다.

이후에는 ☸️`Kubernetes`를 이용해서 Orchestration을 할 수 있다.

## 📄 Collections

Qdrant에는 크게 두 개의 개념이 있다. 첫번째가 Collection, 두번째가 Point다. Point는 각 Vector 데이터를 의미하며, Collection은 이러한 Point들의 Set이다. 같은 Collection에 속한 Point들은 같은 Dimentionality를 가지며, 같은 metric으로 비교된다. Qdrant는 Vector의 비교에서 가장 흔히 사용되는 아래의 세가지 metric을 제공한다.

- Dot product: Dot - [https://en.wikipedia.org/wiki/Dot_product](https://en.wikipedia.org/wiki/Dot_product)
- Cosine similarity: Cosine - [https://en.wikipedia.org/wiki/Cosine_similarity](https://en.wikipedia.org/wiki/Cosine_similarity)
- Euclidean distance: Euclid - [https://en.wikipedia.org/wiki/Euclidean_distance](https://en.wikipedia.org/wiki/Euclidean_distance)

Search Engine도 Indexing을 지원하는 일종의 DB로 볼 수 있다. CRUD를 지원한다. Qdrant는 REST API를 통해 거의 대부분의 조작이 가능하다. 자세한 REST API 옵션은 [https://qdrant.github.io/qdrant/redoc/index.html#section](https://qdrant.github.io/qdrant/redoc/index.html#section)에 [Redoc](https://github.com/Redocly/redoc) 포멧으로 잘 설명되어 있으니, 확인하면 좋습니다.

### Create Collection

```
PUT /collections/example_collection

{
    "name": "example_collection",
    "distance": "Cosine",
    "vector_size": 300
}
```

이외의 옵션들은 위에서 언급한 대로 [schema definitions](https://qdrant.github.io/qdrant/redoc/index.html#operation/create_collection)와 [configuration file](https://github.com/qdrant/qdrant/blob/master/config/config.yaml)에서 확인할 수 있다.

### Delete Collection

```
DELETE /collections/example_collection
```

### Update Collection Parameters

[Create Collection](#create-collection)에서 적용한 Collection Parameter의 개별적 수정이 가능하다. 예를들어, plain-indexing을 허용하는 points들의 최대값을 default 값인 20,000에서 10,000으로 수정하고 싶다면 아래와 같은 명령을 보내면 된다.

```
PATCH /collections/example_collection

{
    "optimizers_config": {
        "indexing_threshold": 10000
    }
}
```

## 🔗 Collection Aliases

Production 환경에서는 새로운 ML모델으로 업그레이드 하는 등의 상황에서 다른 Vector 버전으로 **끊김 없이** 바꾸어야 하는 상황이 있을 수 있다. Alias를 이용하면 새로운 Vector를 위해 Collection을 리빌드할 때까지 기존 서비스를 중단해야할 필요가 없어진다.

### Create Alias

```
POST /collections/aliases

{
    "actions": [
        {
            "create_alias": {
                "alias_name": "production_collection",
                "collection_name": "example_collection"
            }
        }
    ]
}
```

### Remove alias

```
POST /collections/aliases

{
    "actions": [
        {
            "delete_alias": {
                "alias_name": "production_collection"
            }
        }
    ]
}
```

### Switch collection

```
POST /collections/aliases

{
    "actions": [
        {
            "delete_alias": {
                "alias_name": "production_collection"
            }
        },
        {
            "create_alias": {
                "alias_name": "production_collection",
                "collection_name": "new_collection"
            }
        }
    ]
}
```

## Points

Point는 Qdrant에 저장된 Vector값을 의미하며, 각 Point는 필요에 따라 추가적인 [Payload](#payload) 데이터를 구성할 수 있다. Point 데이터의 Modification Operation은 2단계로 수행된다. 첫번째 단계에서는 Operation이 `Write-ahead-log`에 작성된다. API가 `&wait=false` 파라미터와 함께 호출되거나 `&wait` 파라미터가 생략된다면, client는 아래와 같이 "데이터를 받았음"을 의미하는 acknowledgment를 받게 된다.

```
{
    "result": {
        "operation_id": 123,
        "status": "acknowledged"
    },
    "status": "ok",
    "time": 0.000206061
}
```

이 Response를 받았더라도 아직 해당 Point가 서비스에 적용된 것은 아니다. `Write-ahead-log`에 작성된 Operation은 백그라운드에서 비동기적으로 실행된다. 만약 Modification Operation의 즉각적인 적용을 원한다면 API 호출에서 `&wait=true` flag를 이용해야 한다. 이 경우에는 아래와 같은 Response를 받게 된다.

```
{
    "result": {
        "operation_id": 0,
        "status": "completed"
    },
    "status": "ok",
    "time": 0.000206061
}
```

### Point IDs

Qdrant는 Point의 id로 `64-bit unsigned integers`와 `UUID`를 지원한다. 

### Upload Points

Point를 업로드할 때에도 역시 REST-API를 이용한다. Qdrant에서는 Batch Loading을 지원한다. 업로드 양이 많을 때는 첫번째 방법을 이용하는 것이 좋다.

```
PUT /collections/{collection_name}/points

{
    "batch": {
        "ids": [1, 2, 3],
        "payloads": [
            {"color": "red"},
            {"color": "green"},
            {"color": "blue"}
        ],
        "vectors": [
            [0.9, 0.1, 0.1],
            [0.1, 0.9, 0.1],
            [0.1, 0.1, 0.9],
        ]
    }
}
```

```
PUT /collections/{collection_name}/points

{
    "points": [
        {
            "id": 1,
            "payload": {"color": "red"},
            "vector": [0.9, 0.1, 0.1]
        },
        {
            "id": 2,
            "payload": {"color": "green"},
            "vector": [0.1, 0.9, 0.1]
        },
        {
            "id": 3,
            "payload": {"color": "blue"},
            "vector": [0.1, 0.1, 0.9]
        },
    ]
}
```

Qdrant의 모든 API들은 같은 Method를 복수번 실행하더라도 같은 결과를 낸다. 예를들어, 같은 `id`에 대한 Upload 명령에는 '덮어쓰기'가 된다.

### Modify Points

Point의 Vector값 또는 Payload를 수정할 수 있다. Vector값 수정의 경우, 현재(2022년 3월) 기준으로는 재업로드가 유일한 방법이다. 따라서 여기서는 Payload의 수정을 중점적으로 다룬다.

#### Set Payload  
```
POST /collections/{collection_name}/points/payload

{
    "payload": {
        "property1": "string",
        "property2": "string"
    },
    "points": [
        0, 3, 100
    ]
}
```

#### Delete Payload Keys  

```
POST /collections/{collection_name}/points/payload/delete

{
    "keys": ["color", "price"],
    "points": [0, 3, 100]
}
```

#### Clear Payload  

Clear를 이용하면 해당 Point의 모든 Payload Key를 삭제한다.
```
POST /collections/{collection_name}/points/payload/clear

{
    "points": [0, 3, 100]
}
```

### Delete Points

```
POST /collections/{collection_name}/points/delete

{
    "points": [0, 3, 100]
}
```

`filter`를 이용해서 Point의 삭제가 가능하다.

```
POST /collections/{collection_name}/points/delete

{
    "filter": {
        "must": [
            {
                "key": "color"
                "match": {
                    "keyword": "red"
                }
            }
        ]
    }
}
```

위의 예제는 Collection에서 `{ "color": "red" }` Payload를 가진 모든 Point를 삭제한다.

### Retrieve Points

Point의 `id`를 이용해서 point 데이터를 얻을 수 있다. 

```
POST /collections/{collection_name}/points

{
    "ids": [0, 3, 100]
}
```

`with_vector` 또는 `with_payload` 파라메터를 이용해서 Point의 Vector값이나 Payload도 함께 리턴받을 수 있다.

```
GET /collections/{collection_name}/points/{point_id}
```

### Scroll Points

Scroll method를 이용하면 id를 모르는 상태에서 모든 Point에 접근하거나, `filter`를 이용해서 일치하는 Point를 iterate할 수 있다.

```
POST /collections/{collection_name}/points/scroll

{
    "filter": {
        "must": [
            {
                "key": "color",
                "match": {
                    "keyword": "red"
                }
            }
        ]
    },
    "limit": 1,
    "with_payload": true,
    "with_vector": false
}
```

위의 API Calldms `color = red`인 모든 Point를 아래와 같이 리턴한다.

```
{
    "result": {
        "next_page_offset": 1,
        "points": [
            {
                "id": 0,
                "payload": {
                    "color": { "type": "keyword", "value": [ "red" ] }
                }
            }
        ]
    },
    "status": "ok",
    "time": 0.0001
}
```

Scroll API는 `filter`에 매치되는 모든 Point를 page-by-page로 리턴한다. 결과는 Point의 ID를 기준으로 정렬된다. 다음 Page를 Query하기 위해, 현재 Page에서 가장 큰 Point의 ID를 API Call에서 Query의 `offset` 필드로 입력해야한다. 여기에 입력되어야 할 ID는 Response의 `next_page_offset` 필드를 보고도 알 수 있다. 만약 Response의 `next_page_offset`이 `null`이면 마지막 페이지로서, 다음 페이지가 없음을 뜻한다.

## Payload

Qdrant의 대표적 기능이 바로 Vector에 붙어있는 Payload 개념이다. Payload는 `key-value`로 구성되고, 각 key는 같은 Type의 여러 value를 가질 수 있다.

```
{
    "colors": ["red", "blue"],
    "price": 11.99,
    "locations": [
        {
            "lon": 52.5200, 
            "lat": 13.4050
        }
    ]
}
```

Qdrant는 value의 Type을 자동으로 인식하지만, 원한다면 아래와 같이 Type을 명시할 수도 있다.

```
{
    "colors": {
        "type": "keyword",
        "value": ["red", "blue"] 
    },
    "price": {
        "type": "float",
        "value": 11.99
    },
    "locations": {
        "type": "geo",
        "value": [
            {
                "lon": 52.5200, 
                "lat": 13.4050
            }
        ]
    }
}
```

Qdrant에서 이용 가능한 Type은 아래와 같다.

- integer - 64-bit integer in the range -9223372036854775808 to 9223372036854775807.
- float - 64-bit floating point number.
- keyword - string value.
- geo - Geographical coordinates. Example: { "lon": 52.5200, "lat": 13.4050 }

같은 key에 해당하는 value는 모두 같은 Type으로 구성되어야 한다.

### Payload Filtered Search

Qdrant는 Payload의 Value 기반 검색 기능을 제공한다.

### Create Point with Payload

```
PUT http://localhost:6333/collections/{collection_name}/points

{
    "points": [
        {
            "id": 1,
            "vector": [0.05, 0.61, 0.76, 0.74],
            "payload": {"city": "Berlin", price: 1.99}
        },
        {
            "id": 2,
            "vector": [0.19, 0.81, 0.75, 0.11],
            "payload": {"city": ["Berlin", "London"], price: 1.99}
        },
        {
            "id": 3,
            "vector": [0.36, 0.55, 0.47, 0.94],
            "payload": {"city": ["Berlin", "Moscow"], price: [1.99, 2.99]}
        }
    ]
}
```

### Update Payload

#### Set Payload

```
POST /collections/{collection_name}/points/payload

{
    "payload": {
        "property1": "string",
        "property2": "string"
    },
    "points": [
        0, 3, 100
    ]
}
```

#### Delete Payload

특정 Point의 특정 Payload Key를 삭제할 수 있다.

```
POST /collections/{collection_name}/points/payload/delete

{
    "keys": ["color", "price"],
    "points": [0, 3, 100]
}
```

#### Clear Payload

특정 Point의 모든 Payload를 삭제할 수 있다.

```
POST /collections/{collection_name}/points/payload/clear

{
    "points": [0, 3, 100]
}
```

### Payload Indexing

Filter를 통한 검색 효율 증진을 위해, Qdrant는 특정 Payload 필드의 인덱싱 기능을 제공한다. Payload Index는 일반적인 Document-oriented 데이터베이스에서의 방식과 비슷하다. 

```
PUT /collections/{collection_name}/index

{
    "field_name": "name_of_the_field_to_index"
}
```

Index 설정 후에는 다음과 같이 collection info API에서 제공하는 Payload Schema에 `indexed` flag를 확인할 수 있다.

```
{
    "payload_schema": {
        "property1": {
            "data_type": {
                "type": "keyword"
            },
            "indexed": true
        },
        "property2": {
            "data_type": {
                "type": "integer"
            },
            "indexed": false
        }
    }
}
```

## Search

### Similarity Search

### Metrics

### Query Planning

### Search API

#### Payload in Vector in the Result

### Recommendtaion API