---
layout:     post
title:      "Python 가상환경 만들기"
subtitle:   "pyenv, virtualenvwrapper 을 이용한"
date:       2015-02-20 20:00:00
author:     "yg"
header-img: ""
---

# Python 의 가상환경

과거 하나의 서버에 여러 버전의 파이썬 버전을 설치해야 할 필요가 있었던 적이 있다. 대표적인 것이 구 버전의 CentOS 에 설치된 파이썬보다 높은 버전의 파이썬이 필요했던 경우가 그렇다. 최근에 django에 대해 이것저것 찾아보다가 파이썬 가상환경을 활용을 많이 한다길래 정리해봤다. 예전에는 잘 몰라서 virtualenv만 가지고 사용했었는데 조금 불편한 감이 있긴 했다.

이번에는 pyenv, virtualenvwrapper 이 두가지를 사용하려고 한다. (pyenv 는 윈도우를 공식적으로 지원하지 않는다. CentOS 6과 OSX에서 정상동작 한 것은 확인했다.)

pyenv 로 python 을 버전별로 설치관리하고, virtualenvwrapper로 새로운 가상환경을 만드는 구성이다.

- pyenv : python 버전별 설치 관리
- virtualenvwrapper : 여러 가상화를 설치

# pyenv 설치
[https://github.com/yyuu/pyenv](https://github.com/yyuu/pyenv)
github 저장소에서 설명하는 과정 그대로 설명한다. 추가 내용을 보고 싶다면 링크로 가서 확인하도록 한다. 설치는 CentOS 에 소스로 설치하려고 한다. 당연한 얘기지만 git 는 필수로 설치하고 있어야 한다. (Mac 이라면 Homebrew로 간단하게 설치할 수 있다!)

> 외부 인터넷 망에 접근이 안되는 환경에서는 사실 사용할 수 없다. 그럴 경우는 실제 python 소스를 받아서 특정 디렉토리에 설치해서 파이썬 버전을 관리해야 한다.

1. pyenv 를 설치하고자 하는 디렉토리를 명시해 주고 저장소를 clone 한다. `$HOME/.pyenv` 에 설치를 권장하지만, 어디든 상관없다.

		$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv

2. 환경변수 `PYENV_ROOT` 와 `PATH`를 추가한다.

		$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
		$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile

3. shims 와 쉘에서의 자동완성 기능을 위해 `pyenv init` 을 쉘에 추가한다.

		$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile


4. 쉘을 재시작한다.

		$ exec $SHELL


5. 새로운 Python 을 설치한다. (`$PYENV_ROOT/versions` 아래 디렉토리에 설치가 된다)

		$ pyenv install 2.7.8


6. 새로운 Python 을 설치하거나 package를 설치하게 되면 아래 명령으로 shim 바이너리 파일들을 리빌드 한다.

		$ pyenv rehash


설치할 수 있는 python 버전리스트를 보고 싶다면 아래 명령으로 확인 할 수 있다.

		$ pyenv install —list


파이썬 버전을 확인하고 싶다면 `pyenv versions` 명령으로 설치된 버전과 현재 버전을 확인 할 수 있다. 또 다른 버전을 사용하려면 `pyenv shell 3.4.2` 처럼 버전을 명시해 주면 된다. 이제 가상화를 하기 위해 virtualenvwrapper 를 설치해 보자.


# virtualenvwrapper 설치
http://virtualenvwrapper.readthedocs.org/en/latest/install.html
역시 정확한 것은 링크 문서를 확인하는게 가장 좋다. 여기에서는 pip 로 간단히 설치한다.

1. pip 를 이용해서 설치

		$ pip install virtualenvwrapper

virtaulenvwrapper 를 설치하면서 virtualenv 로 같이 설치가 된다.

2. 환경변수 설정
PROJECT_HOME 은 설정하지 않아도 된다. .bash_profile 파일에 아래 환경변수를 추가해 준다.

        export WORKON_HOME=$HOME/.virtualenvs
        export PROJECT_HOME=$HOME/Devel
        source /usr/local/bin/virtualenvwrapper.sh


3. 쉘을 다시 시작해야 한다.

		$ exec $SHELL


4. 명령어
- workon : 가상화 환경리스트를 확인하거나 이미 만들어논 가상화 환경으로 변경한다.
- mkvirtualenv : 새로운 파이썬 가상화 환경을 만든다.
- rmvirtualenv : 이미 만들어 놓은 가상화 환경을 삭제한다.
- deactivate : 현재 가상화 환경을 해제한다. (시스템 환경사용)

# 실제 사용 방법
1. python 3.4.2 버전을 새로 설치하고
2. 새로운 temp 이름의 가상화 환경을 만들고 종료한다.
3. 터미널을 새로 시작해서 만들어져 있는 temp 가상화 환경으로 로드 한다.


1. python 3.4.2 버전 새로 설치

		$ pyenv install 3.4.2

2. 새로운 temp 이름의 python 3.4.2 버전 환경을 만들고 종료.

        # pyenv versions 명령으로 현재 python 버전이 3.4.2 인 것을 확인한다.
        # 현재 버전의 python 환경을 만든다.
        $ mkvirtualenv temp
        # 가상화 환경을 비활성화 한다.
        $ deactivate

3. 이미 만들어 놓은 temp 가상화 환경으로 전환한다.

		$ workon temp


# 마치고...
과거에는 python 이 그냥 버전으로만 관리가 된다면 그것으로 만족했었다. 내가 Python을 많이 사용해 보지 않아서 그랬던 것 같다. 요즘은 패키지들을 앱 별로 관리를 해야 하는 경우가 종종 있는데, 이럴때 파이썬 가상 환경을 유용하게 사용하고 있다. 얘를들어 infodesk 라는 django 프로젝트를 할때는 infodesk 라는 가상 환경을 만들어 사용하고, 또 다른 프로젝트를 할때는 다른 이름의 가상환경을 만드는 식으로 사용하고 있다.