# Model-View

※주의 : 코드가 옳지 않을 수 있음

## Model-View-Controller
**Model** : 데이터 처리 담당   
**View** : UI 담당   
**Controller** : 인터페이스 담당   
<br/>

사용자의 입력은 Controller를 통해 처리된다. Controller에서 Model에 데이터 처리를 명령하고, 그 결과를 View에 넘겨 UI에 출력한다.   
View를 통해 들어온 입력에 따라 Controller가 Model을 갱신하고, 그 결과를 다시 View에 출력한다. View가 직접 Model을 가지고 업데이트 하기 때문에 의존성이 높다고한다.

조사 결과 Model의 변경을 View에 반영하는 방법이 갈리는 것 같다.
1. Model에서 Delegate로 Notify
2. Controller를 경유해서 Model을 View에 넘김

### 예시 코드 
```
//Model
public class MVCModel
{
    int _attack;
    public int Attack
    {
        set 
        {
            _attack = value;
            OnAttackChanged?.Invoke(this);
        }
        get => _attack;
    }

    public event Action<MVCModel> OnAttackChanged;
}
```

```
//View
public class MVCView : MonoBehaviour
{
    public Text attackText;

    public void UpdateUI(MVCModel model)
    {
        attackText.text = $"Attack : {model.Attack}";
    }
}
```

```
//Controller
public class MVCController : MonoBehaviour
{
    public MVCView view;
    public MVCModel model;

    private void Start() 
    {
        model = new MVCModel();
        view = FindFirstObjectByType<MVCView>();
        model.OnAttackChanged += (model) => view.UpdateUI(model);
        model.Attack = 0;
    }

    public void OnPress_Upgrade()
    {
        model.Attack++;
        view.UpdateUI(model);
    }
}
```

## Model-View-Presenter
View를 통해 들어온 입력은 Presenter에게 데이터를 요청한다. Presenter는 Model에서 데이터를 받아와서 가공한 뒤 View에 넘겨준다. MVC와 다른 점은 Presenter는 View와 1:1대응이고 보통 View의 인터페이스를 바라본다.

### 코드 예시
```
//Model
public class MVPModel
{
    int _attack;
    public int Attack
    {
        get => _attack;
    }

    public void Upgrade()
    {
        _attack++;
        OnAttackChanged?.Invoke();
    }

    public event Action OnAttackChanged;
}

```

```
//View
public class MVPView : MonoBehaviour, IMVPView
{
    Text text;
    MVPPresenter presenter;

    private void Awake() 
    {
        TryGetComponent(out text);    
    }

    public void UpdateUI(int attack)
    {
        text.text = $"Attack : {attack}";
    }

    public void Init(MVPPresenter presenter)
    {
        this.presenter = presenter;
    }

    public void OnPress_Upgrade()
    {
        presenter.Upgrade();
    }
}

public interface IMVPView
{
    public void Init(MVPPresenter presenter);
    public void UpdateUI(int attack);
}
```

```
//Presenter
public class MVPPresenter : MonoBehaviour
{
    MVPModel model;
    IMVPView view;

    private void Start() 
    {
        model = new MVPModel();
        view = FindFirstObjectByType<MVPView>();
        view.Init(this);
        model.OnAttackChanged += UpdateUI;
        view.UpdateUI(model.Attack);
    }

    public void Upgrade()
    {
        model.Upgrade();
    }

    void UpdateUI()
    {
        view.UpdateUI(model.Attack);
    }
}
```

## Model-View-ViewModel
View와 ViewModel간 데이터 바인딩을 통해 동기화되고(보통 1:1 연결), ViewModel과 Model은 서로 직접 연결되며, ViewModel이 Model을 갱신, Model이 ViewModel에 데이터를 전달한다.

### 코드 예시
```
//Model
public class MVVMModel : MonoBehaviour
{
    public MVVMViewModel vm;

    int _attack;
    public int Attack
    {
        set
        {
            _attack = value;
            vm.Attack = value;
        }
        get => _attack;
    }

    public void Upgrade()
    {
        Attack++;
    }
}
```

```
//View
public class MVVMView : MonoBehaviour
{
    public Text UIText;
    public string Path;
    public string Format;
    public object Value;
    public object Obj;

    private void Awake() 
    {
        TryGetComponent(out UIText);
    }

    private void Start() 
    {
        FindViewModel();
        OnChange();
    }

    void FindViewModel()
    {
        Transform parent = transform.parent;
        MVVMViewModel vm = null;

        while(parent != null && !parent.TryGetComponent(out vm))
        {
            parent = parent.parent;
        }

        if(vm != null)
        {
            vm.Bind(this, Path);
        }
    }

    public object GetValue(object obj, string path)
    {
        var info = obj.GetType().GetProperty(path, BindingFlags.Instance | BindingFlags.NonPublic | BindingFlags.Public | BindingFlags.Static);
        return info.GetValue(obj);
    }

    public void OnChange()
    {
        Value = GetValue(Obj, Path);
        UIText.text = string.Format(Format, Value);
    }
}
```

```
//ViewModel
public class MVVMViewModel : MonoBehaviour
{
    MVVMProperty<int> attack = new MVVMProperty<int>();
    public int Attack
    {
        set=>attack.Value = value;
        get=>attack.Value;
    }

    public void Bind(MVVMView view, string path)
    {
        string propertyName = path.ToLower();
        var field = GetType().GetField(propertyName, BindingFlags.Instance | BindingFlags.NonPublic | BindingFlags.Public | BindingFlags.Static);
        var obj = (MVVMPropertyBase)field.GetValue(this);
        obj.Bind(view);

        view.Obj = this;
        view.Value = view.GetValue(this, path);
    }
}

public interface MVVMPropertyBase
{
    public void Bind(MVVMView view);
}

public class MVVMProperty<T> : MVVMPropertyBase
{
    T _value;

    public T Value
    {
        set
        {
            _value = value;
            Bindings?.Invoke();
        }
        get => _value;
    }
    event Action Bindings;

    public void Bind(MVVMView view)
    {
        Bindings += view.OnChange;
    }
}
```

엄밀하게는 MVVM이 아니다. Path 텍스트를 따라서 데이터를 찾는 것에 가깝다. 간단하게 만들었지만 자동 텍스트 업데이트는 가능하다. 리플렉션과 박싱이 불가피하기 때문에 성능측면에서 단점을 가질 수 있지만, 레퍼런스를 잃어버릴 걱정이 없다. 단순히 컴포넌트에 맞는 스크립트만 추가해주면 되기 때문이다. 대략 작동 순서는 아래와 같다.
1. 레퍼런스가 될 컴포넌트를 찾아 캐싱한다.
2. 상위 오브젝트들 중에서 ViewModel(Context)를 찾는다.
3. Reflection으로 Path에 해당하는 필드의 값와 ViewModel을 찾아 캐싱한다.
4. 찾은 필드에 업데이트 메소드를 델리게이트에 등록해놓는다.
5. Model을 변경할 때 ViewModel도 변경한다.
6. View에서 값을 사용하기 전에 업데이트한다.
<br/>

위 코드에서는 업데이트할 때 다시 Path를 따라서 찾았지만, PropertyInfo나 FieldInfo를 캐싱해서 업데이트하는 편이 좋을 것이다.   
더 자세히 알고싶은 사람은 아래 두 링크를 참고하는 것이 더 이해가 될 것이다.   
[Dev Weeks: 작업 효율을 높이기 위한 유니티 UI 제작 프로그래밍 패턴들](https://www.youtube.com/watch?v=_jW_D2vF9J8&t=5304s)   
[MVVM 4 uGUI (무료 에셋)](https://assetstore.unity.com/packages/tools/gui/mvvm-4-ugui-44793?locale=ko-KR#content)