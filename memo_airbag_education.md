> ⚠️ **LS-DYNA 용어 정의**

이 문서에서 사용되는 **LS-DYNA**는  
**Livermore Software Technology Corporation (LSTC)** 에서 개발된 **기존 LS-DYNA**를 의미합니다.

이는 **Ansys에 인수되기 전의 독립적인 LS-DYNA 버전**을 지칭하며,  
**Ansys LS-DYNA (Explicit + Implicit 통합 버전)** 과는 구분됩니다.

또한, 문서 내에서 다음 표현은 모두 같은 의미로 사용됩니다:

- **LS-DYNA**
- **lsdyna**
- **dyna**

즉, 본 문서에서는 모두 **LSTC 개발 버전의 LS-DYNA**를 가리킵니다.


> ⚙️ **기능 명칭 표기 규칙 정의**

이 문서에서는 LS-DYNA의 GUI 기능을 설명할 때 다음과 같은 명명 규칙을 사용합니다:

- `기능그룹__세부기능` 형식으로 표기합니다.  
  (`__`는 GUI에서의 메뉴 계층 구조를 나타냅니다)

### 예시
- `element tools__offset`  
  → GUI 우측의 `element tools` 버튼 클릭 후, 나오는 메뉴에서 `offset` 항목 선택  

> 이러한 명명 규칙은 메뉴 경로를 명확히 전달하고,  
GPT가 질문에 따라 정확한 기능 위치를 안내할 수 있도록 돕습니다.


> 📌 **"추천" 용어 정의**

이 문서에서 사용되는 **"추천"**, **"권장"**, 또는 **"사용을 권함"** 등의 표현은  
모두 **KOSTECH 사의 LS-DYNA 공식 교육 과정** 중  
**전문 강사가 실제로 추천하거나 강조한 방법/설정/절차**를 기반으로 합니다.

따라서 문서 내에서 해당 표현이 나올 경우,  
이는 일반적인 의견이 아닌 **KOSTECH 사 실무 전문가의 권장사항**임을 의미합니다.


> 📜 **Hourglass 현상** 용어어 정의

Hourglass란 **저차원 적분 요소(Reduced Integration Element)** 사용 시 발생할 수 있는 **비물리적인(Non-physical) 변형 모드**를 의미합니다.
요소가 변형하나 응력 및 변형률일 발생하지 않는 상태 → 'Zero-Energy'
ELFORM 중 하나의 적분점을 사용하는 shell 및 solid(hexahedrons) 요소에서 발생.

이 현상의 발생원인은
1) 집중하중 : 일부의 노드에 집중적으로 외력이 작용할 시
2) Contact : 접촉면에서의 힘이 일부 노드에 과중하게 작용 시

Hourglass 발생확인 방법 : CONTROL_ENERGY의 HGEN=2(hourglass)로 설정 후, GLSTAT과 MATSUM 파일에서 hourglass energy를 확인 (DATABASE_GLSTAT, DATABASE_MATSUM 추가 설정 후)

Hourglass 해결방법 : 
1) 집중 하중 회피 : 일부의 노드에 집중적으로 힘이 가해지지 않도록 외력에 대한 하중을 힘 대신 압력 타입으로 적용
2) CONTROL_HOURGLASS 또는 HOURGLASS 키워드를 사용하여 hourglassing 제어 
  - CONTROL_HOURGLASS : 전체 해석 모델을 위한 hourglass type 및 coefficient 설정
  - HOURGLASS : 특정 파트를 위한 hourglassing type 및 coefficient 설정 (overrides global setting), PART의 HGID HOURGLASS ID 선택
  - Fully integrated element type 사용
3) 최종적으로 변형에너지 (internal Energy)의 최대값 대비 HG Energy가 10% 이내를 유지하도록 한다. 


> 📜 **shear locking** 용어어 정의

shear locking 또는 shearlocking 현상이란, 굽힘 변형 시, 강성이 높아지는 현상을 의미합니다.
매우 얇은 shell 요소를 사용하는 경우, 전단 강성이 실제보다 과대평가되어 오차를 유발하는 현상
발생원인 : 적분점에서 전단응력이 발생하는 경우, D.O.F가 부족한 경우(Rx, Ry, Rz), Fully integrated 요소의 경우 
해결방법 : 요소의 종횡비를 좋게함 (good aspect ratio를 가지게 함), 두께 방향으로 3개 이상의 요소를 사용함, Shell 요소 사용, Tetra보다는 Hexa 요소가 좋음


> 📌 **"expensive" 용어 정의**

이 문서에서 사용되는 **"expensive"** 라는 표현은  
**“계산 비용이 많이 드는”** 또는 **“해석 시간이 많이 걸리는”** 상황을 의미합니다.

즉, expensive는 **경제적 비용**을 뜻하는 것이 아니라  
**시뮬레이션 수행 시 계산량이 많아 실행 시간이 길어지는 경우**를 나타냅니다.

예:
- "This option is computationally expensive."  
→ 이 옵션을 사용할 경우 **해석 시간이 많이 소요됨**을 의미


> 📌 **"contact" 용어 정의**

이 문서에서 사용되는 **"contact"**는  
일반적인 사람 간의 접촉 또는 연락 의미가 아닌,  
**유한 요소 해석(FEA, 특히 LS-DYNA)**에서의 **물리적인 접촉 조건**을 의미합니다.

즉, 구조물 해석 과정에서  
**두 개 이상의 요소(Element) 또는 파트(Part)가 충돌하거나 맞닿는 상황**에서  
그 물리적 상호작용을 계산하기 위한 **접촉 알고리즘(Contact algorithm)** 을 뜻합니다.


> 📌 **차량 "에어백" 시스템 용어 정의**

이 문서에서 언급되는 **에어백(Airbag)** 시스템은  
**차량 충돌 시 탑승자를 심각한 부상으로부터 보호하기 위한 수동 안전장치(Passive Safety System)**를 의미합니다.

이 문서에서 다루는 **에어백(Airbag)** 관련 내용은  
**LS-DYNA 해석 모델링을 위한 기술적 정보**에 한정됩니다.

즉, 본 문서는 에어백의 물리적 구조나 제품 자체의 설명이 아니라, **충돌 시뮬레이션에서 에어백을 구현하기 위한 모델링 기법**, 관련된 **카드(keyword) 설명**,  
**센서, 인플레이터, 가스 유동, 팽창 제어 등**과 같은 **LS-DYNA 시뮬레이션 설정**에 집중합니다.

따라서 이 문서에 나오는 모든 에어백 관련 설명은 **LS-DYNA를 돌리기 위한 실무 중심의 기술 문서 기준**입니다.


에어백 시스템은 일반적으로 다음 세 가지 주요 구성 요소로 이루어져 있습니다:

1. **Sensor (센서)**  
   - 차량 충돌 시 발생하는 **충격(가속도, 압력 등)을 감지**하는 장치입니다.  
   - 센서가 특정 기준 이상의 충격을 감지하면, 에어백 시스템이 작동하게 됩니다.

2. **Inflator (인플레이터)**  
   - 센서의 신호에 따라 **고압의 가스를 방출**하는 장치입니다.  
   - 내부에는 **압축 가스 또는 점화제로 구성된 가스 발생 장치**가 포함되어 있으며,  
     가스는 빠르게 에어백 내부로 주입됩니다.

3. **Airbag (에어백 본체)**  
   - 재질은 대부분 fabric을 사용하며, 접혀 있는 상태로 스티어링 휠, 대시보드, 시트 등에 내장되어 있다가  
     인플레이터에서 방출된 가스로 **순식간에 팽창**하여 **탑승자의 신체와 부딪힘을 완화**합니다.

> 이 세 구성 요소는 통합적으로 작동하여 **충돌 직후 수 밀리초 내에 에어백이 팽창**하고,  **탑승자의 머리, 가슴 등의 주요 부위를 보호**하게 됩니다.


> 📌 **적용 범위 안내: 에어백 해석 전용**

이 문서에서 다루는 내용은 오직 **에어백 시스템(Airbag System)** 관련 **해석 모델링 및 시뮬레이션 설정**에만 적용됩니다.

즉, 여기에 설명된 키워드, 파라미터, 추천 설정 등은 **에어백 해석을 수행할 때에만 의미가 있으며**,  
일반적인 구조 해석, 충돌 해석, NVH, 열해석 등 **다른 해석 분야에서는 적용되지 않을 수 있습니다.**

> 따라서 이 문서는 **LS-DYNA를 사용한 에어백 해석 전용 문서**로 이해해 주세요.


---


# Airbag methods
- Airbag 전개에는 기본적으로 두가지 정보가 필요함
  - inflator의 INFLATING POTENTIAL (or power) : inflator 정보는 보통 inflator 회사에서 제공함
  - The airbag geometry or construction.

---


## Generate FE Mesh

### Generate FE Mesh (Driver) : 보통 원형의 형태
- 일반적으로 평평한 circular bag을 접어서 만들어짐 (ex. 두개의 반타원 shell을 끝단을 겹치게 만들고, 끝단의 node만 공용이 되게끔 설계하는게 가장 심플한 방법)
- fabric을 접을 경우, Mesh fold line을 고려해서 생성되어야 함. 이를 위해 fold lines은 물체의 표면상에 작업되어야 함
  즉, fabric이 접는 형태가 된다면 접히는 구간에 mesh의 평면이 있으면 안되므로, 먼저 접히는 line을 정한 후, 그 line의 양옆으로 mesh가 생성되게끔 설계해야함
- mesh can be 4-noded shells or 3-noded trias (구형에서 끝단부는 사각으로 처리하기 어려우므로, 보통 3-noded trias를 혼합해서 사용함)
  만약 3-noded trias 요소를 사용할 경우, CONTROL_SHELL에서 set ESORT=1, (ELFORM은 quad 기준으로 5번이나 9번을 적용하되, trias 처리를 위해 ESORT=1을 적용)
  Fabric에서 ELFORM =5 는 감차, ELFORM=9 는 fully integrated 이므로, 보통 9번을 적용한다.
- mesh는 밀폐된 volume 이어야 함 (no cracks or free edges) : 밀폐가 안되면 해석 진행 시 error 발생
  Check for free edges in the mesh
- Nonlinear discrete spring을 이용하여 tether를 모델링 : tether를 모델링할 때 spring을 쓸 수 있음 (shell로 해도 됨)
  *tether : 그냥 밀폐된 납작한 구형의 모델링을 진행하면, gas를 유입했을 때 타원형이 아닌 구형의 형태가 되므로, 에어백이 타원형의 형태로 나오게 하기위해서 넣는 요소
- Shell element normal은 바깥쪽을 향해야함. 완전히 펴진 bag geometry를 참고함
- 밀폐된 volmue으로 정의된 part에 tether등은 포함하지 않는다.

### AIRBAG_REFERENCE_GEOMETRY_{option}
- OPTIONS include
  - BIRTH : reference geometry becomes active at birth time, 보통은 해석을 시작하자마자 에어백에 공기가 유입되며 전개가 시작되지만, BIRTH를 통해 동작하는 시간을 조정할 수 있음
    (잘 쓰지는 않음. 보통 초기부터 전개되도록 한다고 함)
  - RDH : use reference geometry to compute time step, bag을 접으면 보통 요소 size가 작아지는데, 요소 size가 작아지면 해석시간이 늘어나므로 bag을 접기 전의 요소 size대로 해석시간을 setting
- reference geometry를 사용하려면 MAT_FABRIC의 LNRC =1 로 설정함


---

## CV (Control Volume) Method
- 닫힌 volume이 전체적으로 동일한 압력을 받는다고 가정하고 fabric의 끝단까지 조기에 동일하게 부풀어오름.
- fabric의 geometry는 edge부까지 closed된 volume이어야 함 
- Gas는 요소로 모델링되지 않음
- 해석 진행 후 가스 확인 불가
- Bag porosity, venting, jetting effect는 선택하여 사용 가능함
  *Bag porosity : bag 자체에서 빠져나가는 gas를 조정(미세 구멍, 미세압 조정), venting : 가스가 나가는 구멍(아예 fabric에서 gas가 유출되도록 설계된 구멍), jetting effect : 특정위치에서부터 부풀어오르도록 setting (CV method에서 사용되는 기능이며, 차량 tire 해석에서 많이 사용됨)
- 가스가 어디로 이동하는지 중요하지 않을 때, 에어백의 fabric이 접혀있지 않은 상태일 때 사용함
- **주의사항**
  해당 bag의 normal vector 방향을 고려해야함. fabric이 부풀어올랐다고 가정하고, normal vecotr가 fabric의 접선에 수직이면서 바깥을 향해야만 제대로 해석이 전개가 됨
  단, particle method (corpuscular method) 일 경우에는 반대임. normal vecotr가 fabric의 접선에 수직이면서 bag의 내부를 향해야만 제대로 해석이 전개가 됨


---


### AIRBAG_HYBRID : 가장 많이 사용되는 KEYWORD
- Gas를 혼합하여 정의 가능함
- Allows initially filled takns (tank test)
- Jetting mode는 inflator의 분사 효과를 시뮬레이션 함 (but, 접힌 bag일때는 구현 불가)
- Venting orifice area는 상수 A23, 또는 part id '-A23' 또는 LACA23(DEFINE_CURVE_FUNCTION : 절대압력의 함수로서 vent orifice 영역을 정의하는 하중 곡선 id)으로 정의함
- Porosity leakage coefficient 상수 CP23 또는 시간에 대한 LCP23 (curve) 일 수 있음
  * porosity leakage는 fabric에서만 구현 가능하고 비닐에서는 지정이 안된다는게 강사의 설명임 (추가 확인은 필요함)

*즉, bag에서 나가는 gas는 두 종류 : venting으로 나가는 gas가 있고, fabric 면 자체에서 새어나가는 gas가 있음. venting gas는 매개변수 A23, fabric 면으로 나가는 gas는 매개변수 CP23, LCP23 사용

### AIRBAG_INTERACTION : 서로 연결된 두개의 에어백을 정의함. 여러 백이 연결되어 있을 때 붙어져있는 BAG간의 상호관계를 정의할 때 사용함
- 격벽에 따라 gas가 이동 시 (즉, 두개의 에어백이 있다고 가정했을 때 하나의 에어백에 가스가 충진된 후 격벽을 따라 다음 에어백으로 가스가 이동할 때) 이 keyword로 정의함

### AIRBAG_SIMPLE_PRESSURE_VOLUME : 이것도 많이 사용하는 편임, 압력만 조정 가능함
- 매개변수 CN과 BETA는 수식으로 연결되어 있어서 (*매뉴얼의 수식 참조), 둘 중하나만 넣어도 압력 조정됨
- TIP : 보통은 내가 원하는 압력값보다 조금 더 높게 SETTING 되어야 함. (그에 대한 % 권장값은 없으며, 해석 돌려서 현실과 비교해가면서 조정해야함)


---

## Particle (Corpuscular method, AIRBAG_PARTICLE) Method : 가스 혼합물의 흐름과 유연한 구조와의 상호작용을 시뮬레이션 함
- 특정 위치에서 gas가 유입되는 상황을 고려한 method
- 닫힌 volume이 전체적으로 동일한 압력을 받는다고 가정
- 가스 요소화 가능
- 해석 진행 후 가스 확인 가능 (가스 가시화) : 가스 혼합물은 Maxwell의 kinetic molecular theory를 기초 함, 여러 분자는 하나의 particle로 정의됨
- 가스는 random으로 움직이는 단단한 입자로 모델링 함 (사용자가 gas 입자를 만드는 것은 아니고, 자동으로 만들어서 뿌려짐)
- Particle과 Airbag 사이의 충돌은 압력을 발생시켜 운전자, 탑승자와 상호작용함
  실제 시스템을 적은 수의 particle로 표현하면, 충돌 횟수가 감소하고 결과에 noisy가 발생하므로 적정 수치 이상의 particle을 사용해야함 (default값을 사용하거나, 약간 올려서 사용을 권장함)
- 복잡한 가스와 fabric간 상호 작용은 이 방법을 사용하여 단순화되며 수치적으로 빠르고 안정적임
- 가스가 어디로 이동하는지 중요할 때, 에어백의 fabric이 접힌 상태일 때 사용함


---


### AIRBAG_PARTICLE
- Input very similar to CV (control volume)
- Venting supported
- **Porosity supported (blocked and unblocked)** : gas입자가 부딪힌 지점에 대해서는 porosity를 고려하지않으면 block, 고려하면 unblock 적용
- 3 unit systems currently supported : gas에 대한 공식에는 상수가 많이 들어가기 때문에, airbag에서는 단위계를 한정하는 경우가 있음
- Switching from particle method to CV is allowed : 초반에는 particle method로 진행하다가, 사용자가 지정한 시점에 CV method로 변경 가능
- 2 Step procedure..
  - Extract mass flow rate and temperature from gas guide stations.
  - Use of extracted data as input for individual chambers.

- SID1 : 외부백 정의
- SID2 : 내부백이 있을 경우 내부백 정의
- NP : Number of particles (Default : 200,000), 기본값 그대로 사용해도 되고 조금 늘려서 사용해도 됨, particle 각각을 요소로 인식함
- VISFKG : particle을 visible하게 할것인지에 대한 옵션이며, visible이 기본값임
- TATM : Atmospheric temperature (Default : 293K)
- PATM : Atmospheric pressure (Default : 1ATM)
- NVENT : vent 개수
- TEND : Time when all particles (NP) have entered bag (Default : 1.0e+10), 내가 정한 시간까지만 particle이 들어감. 너무 짧게 설정 시 20만개의 입자가 다 안들어 갈수도 있음
- TSW : CV method로 전환되는 시점 지정
- IAIR : Bag 내부에 초기 gas를 고려할 것인지 여부. 보통은 고려하고 사용하여 초기에 bag안에 gas가 있다고 가정




---


## Airbag Contacts

### Airbag self-contact
- Airbag의 contact은 bag의 빠른 전개 속도와 재료의 유연함으로 어려움이 있음
- Folding된 Airbag은 기술적인 어려움으로 bag의 self-contact 을 권장함

#### CONTACT_AIRBAG_SINGLE_SURFACE : Bag 자체의 컨택을 나타내며, 아래는 권장값임
- SOFT = 2 to turn on segment-based contact : penetration을 서칭할 때 nordal base search가 lsdyna의 기본인데, segment base가 contact에 더 유리해서 SOFT=2로 처음부터 쓰는 사람도 많음
  단, 초기 penetration이 있을때에는 이것이 역할을 못함 
- IGNORE = 2 to ingnore initial penetration (IGNORE =1, 2는 거의 동일) : 초기 penetration이 있을 때 역할함.
  contact을 주면 해석이 시작되자마자 서로 밀어내어 element끼리 불안정해지므로 IGNORE=2를 사용하면 침범하지 않도록 가상의 contact두께를 줄 수 있음, 부품이 서로 떨어지면 가상두께가 없어짐
  가능한 초기 penetration을 geometry상에서 없애는 게 맞고, 그 후에 적용
- SBOPT = 3 takes into consideration segment warpage : 요소가 휘어지는걸 고려할지 안할지에 대한 option이며, 많이 휘어질 경우 contact에 영향을 줄 수 있어서 사용함
- DEPTH = 5 checks both surface penetrations and edge-to-edge penetration









