#Objective-C Generics

## 0. Lightweight Generics

Xcode 7 beta 부터 지원하며, Foundation Collection에 적용되어 있다.<br/>
(NSArray, NSMutableArray, NSSet, NSMutableSet, NSOrderedSet, NSMutableOrderedSet, NSDictionary, NSMutableDictionary, NSHashTable, NSMapTable)

## 1. Syntax

Objective-C의 Generics는 다음과 같이 표현할 수 있다.
```
NSArray<NSString*> *strings = [[NSArray<NSString*> alloc] init];
```

## 2. Type check

안타깝게도 Objective-C의 Generics는 타입 매개변수에 대한 검사가 Java나 C#에 비해 엄격하지 못하다.<br/>
만약 ```NSArray<NSString*>``` 배열에 ```NSNumber``` 객체를 담으려고 하면 경고만 뜰 뿐 컴파일이 성공한다. 심지어 정상적으로 동작한다.

#### Why?
    Objective-C Generics의 구현은 id 타입을 이용하기 때문이다. 자세한건 아래 구현 예제를 참고하면 된다.

## 3. Generics 구현

클래스를 정의할 때 다음과 같이 정의할 수 있다.

```
@interface VPStack<__covariant ElemType> : NSObject
-(void)push:(ElemType)element;
-(ElemType)pop;
@end
```

하지만 구현부는 id 타입을 이용해 구현한다.

```
@implementation VPStack
-(instancetype)init {...}
-(void)push:(id)element {...}
-(id)pop {...}
@end
```

이렇게 구현된 클래스는 다음과 같이 사용할 수 있다.

```
VPStack<NSString*>* stack = [[VPStack<NSString*> alloc] init];
[stack push:@"element1"];
[stack push:@1];
```

세 번째 라인처럼 정의된 타입과 다른 타입을 넣을 경우, 다음과 같은 경고가 노출된다.<br/>
하지만 구현이 id 형으로 되어있기 때문에 정상동작한다. 

```
Incompatible pointer types sending ’NSNumber *’ to parameter of type ’NSString *'
```