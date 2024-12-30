# Navigation

## 탐색 컨트롤
사이트 구조를 표현하거나 이동할 수 있게 해주는 컨트롤   

**목록**   
- SiteMapPath : 현재 페이지의 탐색 경로 출력
- Menu : 메뉴 구현 컨트롤, NavigateUrl 속성으로 이동할 페이지 지정가능 
- TreeView : 트리 구조 데이터 표시

## Web.sitemap
탐색 컨트롤에서 사용되는 경로에 대한 데이터를 정의, siteMap 루트 엘리먼트를 하나 가지고 있고, siteMapNode로 메뉴 항목 하나를 나타낸다. siteMapNode 하위에 siteMapNode를 가질 수 있으며, 이는 하위로 나타낸다. siteMapNode는 대표적인 속성으로 url, title, description을 갖는다.