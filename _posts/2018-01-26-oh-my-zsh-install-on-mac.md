
Oh My ZSH!


- ``` zsh 설치 확인 ```
```
$ zsh --version
zsh 5.4.2 (x86_64-apple-darwin16.7.0)\
```

- ``` 설치 ```
```
$ brew update
$ brew install zsh
```

- ``` bash 기본 쉘을 zsh로 변경 ``` 
```
$ which zsh             #쉘의 위치 변경
/usr/bin/zsh

$ chsh -s /usr/bin/zsh  #기본 쉘 변경
```

- ``` 터미널 재시작 ```

- ``` 쉘 확인 ```
```
$ echo $SHELL
/usr/bin/zsh
```

- ``` On My Zsh 설치 ```
```
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

