# Localization

## 사전 준비
1. Project Settings -> Localization -> Create Localization Settings
2. 생성된 Localization Settings에서 Specific Locale Selector에 언어 추가
3. Project Locale Identifier에 기본이될 언어 설정
4. Window -> Asset Management -> Localization Tables에서 테이블 생성

## Localization Table
![Localization_Table](/images/LocalTable.PNG)   
String Table의 화면이다. Asset Table은 문자열 값을 넣는 위치에 오브젝트를  바인딩 할 수 있으며, 맨 처음 넣은 오브젝트의 타입에 따라 다른 언어 환경의 타입이 변화한다.

## Localization 적용
![Localization_Component](/images/LocalComponent.PNG)   
Localization을 적용하는 방법은 Localize [Component] Event를 원하는 오브젝트에 추가하면 된다. 오브젝트의 로컬라이징 대상 컴포넌트에서 우클릭한 뒤 Localize를 추가하면 Event에 직접 컴포넌트를 연결할 필요 없이 바로 연결해준다.   
Event 컴포넌트에 레퍼런스를 우리가 생성해놓았던 테이블로 설정하면 언어 설정에 따라 내용이 달라진다.

## 언어별 속성 변경
![Localization_Change](/images/LocalChange.PNG)   
Window -> Asset Management -> Localization Scene Controls를 활성화한다. Active Locale에서 언어를 선택하고 Track Changes를 체크한 뒤 씬의 오브젝트에 변화를 주면 오브젝트에 GameObject Localizer가 추가되며 변화된 내용이 저장된다.

## Smart
![Localization_Smart](/images/LocalSmart.PNG)
String Table에서 설정할 수 있는 옵션이다. 변수를 출력할 수 있게 해준다. 활성화 한 뒤 Local Variables를 추가한다. 중괄호 안에 Variable Name을 넣어서 변수를 출력할 수 있다. Debug를 통해 연결이 되었는지 검증이 가능하고 Preview를 통해 결과를 미리 볼 수 있다.

## 코드 내부에서 언어 변경
```
IEnumerator Change(int index)
{
    yield return LocalizationSettings.InitializationOperation;
    LocalizationSettings.SelectedLocale = Localization.AvaliableLocales.Locales[index];
}
```

## 코드 내부에서 테이블 접근
```
LocalizationSettings.stringDataBase.GetLocalizedString(TableName, Key, currentLocale);
```

## 외부 파일 연동
테이블 에셋의 Inspector에서 Extension을 볼 수 있다. CSV와 Google Sheet가 가능하며 Save/Open을 통해 Export/Import가 가능하다.