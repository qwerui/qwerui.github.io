# Addressable

## Unity의 에셋 관리 장단점
1. **Resources**
장점 : 이름으로 간편하게 접근 가능   
단점 : 빌드 크기 및 초기 로딩시간 증가, 파일 이름 변경이 어려움   
2. **Asset Bundle**
장점 : 작은 빌드 크기 및 로딩시간
단점 : 의존성 처리가 까다로워 메모리에 에셋이 중복으로 올라갈 수 있음.
3. **Addressable**
장점 : Asset Bundle보다 의존성 처리가 용이

## Addressable 사용법
### 번들 빌드
![Addressable_Inspector](/images/AddressableInspector.PNG)   
Addressable를 적용하려는 에셋의 Addressable 체크박스에 체크한다.
여기서 이름과 라벨, 그룹을 지정할 수 있다.   

![Addressable_Group](/images/AddressableGroup.PNG)   
Window->Asset Management->Addressables->Groups으로 접근하면 나오는 창이다.   
여기서도 에셋의 관리를 할 수 있으며, 그룹 생성 및 번들 빌드를 실행할 수 있다.   

**Play Mode Script**   
Use Asset Database : 에디터용 모드, 실제 런타임과는 차이가 있을 수 있다. 로직을 테스트하는데 유용   
Simulate Groups : 주소지정 방식에대한 시뮬레이팅만 제공   
Use Existing Build : 릴리즈 환경, 실제 번들을 사용   

**Build**   
New Build : 새로 에셋 번들을 빌드한다.   
Update a Previous Build : 이전 빌드에서 변경된 항목만 따로 에셋 번들을 빌드한다.   
Clear Build Cache : 빌드한 번들을 삭제한다.   

![Addressable_Profile](/images/AddressableProfile.PNG)   
Window->Asset Management->Addressables->Profiles로 접근하면 나오는 창이다.   
여기서 Local과 Remote환경의 경로를 설정할 수 있다. CDN을 통한 다운로드를 확인하기 위해 Cloudflare의 R2를 사용했다.   

![Addressable_Setting](/images/AddressableSetting.PNG)   
Assets/AddressableAssetsData/AssetGroup에 존재하는 Addressable Group의 세팅이다. Build & Load 경로, 압축 방법 등을 설정할 수 있다. 서버에서 데이터를 받아올 것이기 때문에 Remote로 설정해놓았다.   

![Addressable_Build](/images/AddressableBuild.PNG)   
Build가 완료되면 BuildPath에 번들과 함께 카탈로그와 해시가 들어있다. 이것을 클라우드 스토리지에 폴더와 함께 넣는다.

![Addressable_Server](/images/AddressableServer.PNG)

### 번들 로드
**테스트에 사용된 코드**   
```
public class AddressableTest : MonoBehaviour
{
    public Image image;
    public AssetReference assetRef;
    AsyncOperationHandle handle;
    
    public void OnClick_DownloadImage()
    {
        Addressables.GetDownloadSizeAsync(assetRef).Completed += (opSize) =>
        {
            if (opSize.Status == AsyncOperationStatus.Succeeded && opSize.Result > 0)
            {
                Debug.Log(opSize.Result);
                Addressables.DownloadDependenciesAsync(assetRef, true).Completed += (opDownload) =>
                {
                    if (opDownload.Status != AsyncOperationStatus.Succeeded)
                    {
                        return;
                    }
                    Debug.Log("Done");
                };
            }
            else
            {
                Debug.Log("Already Done");
            }
        };
    }

    public void OnClick_LoadImage()
    {
       
        Addressables.LoadAssetAsync<Sprite>(assetRef).Completed +=
        (AsyncOperationHandle<Sprite> obj) =>
        {
            handle = obj;
            image.sprite = obj.Result;
        };

    }

    public void OnClick_UnloadImage()
    {
        Addressables.Release(handle);
        image.sprite = null;
    }
}
```
**메소드 설명**   
GetDownloadSizeAsync : 다운받을 에셋의 파일 크기를 가져온다. 이미 다운로드 되어있다면 0이된다.   
DownloadDependenciesAsync : 에셋을 다운로드 받는다.   
LoadAssetAsync : 에셋을 메모리에 로드한다. 만약 다운로드되어있지 않다면 다운로드 받는다.   
Release : 에셋을 메모리에서 해제한다.   
<br/>
GetDownloadSizeAsync나 LoadAssetAsync등은 매개변수로 Addressable 이름, Addressable 라벨, AssetReference를 사용할 수 있다. 이름이나 라벨은 string을 통해 전달하기 때문에 오타의 위험이 있어, AssetReference를 사용하는 편이 좋아보인다.   
<br/>
![Addressable_Result](/images/AddressableResult.PNG)   
위 코드를 통해 Download 후 Load를 한 결과다.   

### 기타 설명
**Remote 기본 캐시 경로**   
Windows : 사용자/Appdata/LocalLow/Unity/[CompanyName]_[ProjectName]   
Android : /storage/emulated/0/Android/data/com.[CompanyName].[ProjectName]/files/UnityCache/Shared   
IOS : /var/moblie/Containers/Data/Application/[App Guid]/Library/UnityCache/Shared

캐시는 번들파일 형태가 아닌 번들이 풀린형태로 저장된다.   
PC의 경우 프로그램을 삭제할 때 이 캐시는 남아있는 경우가 있다.   