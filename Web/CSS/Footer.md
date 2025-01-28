# Footer

## Footer 하단 고정
```css
#content-wrapper{
  height: auto;
  min-height: 100%;
  padding-bottom: 300px;
}
footer{
  height: 300px;
  position : relative;
  transform : translateY(-100%);
}
```
```css
/* flex 기반, 어지간해선 이쪽을 사용하자 */
html, body {
  min-height: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  display: flex;
  flex-direction: column;
}

main {
    flex: 1;
}
```