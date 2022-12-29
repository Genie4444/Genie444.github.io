---
layout: single
title:  "Worshop으로 Class 복습(초안)"
---
### 1일 1커밋을 위해 내마음대로 해석 및 공부한 자료, 지속적으로 수정할 예정

```python
from abc import abstractmethod, ABCMeta

class IChange(metaclass = ABCMeta):

    def charge(self, time):
            pass
    #충전을 통해 배터리가 증가(분단위로 입력)
    #분당 충전량 언급x, 표현하기 애매함
    #상속받을 Ltab, Otab에서 충전에 대해 재정의함 -> pass

class Mobile(metaclass = ABCMeta):
    __mobileName = 'NONAME'
    __batterySize = 0
    __osType=0
    #static 변수, 멤버 변수 (self로 호출할 수 있다.)
    #self는 (현재 오브젝트 = 클래스)를 지칭하는 연산자
    #클래스 인스턴스: 클래스를 호출하여 생성된 객체
    #객체 생성? -> 동적 메모리 할당 -> 해당 클래스를 지칭?호명?하기 위해 self사용
    #객체 생성이란건, 클래스의 변수를 동적 메모리에 할당
    #클래스를 호출하는 것은 값(상수, 변수)을 메소드에 적용하여 어떤 결과를 도출하려는 의지
    #일정한 상수 값을 메소드에 적용한다면 정적 메모리 사용, 정적 변수
    #매번 변하는 변수값을 메소드에 적용할때는 지금 입력하는게 메소드에 적용할 변수라는것을 알려줘야함(입력값을 변수 공간에 할당?) 입력값(클래스호출(입력값1 대표이름):)이 변수값(self.멤버변수1=입력값1)이라고 정의해야함
    #or 메소드 사용 전, 매번 새롭게 멤버 변수를 입력할때 정적변수 일일이 타이핑 및 수정귀찮음, 클래스 선언할때 한번에 입력하면 편하다??
    #정적 변수 지정해 놓는거랑, init으로 초기화하는거랑 메모리 사용 차이가 어떻게됨???
    #그래서 stack&static 공간에 있는 정적 변수를 사용할떄와는 다르게 heap(자유)영역에서는 이 클래스의 변수-입력값이라다 라는것을 알려주기위해 self를 사용해야한다.
    #정적 변수, 동적 변수 어떤 경우에 적용?
    #private 내용 추가

    def __init__(self, mobileName, batterySize, osType):
        #__init__은 객체를 생성할 때, 값을 전달하는 생성자
        #__init__이라는 공간에 [(self(클래스, 클래스의 주소?), 자유영역에 할당된 변수 입력값)]을 전달함
        # 현재 상황은 Mobile 클래스의 주소 self, 그리고 클래스를 선언할때 정적변수의 초기값(새롭게 입력할)(mobileName, batterySize, osType)값의 대표 이름?? 저장 공간?
        self.__mobileName = mobileName
        self.__batterySize = batterySize
        self.__osType = osType
        #만약 위에서 정적 변수를 정의하지 않았다면, 이것은 어디 메모리에 저장하는 것인가? 업데이트를 하는게 아니라 새로운 공간에 계속 할당되는 것일까?
        #정적 변수를 사용한 이유는 업데이트를 위해서???

    def operator(self, time):
            pass
    #사용을 통해 배터리가 감소(분단위로 입력)
    #분당 사용량 언급x, 표현하기 애매함
    #상속받을 Ltab, Otab에서 사용에 대해 재정의함 -> pass

    #아래 메소드들은 구현해야하는 클래스 목록에 없음.
    #하지만 멤버변수를 설정 및 출력하는데 사용될 예정
    def setMobileName(self, mobileName):
        self.__mobileName = mobileName
    def setBatterySize(self, betterySize):
        self.__batterySize = betterySize
    def setOsType(self, osType):
        self.__osType = osType

    def getMobileName(self):
        return self.__mobileName

    def getBatterySize(self):
        return self.__batterySize

    def getOsType(self):
        return self.__osType

class Ltab(Mobile, IChange):

    def __init__(self, mobileName, batterySize, osType): #지금은 다중 상속이지만 만약 단일 상속일때, init 설정안하고 선조 클래스 입력 형식으로 집어 넣어도 상관없으려나??? 확인해보기
        #Mobile.__mobileName = mobileName                  ---> #private때문에 __mobileName에 바로 접근 못하는듯???
        Mobile.__init__(self, mobileName, batterySize, osType)
        #super.__init__(mobileName, batterySize, osType) ---> #super 쓰면 self 생략해도 된데, 선조한테 내 주소를 자동으로 알려주나봐

    def operate(self, time): #self를 메소드내에서 사용안해도 지정해야하는가? 이유는?? 주소 알려주는거??
        # Mobile.__batterySize - 10*time               ---> #private때문에 __batterySize에 바로 접근 못하는듯???
        #self.__init__으로 초기화(입력)한 선조 클래스의 batterysize인 __batterysize 변수에 접근할 수 있는 방법이 없음
        Mobile.setBatterySize(self, Mobile.getBatterySize(self) - 10*time) #선조 함수의 정적 변수에 접근하기위해, 선조 함수의 set 메소드를 사용했다. 새로운 입력값은 문제에서 주어진 계산식 적용, setBatterySize(self, newBatterysize)로 쓰면 더 해석하기 좋을듯싶다.
        return Mobile.getBatterySize(self)
        #강사님은 usedAmount = time*10을 할당해서 뺴내었다. 이런 사고방식을 배우자!

    def charge(self, time):
        chargedAmount = time*10
        Mobile.setBatterySize(self, Mobile.getBatterySize(self)+chargedAmount)
        return Mobile.getBatterySize(self)


class Otab(Mobile, IChange):

    def __init__(self,mobileName, batterySize, osType):
        Mobile.__init__(self,mobileName, batterySize, osType)

    def operate(self, time):
        usedAmount = time*12
        Mobile.setBatterySize(self, Mobile.getBatterySize(self)-usedAmount)
        return Mobile.getBatterySize()

    def charge(self, time):
        chargedAmount = time*8
        Mobile.setBatterySize(self, Mobile.getBatterySize(self)+chargedAmount)
        return Mobile.getBatterySize(self)
```
