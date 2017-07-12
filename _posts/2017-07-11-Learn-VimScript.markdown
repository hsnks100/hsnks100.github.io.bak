---
layout: post
title: "Learn VimScript"
author: 뻘짓마스터(hsnks100@gmail.com)
date: 2017-07-11 20:24 +0900
tags: vim script plugin
category: vim
omments: true
---

Vim Script 를 배워봅시다.


* table of contents
{:toc}

# VimScript 
이 페이지는 vim 사용법을 알려주는 페이지는 아니다. 좀 더 vim 고급 사용자로 가기 위해서 vim-script 를 배우는 페이지다.

이 페이지를 읽기전 기본적인 buffer, window, tab, vim operator(dd, x, i) 와, insert/ex/visual mode 등에 대해 익숙한 사용자를 대상으로 한다.

크게 세가지 섹션으로 나누어서 설명할 것이다. 

* 기본적인 key map 을 이용하는 방법

* 프로그래밍 하는 방식을 통해 접근하는 vim script, 변수나 함수, 제어구조등이 포함된다.

* 샘플 plugin 제작을 통해 실제 vim 생태에 기여하는 방법.


# .vimrc

기본적으로 vim 을 시작하게 되면 로드하게 되는 script 파일은 .vimrc 파일이다.

.vimrc 파일은 시스템마다 로드하는 위치와 순서가 다를 수 있다.

본인의 컴퓨터의 환경에 대해서 살펴보고 싶으면 
```
:version
```
해보면 로드하는 vimrc 에 대한 정보를 알 수 있다. 

조금 더 일반적인 내용에 대해서 보고 싶으면 
```
:help vimrc
```

```
	Places for your personal initializations:
		Unix		$HOME/.vimrc or $HOME/.vim/vimrc
		OS/2		$HOME/.vimrc, $HOME/vimfiles/vimrc
				or $VIM/.vimrc (or _vimrc)
		MS-Windows	$HOME/_vimrc, $HOME/vimfiles/vimrc
				or $VIM/_vimrc
		Amiga		s:.vimrc, home:.vimrc, home:vimfiles:vimrc
				or $VIM/.vimrc
```

위와 같이 나온다. 각 시스템에서 어떻게 동작하는지 자세한 설명도 볼 수 있다.

이 처럼 vim 은 뭔가에 대해 궁금할 때 :help something 을 이용하면 원하는 정보를 얻을 수 있다.

vim 에서는 간단히 :e ~/.vimrc 정도만 입력해도 자기 시스템에 맞는 vimrc 를 편집할 수 있다.

앞으로 설명할 스크립트들은 간단히 inline 으로 vim editor 창에 입력해도 되지만, 영구히 저장하고 싶다면 자기 시스템에 맞는 vimrc 파일에 기록하면 된다. 

다음시작 때 부터 vim 은 vimrc 을 읽어들여 실행하게 된다.

```
:echo $MYVIMRC
```
으로도 확인 할 수 있다.


# Echo Message

vim 스크립트를 본격적으로 배우기 전에 기본적인 변수확인이나 디버깅 방법을 알아야 한다.

우리가 지금까지 프로그래밍을 하면서 디버깅을 할 때 로그를 남기면서 한것과 같은 이치다.

vim 에서는 echo 내장 명령어를 통해서 변수값을 간단하게 확인할 수 있다.

```
:echo "hello world"
```

쳐보면 하단에 hello world 가 찍히는것을 볼 수 있다.

하지만 이걸로만은 뭔가 나중에 확인하기엔 부족하다. 지나간 로그에 대해서 보고 싶을 때가 있다.

```
:echo "hello1"
:echom "hello2"
:messages 
```

입력해보고 echo 과 echom 의 차이를 스스로 판단하길 바란다.

좀 더 알고 싶으면 :help echom, :help echo 를 활용하자.

# comment

vim 에서 가독성을 높이기 위해서 주석을 달 수 있다. 

~/.vimrc 
```
" open my vimrc
nnoremap <leader>feR :source ~/.vimrc<CR>
```

" 뒤에 나오는 문자는 전부 무효처리 된다. 


# options






















