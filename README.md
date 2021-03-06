# elasticsearch
## 개요
- Elastic 공식 이미지에 elasticsearch-analysis-seunjeon (2.3.3.0) 적용한 이미지
- 실행: `docker-compose up -d`

## 주의할 사항
- `./config/scripts` 폴더가 없을 경우 마운트했을 때 실행이 되지 않음 
    - 자동으로 생성하도록 만들 예정

## 예시

### 한글 분석기 적용한 인덱스 생성

```
curl -XPUT "http://localhost:9200/naver/?pretty" -d '{
	"settings": {
		"index": {
			"analysis": {
				"analyzer": {
					"korean": {
						"type": "custom",
						"tokenizer": "seunjeon_default_tokenizer"
					}
				},
				"tokenizer": {
			      "seunjeon_default_tokenizer": {
			        "type": "seunjeon_tokenizer",
			        "index_eojeol": false
			      }
			    }
			}
		}
	},
	"mappings": {
		"news": {
			"properties": {
				"oid": {
					"type": "string"
				},
				"aid": {
					"type": "string"
				},
				"url": {
					"type": "string"
				},
				"media": {
					"type": "string"
				},
				"datetime": {
					"type": "string"
				},
				"title": {
					"type": "string",
					"analyzer": "korean"
				},
				"content": {
					"type": "string",
					"analyzer": "korean"
				}
			}
		}
	}
}'
```

## TODO
- [ ] Docker는 기본 설정 상 로그를 json 형태로 저장하는데, 용량/파일개수 제한이 걸려있지 않고 또 열람하기에 불편. 개선 필요.
- [ ] backup/restore 스크립트 작성 필요 
