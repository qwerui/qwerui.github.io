# Git Bash

[]와 내부 내용은 Replace할 것

## 설정
### 이름/이메일 설정
```
git config --global user.name "[user_name]"
git config --global user.email "[user_email]"
```

### 설정 값 확인
```
git config --list
```

### 로컬 저장소 생성
```
git init
```

### 원격 저장소 연결/확인
```
git remote add origin [address]
git remote -v
```

### 로컬 저장소 상태 확인
```
git status
```

### 원격 저장소 복제
```
git clone [address]
```

## 커밋 관련
### 스테이징 등록
```
git add . //전체 파일
git add [filename]
```

### 커밋
```
git commit -m "[commit_message]"
```

### 업로드/다운로드
```
git push [remote] [branch]
git pull [remote] // 병합
git fetch [remote] // 병합 X
```

### 커밋 로그
```
git log --all --oneline
git show [commit_id] //특정 커밋 내역
```

## 커밋 비교
```
git diff
git difftool [commit_id_1] [commit_id_2] //vim 에디터로 비교
```

## 파일의 커밋 이력 확인
```
git blame [filename]
git blame -L [from_line_number], [to_line_number] [filename] //라인 지정
```

## 브랜치
### 브랜치 생성
```
git branch [branch_name]
```

### 브랜치 이동
```
git switch [branch_name]
```

### 브랜치 병합
```
git merge [from_branch]
```

## 실행 취소
### 커밋/스테이징 취소
```
git reset --soft [commit_id] //커밋 취소, 스테이징 유지
git reset --mixed [commit_id] //커밋 취소, 스테이징 취소, 로컬 변경 유지
git reset --hard [commit_id] //커밋 취소, 스테이징 취소, 로컬 변경 취소
git reset [filename] //스테이징 취소
```

### 커밋 되돌리기
```
git revert [commit_id] //commit_id로 되돌리기, 되돌린 이력 존재
```