# Bootstrap

## 커스텀 색상 제작
[출처](https://stackoverflow.com/questions/79048994/subtle-and-emphasis-classes-on-bootstrap-5-3-new-color-theme-sass)
```scss
@import "bootstrap/scss/functions";
@import "bootstrap/scss/variables";

$theme-colors: map-merge($theme-colors, ("indigo": $indigo)); // 테마에 색상 추가

@import "bootstrap/scss/maps";
@import "bootstrap/scss/mixins";

$theme-colors-bg-subtle: map-merge($theme-colors-bg-subtle, ("indigo": $indigo-200)); // subtle 색상 추가

$utilities-bg-subtle: map-merge($utilities-bg-subtle, ("indigo-subtle": var(--#{$prefix}indigo-bg-subtle))); // 클래스명 형식 추가

@import "/node_modules/bootstrap/scss/bootstrap";
```