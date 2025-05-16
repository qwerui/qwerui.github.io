# NestJS

## Express vs NestJS

|     | Express | NestJS |
| --- | --- | --- |
| 라우팅 | 함수 추가 혹은 미들웨어 | @Controller |
| 의존성 주입 | 없음 | 제공 |
| 에러 처리 | 직접 처리 | @Catch |
| 테스트 | 관련 도구 설치 | jest 기반 내장 |

## NestJS 흐름
1. 가드
2. 인터셉터
3. 파이프
4. 컨트롤러
5. 서비스
6. 리포지토리

## NestJS 컨벤션
- 파일명은 [모듈명].[컴포넌트명].ts
- 클래스명은 파스칼 케이스
- 동일 디렉토리는 index.ts 사용

## NestJS 디렉토리 간편 설정
nest cli 설치 후 nest new 프로젝트명 실행

## 환경 변수

```js
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
    }),
  ],
})
export class AppModule {}

```

```js

import {ConfigService} from '@nestjs/config';

@Controller()
export class AppController {
  constructor(private readonly configService: ConfigService) {}

  @Get()
  getHello(): string {
    return this.configService.get('MESSAGE');
  }
}

```

## 유효성 검사

### ValidationPipe

class-validator, class-transformer 설치 후 사용

```js

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe())
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();


```

### Guard

```js

import { CanActivate, Injectable } from "@nestjs/common";

@Injectable()
export class LoginGuard implements CanActivate {

    async canActivate(context: ExecutionContext): boolean{
      console.log('인증 과정');
      return true;
    }
    
}

```

```js

import { MyGuard } from './app.guard';

@Controller()
export class AppController {
    constructor(private authService:AuthService){}

    @UseGuards(MyGuard) // Gurad 적용
    @Get('secret')
    testGuard(){
        return '데이터';
    }
}

```

## 도움되는 모듈
- passport : 인증 관련 기능