# Route

## 개요
여러개의 페이지로 구성된 어플리케이션을 작성할 때 주소를 알아내서 그 주소에 해당하는 컴포넌트를 렌더링한다. 호출되는 url에 따라 페이지 이동을 설정   
npm install react-router-dom으로 설치해야한다.

## 사용
```javascript
<BrowserRouter>
    <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/contact">Contact</Link>
            </li>
          </ul>
        </nav>

        <Switch>
          <Route path="/" exact component={Home} />
          <Route path="/about" component={About} />
          <Route path="/contact" component={Contact} />
        </Switch>
      </div>
</BrowserRouter>
```