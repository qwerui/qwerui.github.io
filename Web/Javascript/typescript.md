# Typescript

## 설치 및 설정
```sh
npm install -g typescript

# 노드에서 ts 실행을 위한 환경 설정
npm install -D ts-node typescript
# 필요 타입 설치 react, express 등 다수 존재
npm install -D @type/node

# 타입스크립트 설정파일(tsconfig.json) 생성
npx tsc --init

# 빌드
npm tsc --build

# 빌드 내용 삭제
npm tsc --build --clean
```

## tsconfig.json
```json
// npx tsc --init을 실행하면 설명과 함께 옵션이 제공된다.
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "outDir": "./dist",
    "esModuleInterop": true,               
    "strict": true,  
    "skipLibCheck": true
  },
  "include": ["src/**/*"], // 빌드에 포함할 목록
  "exclude": ["src/test/*"] // 빌드에 제외할 목록
}
```