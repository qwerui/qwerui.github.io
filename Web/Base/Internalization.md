# Internalization

## 개요
- Internalization(I18N) : 소프트웨어가 다양한 지역/언어에 대응되도록 설계
    - 유니코드 활성화
    - 텍스트, 리소스 파일을 소스 코드와 분리
    - 다양한 언어에 대비한 UI 설계
- Localization(L10N) : 국제화된 소프트웨어를 지역/언어에 맞게 수정
    - 서식, 맞춤법, 문법 조정
    - 콘텐츠 문구 조정
    - 문화적 요소 고려
- Globalization(G11N) : I18N + L10N

## I18Next
[공식문서](https://www.i18next.com/)

```html
    <!--간단 예시-->
    <script type="text/javascript" src="https://unpkg.com/i18next/dist/umd/i18next.min.js"></script>
    <div>
        <h1 id="i18n-text"></h1>
    </div>
    <div>
        <select onchange="change(this.value)">
            <option selected value="en">영어</option>
            <option value = "ko">한국어</option>
            <option value = "ja">일본어</option>
        </select>
    </div>
    <script>
        i18next.init({
            lng: 'en', // if you're using a language detector, do not define the lng option
            debug: true,
            resources: {
                en: {
                    translation: {
                        "key": "hello!!!"
                    }
                },
                ko: {
                    translation: {
                        "key": "안녕!!!"
                    }
                },
                ja: {
                    translation: {
                        "key": "こんにちは!!!"
                    }
                }
            }
        });

        function change(value){
            i18next.changeLanguage(value);
            document.getElementById('i18n-text').innerHTML = i18next.t('key');
        }

        document.getElementById('i18n-text').innerHTML = i18next.t('key');
    </script>
```
![i18n예시](/images/i18n.png)