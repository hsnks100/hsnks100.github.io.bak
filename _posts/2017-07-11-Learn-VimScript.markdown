---
layout: post
title: "Learn VimScript"
author: 뻘짓마스터(hsnks100@gmail.com)
date: 2017-07-11 20:24 +0900
tags: vim script plugin
comments: true
---



* table of contents
{:toc}

Vim Script 를 배워봅시다.

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

Vim 은 어떻게 동착할지에 대한 많은 옵션을 가지고 있다.

대부분 옵션들은 on/off 식으로 작동하게 되는데 :set XXX, :set noXXX 식으로 XXX 에 대한 사항을 toggle 할 수 있다.

```
:set number 
:set nonumber
```
둘다 vim 에 쳐보면 알 수 있다.

이미 vim 에 익숙한 사용자라면 잘 알겠지만 postfix 로 붙는 ? 과 ! 에 대해서도 알아둬야 한다.  

```
:set number!
```

on/off 토글하기 위해서는 ! 를 붙인다.

현재 상태를 확인하기 위해서는 

```
:set number?
```

끝에 ? 를 붙인다.


# Basic Mapping

Vim 의 장점중 하나가 키맵핑을 이용해서 동작을 바꿀 수 있다는 점이다. 

```
:map _ x
```
타이핑을 하고 문자 위에서 _ 를 쳐보면 문자가 지워지는 것을 볼 수 있다.

특별한 키를 위해서는 <> 으로 묶어서 표현하기도 한다. 예를 들면 <space>, <C-d> 정도가 될 수 있겠다.

# Modal Mapping

map 을 쓸 때 특정모드에서만 가능하도록 매핑을 할 수 있다. 

```
:imap jj <ESC>j 
```
라고 입력 후 insert 모드에서 jj 를 눌러보자. 

vim 에서는 이러한 모드가 여러가지가 있는데

```
:help :map-modes
```
으로 확인해보자. 

꼭 확인해봐야한다. 이 중 에서 우리가 주로 쓰는 매핑은 사람마다 다르겠지만 대체로 imap, vmap, nmap 이 3가지를 쓴다.

# Recursive Mapping

```
:nmap x dd
:nmap _ x
```

라고 입력후에 어떤 내용이 있는 텍스트 위에서 _ 를 타이핑하면 어떤 일이 일어날까?

vim 은 _ 를 입력받아서 x 를 타이핑하게 하고, 또 vim 은 x 는 dd 니까 최종적으로 dd 를 실행하게 된다.

이러한 방식은 때때로 다른 플러그인과의 충돌을 발생시키고, 원하지 않는 결과를 만든다.

이를 위해서 vim 은 no-remap 을 제공한다.

```
:nnoremap x dd
:nnoremap _ x
```

라고 해보고 아까 했던 행동을 해보자.  

# Leaders
앞에서 정했 듯 

```
:nnoremap x dd 
```
라고 정의하고 x 를 누르면 dd 가 실행되는 것을 알 수 있었다. 하지만 이러한 매핑은 기본동작을 바꾸는 좋지 않은 행동이다.

그래서 
```
:nnnoremap -x dd
```

처럼 - 와 같은 접두사(prefix) 를 붙여서 이러한 동작을 지원해 줄 수 있다. 

하지만 사용자가 이미 -x 와 같은 명령어를 개인적으로 매핑해서 쓴다면 해결할 방법이 없어진다. 

사용자가 직접 찾아서 플러그인의 매핑된 코드를 바꿔야 한다. 

이 때 leader key 를 쓰면 사용자가 매핑한 leader key 기준으로 매핑을 할 수 있다.

```
:let mapleader = ","
:nnoremap <leader>x dd
```

후에 ,x 라고 쳐보자.

이러한 leader key 의 장점은 사용자가 원하는 키로 매핑을 일괄적으로 변경할 수 있다는 점이다.

local 하에서 leader key 를 지정하고 싶다면 
```
:help maplocalleader
```

를 통해 공부하자.




# Load Script

자신이 만드는 스크립트는 앞서 설명했 듯 $MYVIMRC 경로에 저장해놓으면 vim 을 재시작할 때 불러온다고 했다.

그 경로는 :echo $MYVIMRC 를 통해 경로를 알 수 있었다. 

하지만 매번 스크립트를 불러올 때, $MYVIMRC 혹은 ~/.vimrc 를 입력하기엔 고통이 따른다.

우리는 키매핑을 배웠기 때문에 다음과 같이 매핑을 한다.

```
:nnoremap <leader>fed :source $MYVIMRC<cr>
```

자기가 편한 키배열로 세팅을 하면 된다.

그리고 .vimrc 를 편집하고 저장하고, :source % 를 입력하면 스크립트를 다시 불러온다.

(여기서 % 는 현재 파일을 가리키는 특별한 문자)

여기서 :source % 또한 키매핑으로 다음과 같이

```
:nnoremap <leader>feR :source $MYVIMRC<cr>
```

할 수 있다. 하지만 이 방법도 저장하고 매번 타이핑 해야한다.

아직 배우지 않았지만 autocmd 를 이용하여 다음과 같이 설정할 수 있다.

``` 
augroup filetype_vim
  autocmd!
  autocmd BufWritePost *.vimrc,*.vim source $MYVIMRC
augroup END 
```

위 코드를 자신의 $MYVIMRC 에다 넣고 저장하고 테스트 해보라. (처음 저장시 :e % 필요할 수도 있음.)

# A More Complicated Mapping

지금까지 간단한 키매핑을 만들어보았다.

이것만 가지고는 우리가 처음 목표로 삼았던 plugin 제작과는 거리가 멀다고 느껴질 수도 있다.

아주 조금, 복잡한 키매핑을 만들어보자.

우리가 "목표로" 하는 키매핑은 해당커서에 있는 단어에 대괄호 "[]" 로 감싸는 것이다.

어떻게 하면 될까? "일단" 키매핑을 바로 만들지 말고 어떻게 하면 해당 단어를 기계적인 방법으로 [] 로 감쌀지 생각해본다.

일단 나 같은 경우엔 

```
bi"<esc>ww...
```
으로 시도해보았다. 하지만 ww 에서 다음단어의 시작으로 가서 실패했다.

두번 째 시도에선 bi"<esc>ea" 으로 성공했다.

```
<leader>S[
```
으로 매핑을 한다고 하면

```
:nnoremap <leader>S[ bi"<esc>ea"
```

이렇게 하면 된다.


감이 조금 "왔으면" 한다.



# Buffer-Local Options and Mappings

# Auto Commands

# Auto Command Groups

# Operator-Pending Mappings

# More Operator-Pending Mappings

# Status Lines

# Responsible Coding






























