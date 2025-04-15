# 자주 묻는 질문

**Q: 해석시간을 줄이는 방법 중 Mass Scaling하는 방법?**  
A: 다음과 같은 순서로 진행합니다.
    1) DT2MS를 0을 놓고 돌려본다 : Smallest timestep을 확인하기 위해서 해석을 진행함.
    2) 해석이 다 돌아간 후, d3hsp 파일을 확인 : 문서에서 'smallest timesteps' 를 검색하여 값 확인 (리스트 중 가장 작은 값 기준, 예를 들어 6.3e-08 이었다고 가정)
    3) Smallest timestep 보다 큰 DT를 적용할 때, DT값이 "-"부호를 붙인다.
    4) DT2MS값에 -3.0e-07을 적용함 (6.3e-08 * 5배를 적용한 후, 앞에 마이너스 부호를 붙임)
    5) 해석을 재진행
    6) Solving window에서 add mass, percentage increase check
    7) normal termination이 뜨면 해석 정상종료, percentage increae < 2% 이면 수행됨
    8) percentage increae > 2% 이면 DT2MS를 더 작은 값으로 수정하여 다시 해석을 돌린다. (ex. DT2MS에 -2.5e-07을 넣음)

**Q: Shell type에서 추천값을 알려줘**  
A: 1)해석 시간을 줄이기 위해서는 : SECTION_SHELL에서 ELFORM = 2, NIP = 3, HOURGLASS에서 IHQ = 4, CONTROL_SHELL에서 ISTUP=4(두께 변화 고려 시), BCW=1, PROJ =1
   2)정확도를 향상시키기 위해서는 : SECTION_SHELL에서 ELFORM = 16, NIP = 5, HOURGLASS에서 IHQ = 8(for warped elements), CONTROL_SHELL에서 ISTUP=4(두께 변화 고려 시)

**Q: Negative volume을 막는 방법을 알려줘**  
A: 높은 하중과 많은 양의 압축을 받는 foam material을 모델링 할 때 발생할 수 있는 일반적인 error는 solid element의 'negative volume'이다.
solid 요소에서 강성차가 심할 때도 문제가 되며, 순간적으로 약한쪽이 negative volume이 발생하는 문제가 생김
이것은 solid element의 한 면이 다른 면을 통과하면서 발생되는 문제이며, error report에서 *** error negative volume in solid element ***와 같이 나타난다.
One method to solve this problem is to increase the stiffness of the foam material as it starts to lock up at about 80~90% crush (strain = 0.8 ~ 0.9).
This is done by editing the stress-strain curve used for the foam material.
(즉 SS커브에서 strain이 0.8~0.9정도 진행되었을때부터의 stress 값을 확 올려서, negative volume을 방지한다.)
Another method is to put a layer of null shell on the top and bottom (front/back) faces of the foam. A contact between these to layers of null shells is then used to prevent the solid elements from becoming too compressed.
The contact thickness between the two plates is set to about 10% of the thickness of the foam block so that the contact will act to lock up the foam block at 90% crush.
(Null Shell element는 *MAT_NULL 을 사용하며, typical properties는 density = 0.1 * foam density, Young's modulus = 1GPa, thickness = 1mm )
별도로 모델링을 하여, 약한 쪽에게 MAT_NULL카드를 이용하여 껍데기를 씌운다.

**Q: Dynamic relaxtion 방법 알려줘**  
A: DR is a convenient way to preload a model prior to applying dynamic loads in the ensuing transient analysis. It's a way to clearly distinguish the preloading phase from the transient phase.
dynamic relaxation을 통해 안정화시킨후, 그것을 reference로 다시 만들어서 돌리면 해석을 안정적으로 진행할 수 있음
1) LS-prepost의 output 기능으로 dynain 을 생성 : dynain file로 저장
2) 결과물 중 예를 들어 d3plot의 마지막 번호가 102번이라면, 그것을 dynain format으로 저장 : 해당 요소에 걸린 stress, strain을 가지고 다시 해석을 진행할 수 있음
   - 저장된 파일 내부에는 해당 키워드만 가진다 : ELEMENT_SHELL_THICKNESS, NODE, INITIAL_STRESS_SHELL
3) dynain파일에서는 요소, 노드 정보만 가지고 있기때문에, 나머지는 원래의 해석파일에서 다 복사해서 가지고 옴
4) 이 상태에서 해석을 진행하면, 예를 들어 한번 해석을 진행했던 찌그러진 형상에서 그대로 해석을 진행할 수 있음



