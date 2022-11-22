# 2020-2-VRstudio

## VR PACMAN 개발노트

### 1. 게임 요소 디자인.
팩맨은 간결한 디자인을 갖는 모델들을 사용하므로 외부 어셋을 활용하지 않고 직접 만들어서 구현.

1 - 1. 맵 제작
팩맨 기존 게임의 맵을 기반으로 제작. 

![image](https://user-images.githubusercontent.com/77597604/203217793-881dea42-fbea-4031-aab8-f3773c6f3fbf.png)


기술적 해결: 강의노트 Tutorial MobileVR_07_ENG 에서 아트 갤러리 만드는 과정을 보며 마야를 활용하여 제작.

1 - 2. 맵 디자인
기존 게임에 사용된 어두운 색상과 비슷하게 구현.

![image](https://user-images.githubusercontent.com/77597604/203217887-79e81d0b-d405-4ae0-876f-4b5ecc2881aa.png)

기술적 해결: 교수님의 피드백을 통해 수업시간에 사용했던 Standard Asset - Projectors Prefabs 의 Gridprojector 을 사용. Gridprojector의 속성 Aspect Ratio를 조정하고 이를 맵에 투영시켜서 맵 색상 구현. 

1 - 3. 적 제작
기존 게임의 적과 비슷한 모양으로 제작

![image](https://user-images.githubusercontent.com/77597604/203217942-c43c2803-c668-49a0-81a1-2f7418de0f82.png)

![image](https://user-images.githubusercontent.com/77597604/203218006-d4b93586-8eb2-4ca9-89bc-3983ac2ef235.png)
(시도 흔적)

https://www.youtube.com/watch?v=DtBZH0gRn6Q&list=PL8Zav_rb274kCgLrPy_NgxawKaB8_Ebgi&index=4

기술적 해결: 위에 기재한 마야 강의 유튜브 영상을 보며 이를 통해 제작. 
많은 시도 끝에 원하던 모델 제작.

1 - 4. 적 디자인
기술적 해결: 유니티의 Material 을 생성하여 각 적들에게 맞는 색상을 적용.
![image](https://user-images.githubusercontent.com/77597604/203218065-6a6542c0-0081-498d-acf9-a5b0beec3dbf.png)

1 - 5. 먹이, 아이템 제작, 디자인 
![image](https://user-images.githubusercontent.com/77597604/203218093-ea59bd54-d6c5-4c2a-9472-e59c8f2e6a4a.png)

기술적 해결: 유니티에서 Sphere 생성, 각각에 맞는 Material 색상 생성하고 적용. 아이템에는 Standard Asset - Projectors Prefabs 의 BlobLightProjector 을 입혀 빛이 나는 효과 입힘.

### 2. 게임 개발.
2 - 1. 카메라 (플레이어) 움직임 구현.
개발: 강의노트 Tutorial MobileVR_05 에 기재된 카메라가 보는 방향으로 움직이는 Script를 참고.  lookwalk script를 생성하여 카메라에 넣고 카메라 이동 구현. 카메라 Rotation Y를 조금 변화하는 애니메이션을 만들어 카메라에 적용. (플레이어 입장에서 걷는 것처럼 보이는 효과를 주기 위함)
문제점: 애니메이션을 카메라에 적용하니 카메라가 보는 방향으로 움직이지 않게됨. 카메라의 Rotation에 직접적인 변화를 주는 애니메이션으로 인해 생긴 에러.
해결 방안: 튜터링을 통해 해결. Capsule과 Empty object를 생성하고 Capsule에 Empty object를 넣고 Empty object에 카메라 넣기. Empty object에 애니메이션을 주고 Capsule에 보는 방향으로 걷는 lookwalk Script를 포함. 

2 - 2. 적 움직임 구현.
개발: 강의노트 Tutorial MobileVR_02 의 Ethan이 랜덤으로 배치되는 walktarget으로 움직이는 방법을 참고하여 적들의 움직임 구현. 맵을 4등분으로 나눈 각 구역에서 각 적들의 walktarget이 생성하도록 하여 활동하는 영역을 적끼리 겹치기 않게함.
문제점: X

2 - 3. 먹이 구현
역할: 플레이어가 맵에 남아있는 먹이를 모두 먹으면 게임 클리어.
개발: 먹이의 tag를 food로 지정해줌. 플레이어(Capsule)와 먹이의 isTrigger를 체크한 후 Check script를 생성하고 플레이어가 먹이와 충돌하면 OnTriggerEnter 함수를 통해 먹이가 사라지게 함. OnTriggerEnter에서 플레이어와 충돌하는 물체가 무엇인지 판별하기위해 플레이어와 부딪히는 물체가 충돌 시에 Tag를 확인하는 조건문을 넣어줌. Check script를 플레이어에 넣어줌. 
문제점: X

2 - 4. 아이템 구현
역할: 플레이어가 적을 공격가능하게 해줌.
개발: 아이템의 tag를 steroid로 지정하고 isTrigger를 체크한 후 먹이와 같은 방식으로 Check script에 기재. 아이템과 플레이어가 충돌하면 플레이어의 tag가 attack으로 3초동안 변하는 코루틴이 시작됨. 플레이어의 tag가 attack 이면 velocity가 2에서 3이 되도록 lookwalk script에 기재.
문제점: 본래에는 Update함수에 플레이어의 태그가 3초동안 attack으로 변하는 것을 적어서 3초가 지나도 플레이어의 태그가 계속 attack으로 되어있는 오류가 발생. Trigger함수와 Update 함수의 충돌에서 생긴 오류라고 생각함.
해결: 코루틴함수를 따로 적어 플레이어가 아이템과 충돌하면 코루틴이 시작되도록 함.

2 - 5. 적 죽음, 플레이어 죽음과 게임 재시작 구현
개발: 적의 tag는 enemy로 지정. check script 의 OnTriggerEnter함수에 플레이어의 tag가 attack이 아닐 시에 적과 충돌하면 플레이어에게 Explosion 효과가 나타나게 함. 플레이어의 tag가 finish가 되게함. check script 의 Update함수에서 플레이어의 tag가 finish이면 게임 오버 텍스트가 보여지게 하며 4초 후에 새로운 씬이 시작되게하여 게임이 재시작 되도록 함. lookwalk script의 Update함수에서 플레이어의 tag가 finish이면 velocity를 0으로 바꿔줌. 플레이어의 tag가 attack 일 때 적과 충돌하면 적이 파괴되며 적에게 Explosion 효과가 나타나게 함.
문제점: X

2 - 6. 적 색상 변경 구현
역할: 기존 게임에서 팩맨이 아이템을 먹으면 적들의 모양새가 바뀜을 오마주.
개발: changecolor script를 생성하고 각 적들에게 넣어줌. changecolor의 Update함수에서 플레이어의 tag가 attack이 아닐 때에는 본래 적들의 색상이 나타나도록하고 플레이어의 tag가 attack일 때에는 적들의 색상이 흰색으로 변하도록 함.
문제점: 본래에 적들의 색상이 변하는 내용을 check script에 다 포함시켜놨었음. 이 때 플레이어의 tag가 attack이 아님에도 적들의 색상이 계속 흰색이 되는 에러 발생. check script에서 너무 많은 내용들을 담아 생긴 오류라고 생각됨.
해결: changecolor script를 따로 생성하여 여기에 적들의 색이 변하는 내용을 기재.

2 - 7. 미니맵 구현
역할: 1인칭 시점으로 진행되는 게임에서 플레이어가 한층 더 편하게 플레이하기 위함.
개발: https://www.youtube.com/watch?v=F9QnjlrUUNU
위 유튜브 영상을 보며 미니맵을 구현. 플레이어에 새로운 카메라 생성. 카메라가 하늘에서 플레이어를 바라보도록 조정. Render Texture 를 생성한 후 카메라의 Target Texture 에 넣기. Raw image를 생성한 후 Raw image의 texture 에 Render Texture 넣기. 플레이어의 화면에 보이도록 조정. Raw image를 플레이어에 포함시키기.
문제점: X

2 - 8. 게임 시작 구현
역할: 플레이어가 자신이 팩맨이라는 것을 각인 시켜주기 위함.
개발: start script에 정해진 시간뒤에 object가 사라지는 코드를 구현. 게임 시작하기까지의 시간을 보여주는 text 생성. Raw image 생성. Raw image의 texture에 다운로드 받아놓은 팩맨의 사진 넣기. Raw image와 text가 플레이어의 화면에 잘 보이도록 세팅. 시작할 때에 이미지가 화면에 떠있는 동안 팩맨이 움직이지 않게 cube object 두 개 생성. cube에 투명한 material 적용. Raw image, text, 두 cube에 start script 넣기. 
문제점: X



2 - 9. 게임 클리어 구현
개발: check script의 Update함수에서 맵에 남아있는 먹이의 개수가 0이 되면 Time.timescale = 0 을 주어 게임이 진행이 멈추도록하고 게임 클리어 문구, 클리어 시간이 보여지도록 함.
문제점: X

2 - 10. 게임 오디오
개발: Tutorial MobileVR_05 를 참고. 오디오를 각 object에 포함. 플레이어가 먹이와 부딪히면 먹이 먹는 오디오, 플레이어가 아이템과 부딪힐 때 발생하는 오디오, 플레이어의 tag가 attack 일 때 적과 부딪힐 때 발생하는 오디오를 check script의 OnTriggerEnter함수에 각 tag에 맞는 물체와 충돌할 때에 발생하게 구현. 게임 오버 오디오는 Update함수에 플레이어 tag가 finish일 때 발생하도록 구현. 게임 시작오디오는 audio source의 play on awake를 체크하여 게임 시작할 때에 한번만 들리도록 함. 
문제점: X

