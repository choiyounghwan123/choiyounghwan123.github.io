---
title: "[데이터베이스] 샤딩이란 무엇인가?"
excerpt: "대규모 서비스에서 필수적인 데이터베이스 분산 기법, 샤딩의 개념과 구현 방법, 그리고 실제 적용 시 고려사항에 대해 알아봅니다."

categories:
  - Database
tags:
  - [데이터베이스, 샤딩, 분산시스템]

permalink: /database/sharding/

toc: true
toc_sticky: true

date: 2025-05-23
last_modified_at: 2025-05-23
---

오늘은 데이터베이스 강의를 수강하면서 배운 샤딩(Sharding)에 대해 알아볼것이다. DB Sharding은 데이터가 급격하게 증가하게 되거나 트래픽이 특정 DB로 몰리는 상황을 대비해서 빠르고 유연하게 안전하게 DB를 증설할 수 있게 한다. Sharding에 대해 알아보자.

# 샤딩

**샤딩(Sharding)**은 DB 트래픽을 분산할 수 있는 중요한 수단이다. 또한 특정 DB의 장애가 전면 장애로 이어지지 않게 하는 역활도 한다.

샤딩은 각 DB서버에서 데이터를 분할하여 각 DB서버마다 저장하는 방식이다. 똑같은 Table에 대해서 데이터가 나뉘어져 있으니 어느 DB서버에 접속할지 모르는 문제가 발생할 수 있다. 그래서 샤딩키라는 개념이 나와 동적으로 DB서버와 매핑하게된다.

![수평적 파티셔닝과 샤딩 예시](/assets/images/horizontal-partitioning.png)
*그림 2. 수평적 파티셔닝(Vertical/Horizontal Partitioning)과 샤딩(Sharding) 예시*

