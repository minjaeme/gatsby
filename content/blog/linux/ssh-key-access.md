---
title: ssh key access
date: 2021-07-07 17:07:18
category: linux
thumbnail: { thumbnailSrc }
draft: false
---



클라이언트에서 서버로 비밀번호 없이 ssh에 접속하고 싶다.

# 요약
- 클라이언트에서 개인키 – 공개키를 만든다.
- 클라이언트에서 만든 공개키를 서버에 보낸다.
- 클라이언트에서 개인키로 서버에 보관하고 있는 공개키를 검증하여 접속한다.

# 명령어

## 명령어: 요약
```
(클라이언트)
$ ssh-keygen -t rsa
$ scp id_rsa.pub id@ip:id_rsa.pub

(서버)
$ cat id_rsa.pub >> ~/.ssh/authorized_keys

(클라이언트)
$ ssh id@ip -i rd_rsa
```

## 명령어: 클라이언트(1)
개인키 – 공개키 만들기
- `ssh-keygen`을 통하여 개인키, 공개키를 만든다
```
$ ssh-keygen -t rsa
```
ssh키를 저장할 위치를 지정하는데, ssh키 관리를 위하여 일단 현재 폴더에 저장하자.
- `id_rsa`를 입력하고 엔터
```
Enter file in which to save the key (/<home folder>/.ssh/id_rsa): id_rsa
```
passphrase 는 추가 인증 비밀번호인데, 비밀번호 없이 접속하는게 목적이므로 엔터를 두번 치고, no passphrase를 입력
- 개인키는 `.ssh` 폴더에 넣고, 공개키는 서버로 보내기
`ls -al` 명령어를 사용하여 방금 만든 개인키와 공개키를 확인한다.
```
$ ls -al
(...)
-rw-------  1 <id group>  2602  7월  7 16:03 id_rsa
-rw-r--r--  1 <id group>   568  7월  7 16:03 id_rsa.pub
(...)
```

- `id_rsa(개인키)`는 `600`, `id_rsa.pub(공개키)`는 `644` 권한이 주어졌는지 확인한다.
```
chmod 600 id_rsa
chmod 644 id_rsa.pub
```
- cp 명령어를 사용하여 .ssh 폴더에 만든 개인키를 저장한다

```
$ cp id_rsa ~/.ssh/<원하는 파일명>
```
다른 서버와 구별하기 위하여 `id_rsa_infra`로 설정
- `scp`명령어를 사용하여 공개키를 서버로 보낸다

```
$ scp id_rsa.pub <id>@<ip>:<원하는 경로>
```

이때 `id@ip:`이후에 원하는 경로를 입력하는데 절대경로를 따로 설정하지 않는 경우 home폴더에 저장된다

## 명령어: 서버
공개키를 `authorized_key`에 등록
- `scp`명령어를 사용하여 클라이언트로 받은 공개키를 확인한다.

```
$ cd ~
$ ls -al
(...)
-rw-r--r-- 1 <id group>  568 Jul  7 07:26 id_rsa.pub
(...)
```

- `cat` 명령어를 이용하여, `authorized_key`에 등록하기

```
$ cat id_rsa.pub >> ~/.ssh/authorized_keys
```

`>>`은 `cat`명령어을 결과를 해당 파일에 덧붙여주는 역할을 한다.
`id_rsa.pub`의 내용을 `~/.ssh/authorized_keys`에 추가

## 명령어: 클라이언트(2) 
- ssh 키를 이용하여 접속을 해본다

```
$ ssh id@ip -i rd_rsa
```

`-i` 옵션은 개인키를 사용하여 접속

- (opt) ~/.ssh/config파일을 생성하고 해당 내용을 작성
```
Host <name>
        HostName <ip주소>
        User <id>
        IdentityFile <개인키 경로>
```
- 접속확인
```
ssh <name>
```