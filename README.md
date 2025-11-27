# new-auction-description

# New Auction System

## 시스템 아키텍처
```
┌─────────────────────────────────────────────┐
│              Client (Browser)               │
└─────────────────┬───────────────────────────┘
                  │
         ┌────────▼────────┐
         │   API Gateway   │ (8080)
         │  - JWT 검증     │
         │  - Rate Limiting│
         └────────┬────────┘
                  │
         ┌────────▼────────┐
         │  Eureka Server  │ (8761)
         │Service Discovery│
         └────────┬────────┘
                  │
      ┌───────────┼───────────┐
      │           │           │
┌─────▼─────┐ ┌──▼───┐ ┌─────▼─────┐
│   User    │ │Product│ │  Auction  │
│  Service  │ │Service│ │  Service  │
│  (8081)   │ │(8082) │ │  (8083)   │
└─────┬─────┘ └──┬───┘ └─────┬─────┘
      │          │           │
      └──────────┼───────────┘
                 │
    ┌────────────┼────────────┐
    │            │            │
┌───▼───┐   ┌───▼───┐   ┌───▼───┐
│ Redis │   │ Kafka │   │ MySQL │
│ 분산락 │   │ 이벤트 │   │메인 DB│
│ 캐싱   │   │       │   │       │
└───────┘   └───────┘   └───────┘
```

## 서비스 구성

### Infrastructure Services
- **Eureka Server** (8761): Service Discovery
  - Repository: `new-auction-eureka-server`
- **API Gateway** (8080): API 라우팅 및 인증
  - JWT 검증
  - Rate Limiting

### Business Services
- **User Service** (8081): 사용자 관리
- **Product Service** (8082): 상품 관리
- **Auction Service** (8083): 경매 처리

### Data & Message Layer
- **Redis**: 분산 락, 캐싱
- **Kafka**: 이벤트 기반 메시징
- **MySQL**: 메인 데이터베이스

## 기술 스택
- Spring Boot
- Spring Cloud Netflix (Eureka)
- Spring Cloud Gateway
- Redis
- Apache Kafka
- MySQL
- JWT

## 시작하기

### 실행 순서
1. Eureka Server 실행 (8761)
2. API Gateway 실행 (8080)
3. 각 마이크로서비스 실행 (8081-8083)

### 필수 인프라
```bash
# Redis 실행
docker run -d -p 6379:6379 redis

# Kafka 실행
docker-compose up -d kafka

# MySQL 실행
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql
```

