# Disk

## 파티션 & 마운트
```sh
# 디스크 확인
fdisk -l

# 파티션 생성, 각 단계에서 m으로 도움말 확인 가능
fdisk [디스크 이름]

# 파티션 포맷(파일 시스템 생성)
mkfs -t [파일 시스템] [파티션 장치]

# 마운트
mount [파티션 장치] [디렉토리]

# 언마운트
unmount [파티션 장치]
```

## RAID
```sh
# RAID 생성
mdadm --create [RAID 장치명] --level=[RAID 단계] --raid-devices=[디스크 개수] [...디스크 목록]

# RAID 중지
mdadm --stop [RAID 장치명]

# RAID 상태 확인
mdadm --detail [RAID 장치명]

# RAID 재가동
mdadm --run [RAID 장치명]
```