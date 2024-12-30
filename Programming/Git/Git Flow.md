# Git Flow

## Git Flow 개요
Git Branch를 관리하는 전략 중 하나, main/develop/feature/release/hotfix로 브랜치를 나누어 관리

## 브랜치 별 역할
main : 실제로 사용자에게 배포되는 프로그램
develop : 개발용 브랜치
feature : 기능 추가, 수정 등 작업영역, develop, feature에서 생성, 작업 완료 시 develop에 병합
release : 출시 전 테스트를 위한 브랜치, develop에서 생성, 작업 완료 시 develop, main에 병합
hotfix : 긴급한 버그 수정을 위한 브랜치, main에서 생성, 작업 완료 시 develop, main에 병합