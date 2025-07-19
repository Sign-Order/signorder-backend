# Go Backend Server

## 1. 프로젝트 개요

본 프로젝트는 수화 번역 서비스를 위한 종합적인 백엔드 서버로, Go 언어로 개발되었습니다. 레스토랑 주문 시스템과 수화 번역 기능을 결합하여, 청각 장애인 고객이 편리하게 주문할 수 있도록 돕는 서비스의 핵심 인프라입니다.

### 주요 기능
- **다중 프로토콜 지원**: REST API, WebSocket, gRPC를 통한 다양한 클라이언트 지원
- **실시간 수화 번역**: 외부 AI 서버와 연동하여 한국어를 수화 영상으로 변환
- **스트리밍 처리**: 카메라 프레임 스트리밍을 통한 실시간 수화 인식 및 번역
- **실시간 통신**: WebSocket을 통한 관리자와 카운터 앱 간 실시간 메시지 전송
- **보안**: API 키 기반 인증 및 TLS 암호화 통신



## 2. 폴더 구조
```
server/
├── cmd/
│   └── myapp/                  # 애플리케이션 진입점
├── gen/                        # 자동 생성된 gRPC 코드
├── internal/
│   └── pkg/
│       ├── database/
│       │   └── mongodb/        # MongoDB 연동
│       ├── grpc/               # gRPC 서버
│       ├── rest/               # REST API 서버
│       ├── websocket/          # WebSocket 서버
│       └── utils/              # 유틸리티 (AI 클라이언트 등)
├── proto/                      # Protobuf 정의 파일
├── .dev.env                    # 환경 변수
├── Dockerfile                  # Docker 설정
├── Makefile                    # 빌드 스크립트
└── nginx.conf                  # Nginx 설정
```

## 3. 기술 스택

- **언어**: Go 1.22.0
- **프레임워크**: Gin (REST), Gorilla WebSocket, gRPC
- **데이터베이스**: MongoDB
- **인프라**: Docker, Nginx

## 4. 설치 및 실행

### 실행 방법
```bash
# 개발 환경
make dev

# Docker 실행
make docker_dev

# Protobuf 코드 생성
make proto
```

## 5. API/모듈 설명

### gRPC 서버 
- **스트리밍 수화 인식**: 카메라 프레임 → AI 서버 → 한국어 번역
- **매장/메뉴/주문 관리**: CRUD 작업
- **인증**: API 키 기반 인터셉터

### REST API 서버
- POST /rest/store # 매장 생성
- GET /rest/store/list # 매장 목록
- POST /rest/menu/:store_code # 메뉴 생성
- POST /rest/order/:store_code # 주문 생성
- GET /rest/messages/:store_code # 메시지 조회

### WebSocket 서버
- **실시간 알림**: 주문/문의 알림 전송
- **수화 번역**: 한국어 → 수화 영상 URL 전송
- **클라이언트 구분**: `counter_app`, `manager_web`

### Nginx
- `/api/*` → REST API 
- `/APIService` → gRPC 
- `/ws` → WebSocket 