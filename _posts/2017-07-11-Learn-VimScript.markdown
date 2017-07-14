---
layout: post
title: "Learn VimScript"
author: 뻘짓마스터(hsnks100@gmail.com)
date: 2017-07-11 20:24 +0900
tags: vim script plugin
comments: true
category: vim
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


## key mapping
buffer-local 기준으로 옵션과 키매핑을 지정할 수도 있다.

```
:nnoremap <buffer> - x
```

으로 nnoremap 지정하고, :vs some.txt 후

some.txt buffer 에서 - 로 지워보려고 하면 x 명령어로 작동하지 않을 것이다.

buffer 마다 키매핑을 다르게 할 수 있단 이야기다.

nerdtree 가 설치되어있다면 nerdtree 를 열고 :nnoremap <buffer> 를 타이핑 해보면

nerdtree 에서만 사용되는 키매핑을 볼 수 있다.

## options & leader key

set number 과 같은 일반 옵션도 buffer 기준으로 지정 할 수 있다.

setlocal 을 통해 지정한다.

leader key 도 마찬가지로 

```
:let maplocalleader=","
:nnoremap <localleader> x
```
위 처럼 local 으로 지정할 수 있다.

자세한건

```
:help setlocal
:help maplocalleader
```

참고하면 된다.

## Shadowing

```
:nnoremap <buffer> - x
:nnoremap - i
```

두가지 매핑을 실행하고 - 를 눌렀을 때 어떤 결과가 나올까?

vim 은 buffer 명령어를 우선순위로 잡는다.

일반적인 프로그래밍 방식이랑 일치하며 이치에 맞아보인다.



# Auto Commands

앞서 vimrc 로드를 위해 잠깐 소개했던 auto commands 를 정식으로 소개한다.

``` 
:autocmd BufNewFile * :echom "hi newfile"
:autocmd BufNewFile *.txt :echom "hi textfile"
:e somefile
:e somefile.txt
```

위 명령을 실행하고 :messages 해보자.

autocmd 는 특정한 이벤트가 발생할 때 실행할 명령어를 등록할 수 있다.

이 단문일 경우엔 in-line 으로 명령을 삽입하면 된다. 만약 여러 문장이면 

뒷장에서 소개할 함수를 이용하여 
```
:call function()
```

함수를 호출 할 수도 있다.

```
:autocmd BufNewFile * :write
         ^          ^ ^
         |          | |
         |          | 여기서부터 커맨드
         |          |
         |          대상이 되는 파일의 패턴
         |
         이벤트 종류
```

여기서 이벤트 종류와 파일의 패턴에 대해서 좀 더 알아보고 싶으면

```
:help event
:help autocmd-patterns 
```

을 통해 알아보도록 하자. 



normal 명령어와 같이 써서 일련의 키시퀀스도 전달가능하다.

``` 
:autocmd BufWritePre *.vimrc normal gg=G 
```



## Multiple Events

```
:autocmd BufWritePre *.virmc normal gg=G 
:autocmd BufRead *.virmc normal gg=G 
``` 

위 명령어는 

``` 
:autocmd BufNewFile,BufRead *.html normal gg=G
```

이렇게 하나의 명령어로 합칠 수 있다.


## autocmd FileType

```
:autocmd FileType javascript nnoremap <buffer> _ I//<esc>
:autocmd FileType python     nnoremap <buffer> _ I#<esc> 
```

FileType 에 따라 명령어를 지정할 수도 있다.

## Buffer-local autocommands

```
:au CursorHold <buffer>  echo 'hold'
:au BufNewFile *.txt au CursorHold <buffer>  echo 'hold txtfile'
:au BufNewFile *.js au CursorHold <buffer>  echo 'hold jsfile'
```

autocmd 또한 buffer 단위로 명령을 지정할 수 있다.

위 명령어중 모르는 이벤트가 있을것이다. 지금까지 이 문서를 잘 따라왔다면 모르는 이벤트에 대해 어떻게 정보를 얻어야 하는지 알거라 생각한다.

즉시 실행해보자.

```
:help autocmd-buflocal
```

autocmd-buflocal 에 더 알아보고 싶으면 위 명령어를 통해 더 확인해보자. 

# Auto Command Groups

위에 소개했던 autocmd 는 사실 한가지 문제가 있었다.

이를 알아보기 위해

```
:au BufWrite * :echom "writing buffer."
:w
:messages
```

해보자. 예상대로 writing buffer 를 볼 수 있을 것이다.

여기서 멈추지 말고 한번 더 
```
:au BufWrite * :echom "writing buffer."
```

를 실행하고 
```
:w
```

해보자. messages 창에는 앞에 기록됐던 로그를 포함하여, 로그가 3개가 찍혀있다. 

무슨 일이 일어난걸까?

vim 의 au 는 명령어들이 중첩된다. 

이를 해결하기 위해 autocmd group 이라는게 있다. 

```
:augroup testgroup
:au BufWrite * :echom "writing buffer1 in a group"
:augroup END
:augroup testgroup
:au BufWrite * :echom "writing buffer2 in a group"
:augroup END
:w
```

위 명령어를 실행해보자. 어떤지 살펴보자. 기대했던 결과가 나왔는가?

autocmd group 이 아까와 같은 불상사를 해결할 수 있다고 했는데, 사기 당한 기분이 들 것이다.

그렇다, augroup 조차 명령어가 중첩이 된다.

``` 
:augroup testgroup
:au!
:au BufWrite * :echom "writing buffer in a group"
:augroup END
:w
```

위와 같이 입력을 다시 해보고 로그를 살펴보자.


au! 는 선택된 그룹의 autocmd 명령어를 초기화 시켜준다.

자세한 사항은 

```
:help autocmd-remove
```

자기의 vimrc 에 다음과 같이 augroup 을 등록시켜보자.

```
augroup testgroup
    au!
    au BufWrite * :echom "writing buffer in a group"
augroup END 
```

# Operator-Pending Mappings

# More Operator-Pending Mappings

# Status Lines

# Responsible Coding






























