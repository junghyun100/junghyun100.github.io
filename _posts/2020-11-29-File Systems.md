---
layout: post
title: "운영체제 - File Systems"
tags: [OS]
comments: true

---

OS를 공부하면서 정리를 한 내용들 입니다.

-참고 KOCW 이화여자 대학교 반효경 교수님 : 운영체제 강의

---

# File and File System

## File

`관련 정보를 이름을 가지고 저장하는 것`

일반적으로 파일은 비휘발성의 보조기억장치(ex. 하드디스크 )에 저장을 합니다.

우리가 데이터를 저장하는 목적으로만 사용하는 것이 아니라,

운영체제에서는(ex. Linux 등) 여러가지 장치들도 관리하기 위해 file이라는 

동일한 논리적 단위로 볼 수 있게 합니다.

> 연산 : create, read, write, reposition(lseek), delete, open, close 등

## File attribute(혹은 파일의 metadata)

파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들

* 파일의 이름, 유형, 저장된 위치, 파일 사이즈
* 접근 권한(읽기/쓰기/실행), 시간(생성/변경/사용), 소유자 등

## File System

운영체제에서 파일을 관리하는 부분입니다.

파일 자체의 내용도 관리해야겠지만, 메타데이터, 디렉토리 정보등을 관리합니다.

파일의 저장 방법을 결정하며, 파일 보호 등을 담당합니다.

# Directory and Logical Disk

## Directory

디렉토리 또한 하나의 파일입니다.

디렉토리 하위의 파일들이 어떤 것이 해당하는지에 대한 정보들을 내용으로 하는 파일 입니다.

디렉토리에 속한 파일 이름 및 파일의 attribute들을 저장합니다.

> 연산 : search for a file, create a file, delete a file
> 연산 : list a directory, rename a file, traverse the file system

## Partition(= Logical Disk)

디스크는 논리적인 디스크가 있고, 물리적인 디스크가 있습니다.

운영체제가 보는 디스크는 논리적인 디스크입니다.

이것을 다른 말로는 `Partition`이라고도 합니다.

하나의 물리적인 디스크 안에 여러개의 파티션을 두는 것이 일반적이며,

경우에 따라서는 여러개의 물리저긴 디스크를 하나의 파티션으로 구성하기도 합니다.

물리적 디스크를 파티션으로 구성한 뒤 각각의 파티션에 `file system`을 깔거나 `swapping` 등 다른 용도로 사용할 수 있습니다.

## open()

<img src="https://raw.githubusercontent.com/junghyun100/junghyun100.github.io/master/images/1129/OPen.PNG">

`open()`이라는 것은 파일의 메타데이터를 메모리로 올려놓는 것 입니다.

논리적인 디스크 안에 파일 시스템이 있으면, 특정 파일의 메타데이터가 저장 되어있고,

그 파일의 내용 또한 저장되어 있습니다.

메타 데이터 중에는 파일의 저장 위치도 있기에 해당 파일을 open() 하게 되면

파일의 메타데이터가 메모리로 올라가게 됩니다.

### open("a/b/c")

예를 들어 open("a/b/c")를 하게 되면

디스크로부터 파일 c의 메타데이터를 메모리로 가지고 옵니다.

이를 위하여 directory path를 탐색합니다.

1. root 디렉토리 "/"를 open하고, 그 안에서 파일 "a"의 위치를 획득합니다.
2. root 디렉토리 "a"를 open한 후, read하여 그 안에서 파일 "b"의 위치를 획득합니다.
3. root 디렉토리 "b"를 open한 후, read하여 그 안에서 파일 "c"의 위치를 획득합니다.
4. 파일 "c"를 open 합니다.

이러한 과정에서 search에 너무 많은 시간이 소요 되어 Open을 read/write와 별도로 둡니다.

한번 open한 파일은 read/write 시 directory search가 불필요합니다.

### Open file table

현재 open 된 파일들의 메타데이터 보관소로 메모리 안에 있습니다.

디스크의 메타 데이터보다 Open한 프로세스의 수, 파일 어느 위치에 접근 중인지에 대한 정보가 추가로 들어있습니다.

### File descriptor(file handle file control block)

Open file table에 대한 위치정보를 가집니다.

---
