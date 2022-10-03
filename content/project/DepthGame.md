---
title: "DepthGame"
date: 2022-10-03T22:50:01+09:00
draft: false
tags: ["project"]
---

### 개요
Kinect V2를 이용해 제작한 실감형 미니게임 콘텐츠 개발 

### 기본정보
| | |
|-------------|----------------------------------------------------------|
| 개발인원 / 기간 |3명(기획1, 개발1, 디자인1) / 21.10~21.12(3개월)|
|개발 환경| Unity3D, Kinect V2 SDK, Window|
|기능| Unity3D 시스템 / Body Tracking / Kinect 디바이스 연동 / Firebase를 이용한 랭크 시스템|
|역할 / 기여도| 메인 개발자 / 40%|
|개발 목적| 교내 프로젝트, GGC 게임컨테스트 출품용|

### 아이디어
기획자와 디자이너와 브레인스토밍을 통해 미니게임 모음집 아이디어로 의견을 모으고 구현가능한 다양한 게임들을 기획했습니다.
이후 개발난이도를 분류하고 1주 1게임을 목표로 기한 내에 최대한 
많은 게임이 확보되도록 계획을 수립했습니다. 
Github의 Project 기능을 활용해 팀원들의 일정을 관리했습니다.

### 주요 결과물
- 시연영상 : https://www.youtube.com/watch?v=OM-PopWtTVA
- APK : https://github.com/beargrllys/VirtualReality/releases/tag/Execute_File
- 소스코드 : https://github.com/beargrllys/DepthGame


### 주요 코드

Kinect V2를 이용해 제작한 실감형 미니게임 콘텐츠 개발 

- 3D 트래킹 - 2D UI 해석 코드 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/BodyTracker.cs
- 게임 시작 코드 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/AnimationController.cs
- 게임 랜덤 실행 코드 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/GameManage.cs
- 주먹 피하기 게임 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/AvoidPunch.cs
- 청기백기 게임 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/FlagGame.cs
- 무궁화 게임 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/MuGungHwa.cs
- OX퀴즈 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/OXGame.cs
- 패널트킥 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/PK.cs
- 그릇쌓기 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/Plate.cs
- 스쿼트 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/Squart.cs
- 랭킹 시스템 : https://github.com/beargrllys/DepthGame/blob/main/Assets/script/Rank.cs
