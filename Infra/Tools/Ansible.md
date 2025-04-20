# Ansible

## 개요
시스템 구성 관리, 애플리케이션 배포, 자동화된 워크플로우 설정 등을 할 수 있는 오픈소스 IT 자동화 도구

## 특징
- 에이전트 없음 : ssh 기반 명령 실행
- yaml 기반
- 멱등성 : 같은 작업을 실행해도 결과가 변하지 않음

**실행절차**
- 인벤토리(Inventory): 관리할 서버 목록을 정의합니다.
- 플레이북(Playbook): 실행할 작업 목록을 정의합니다.
- 명령 실행: Ansible이 SSH로 서버에 접속해 플레이북에 따라 작업을 수행합니다.

## 인벤토리

### 정의방법

```

[ipgroup]
192.168.0.1
192.168.0.2
192.168.0.3

// /etc/hosts에서 ip와 경로를 매핑해야한다.
[pathgroup]
web1.example.com
web2.example.com
web3.example.com

// 중첩 그룹, :children 접미사 추가
[mygroup:children]
ipgroup
pathgroup

// [start:end]로 범위 지정
[rangegroup]
192.168.0.[5:10] // 192.168.0.5 ~ 192.168.0.10
user-[a:f].example.com // user-a.example.com ~ user-f.example.com

```

### 조작

```sh
# 인벤토리 확인
ansible-inventory -i inventory_file --list
ansible-inventory -i inventory_file --graph

```

## ansible.cfg
- 환경 설정 파일

```
// 기본 설정
[defaults]
inventory = ./inventory_file // 기본 인벤토리 지정
remote_user = user // 관리 호스트의 사용자
ask_pass = false // SSH 암호를 묻는 메시지 표시

// 권한등에 의해 다른 사용자로 실행해야하는 경우
[privileage_escalation]
become = true // 사용자 변경 활성화
become_method = sudo // 전환 방식
become_user = root // 전환할 사용자
become_ask_pass = false // 암호 묻는 메시지 표시

```

```sh

# ssh 연결 설정
ssh-keygen
ssh-copy-id root@192.168.0.11

# 관리 호스트에 ssh가 실행중인지 확인
systemctl status sshd
# 방화벽 확인
firewall-cmd --permanent --zone=public --add-port=22/tcp
systemctl restart firewalld

```


## 플레이북

### 정의방법

```yml

---
- hosts: all # 작업 대상, 전체, 그룹 등
  force_handlers: yes # 오류 발생 시에도 핸들러 실행
  tasks: # 작업 상세
  - name: Print message # 작업 이름
    debug:
      msg: Hello World # 문자 출력
  - name: Install Nginx
    register: result # 작업 결과 저장 변수
    apt: # ansible.builtin.apt, ansible.builtin 컬렉션일 경우 생략 가능
      name: nginx
      state: present
  - name: Loop task
    debug:
      msg: "{{ item }}" # 하나의 요소에 여러개의 속성이 있을 경우 ['key']로 지정 가능
    loop:
      - one
      - two
      - three
  - name: Condition task
    debug:
      msg: Hello
    when: def is defined # 거짓이기 때문에 실행하지 않음, and/or 키워드 사용가능, 여러 줄로 나눌 경우 and로 연산
  - name: Handler
    service:
      name: "mysql"
      state: restarted
    ignore_errors: true # 에러 무시  
    notify:
      - print msg # 핸들러 호출
  - name: Error Handle
    block: # try
      - name: Error Shell
        shell:
          register: shell_result
          failed_when: "'Error occured' in shell_result.stdout" # 에러 발생 조건 설정
    rescue: # catch
      - name: Execute when error
        debug:
          msg: "Error occured"
    always: # finally
      - name: Execute always
        debug:
          msg: "always execute"
  handlers:
    - name: print msg # 핸들러 정의
      debug:
        msg: "mysql restarted"

```

### 실행방법

```sh

# 실행, --check 옵션으로 문법 오류 체크 가능
ansible-playbook -i inventory_file playbook.yml


```

### 변수

**변수 우선순위**
1. 추가 변수 : 실행할 때 -e 속성으로 지정
2. 플레이 변수 : 플레이북 내에서 선언되는 변수

```yml
---

- hosts: all
  vars:
    myvar: ansible1
  vars_files:
    - myvars.yml

```

3. 호스트 변수 : inventory의 호스트 옆에 정의
4. 그룹 변수 : inventory에 [all:vars]로 정의, all부분은 그룹으로 대체 가능

### vault

Ansible에 사용되는 데이터 암호화

```sh

ansible-vault create secret.yml # 암호화된 yml 생성, --vault-pass-file 속성으로 별도 파일에 암호 저장 가능
ansible-vault view secret.yml # 암호화된 yml 열람, --vault-pass-file 속성으로 별도 파일의 암호 사용 가능
ansible-vault encrypt plain.yml # 기존 파일 암호화
ansible-vault decrypt secret.yml # 기존 파일 복호화
ansible-vault rekey secret.yml # 암호 재설정

ansible-playbook --vault-id @prompt secret.yml # 암호화된 플레이북 실행
ansible-playbook --vault-password-file=pasword.yml secret.yml # 파일로 암호 입력해서 실행

```

### 팩트
- 관리 호스트에서 자동으로 검색한 변수
- 사용자 지정 팩트는 /etc/ansible/facts.d에 *.fact로 저장되어있어야한다. 이 경우 조회는 ansible_local을 사용한다.

```yml

---
# 팩트 출력 플레이북

- hosts: all
  gather_facts: no # 팩트 수집 비활성화
  tasks:
  - name: Gather facts
    setup: # 명시적 팩트 수집

  - name: Print all facts
    debug:
      var: ansible_facts # 팩트 사용 시 출력 결과와 연결해서 사용 (ex: ansible_facts.os_family) 

```

```
// 사용자 지정 팩트
[users]
user1 = ansible
user2 = redhat

```

## 롤

Ansible을 재사용하기 쉽게 구조화된 디렉토리

```sh

ansible-galaxy role init my-role # 초기화

```

### 디렉토리 구조
- defaults: 롤이 사용될 때 덮어쓸 수 있는 롤 변수의 기본값
- files: 정적 파일
- handlers: 핸들러
- meta: 작성자, 라이센스 등 롤에대한 정보
- tasks: 작업
- templates: Jinja2 템플릿
- tests: 롤을 테스트할 때 사용할 인벤토리와 플레이북
- vars: 변수

### 사용법

```yml

- hosts: all

  pre_tasks:
  - name: Before
    debug:
      msg: "Execute before"

  roles:
  - my-role # roles 블록

  tasks:
    - name: import in task
      import_role:
        name: my-role # task에서 import


  post_tasks:
    - name: After
    debug:
      msg: "Execute after"

```