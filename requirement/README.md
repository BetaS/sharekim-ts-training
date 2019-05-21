# 목표

react + typescript + webpack 환경을 구축하여 Windows 기본 내장게임중 하나인 Klondike 솔리테어 게임을 구현합니다.

## 배울 수 있는것

- 기본적인 react 개발 환경 구축
- Component / props / state / vDOM등 react 기본 개념의 이해
- typescript 문법 / Object-oriented 클래스 설계
- state를 이용한 DOM Manipulation 처리
- click, drag등 각종 Event 처리
- firebase 연동 (axios)

## 과제 내용

권장 과제 수행 기간은 4~6주차로 진행되며 다음과 같은 milestone이 있습니다.

- [ ] 개발환경 (VSCode, tsconfig, babel, webpack, less-loader 등) 설정
- [ ] `GameBase`, `Deck`, `Card`, `CardStack`, `HiddenStack` 등 게임에서 기본적으로 사용되는 클래스들을 정의
- [ ] 정의한 클래스들의 `render()` 작성 및 RANDOM한 기본 게임판 설정
- [ ] 클릭을 이용한 카드 focusing 및 이동 처리
- [ ] Drag & Drop 처리
- [ ] GameRule 구현 / End Game State 정의
- [ ] axios 이용 firebase 서버와 스코어 연동
- [ ] 모바일을 고려한 반응형 / touchevnet 구현

# 게임 규칙

## 기본 용어 정의

![게임 기본 설정](./게임설정.jpeg)

- 좌측 상단 : `CardDeck` * 1개
- 우측 상단 : `SwapStash` * 4개
- 가운데 : `CardStack` * 7개

# 게임 상태

## Initialize State
- 처음에 52장(조커 제외)의 트럼프 카드가 들어있으며, 편의상 스페이드 문양만 사용합니다.
- 게임이 시작되면, CardDeck을 특정 사용자로 부터 `random seed`를 입력받아, `random` 함수에 의하여 shuffle 합니다.
- 각각 1, 2, 3, ..., 7 장을 해당하는 CardStack에 배치합니다. (이때 맨 마지막 카드만 펼쳐진 상태여야 함)
- 위의 로직에 따라 게임 판에 나와있지 않은(사용되지 않은 카드)가 순서대로 보관되어 있습니다.
- 이제 게임을 시작합니다.

## Playing State

### CardStack
- 게임에는 총 7개의 `CardStack` 이 있습니다.
- 펼쳐진 카드는 사용자가 자유롭게 움직일 수 있으나, 내림차순 정렬 (K->Q->J->9->...->->A) 로 배치할 수 있을때만 행동시 실행 (`commit`) 됩니다.
- 펼쳐진 카드는 중간에서 갈라서 다른 `CardStack`으로 움직일 수 있으며, 이를 `Split` 한다고 합니다.
- 만약 카드가 이동된 후 `K`->`A` 까지 하나의 `Deck` 이 완성된다면, `Folding` 되어 게임에서 사라집니다. 

### CardDeck
- 클릭시 한번에 1개씩만 펼쳐볼 수 있습니다.
- 모두 펼쳐보면 다시 처음으로 돌아갈 수 있습니다. (최대 3회까지 가능)
- CardStack 또는 SwapStack 으로 움직일 수 있습니다.
- 카드가 다른 stack으로 움직인 경우에, 이전에 보였던 카드를 다시 보여줍니다.

### SwapStash
- 카드를 움직이기 위해서 임시로 보관하는 장소입니다.
- 한개의 `Card` 만 배치될 수 있습니다.

## End State

- 만약 Play할 수 있는 카드가 없는경우
1. 아무런 카드도 게임에 남아있지 않으면 -> `승리`
2. 카드가 남아있다면 -> `패배` 

### 스코어

- 플레이한 시간을 `firebase`에 아이디와 함께 저장합니다.
- 이후, 자신이 상위 몇 % 인지 계산하여 보여줍니다.
- 다시 게임하기를 누르면 Initialize State로 되돌아 갑니다.

# QNA
- 김민수 CTO - mskim@sharekim.com
