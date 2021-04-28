# wonchul's blog

## To make personal blog (w/ jekyll)

지금부터 설명하는 방법이 공식적인 방법인줄은 모르나, 매우 간단하기 때문에 빠르게 개인 사이트를 만드는게 목적이라면 매우 유용하다고 생각한다.


### 0. {github ID}.github.io 라는 repository를 만든다.

자신의 github ID와 동일하게 하여 {github ID}.github.io 라는 이름으로 새로운 repository를 만든다.


### 1. 자신이 원하는 jekyll theme을 다운로드한다.

* [jekyll theme site](http://jekyllthemes.org/)에서 원하는 theme을 찾고 `Download`를 클릭하여 다운로드한다. 
* 다운받은 파일은 0번에서 만든 repository를 `git clone`하여 해당 repository에서 압축을 푼다.


### 2. ruby를 활용하여 다운받은 jekyll site를 사이트에 올린다. 

* Library installation
```
sudo apt-get update
sudo apt-get install ruby ruby-dev make build-essential
```

*  Setting `gem` into local pc system
  
Add the below content into `~/.bashrc`
```
export GEM_HOME=$HOME/gems
export PATH=$HOME/gems/bin:$PATH
```

* jekyll을 더욱 쉽게 쓸 수 있게 해주는 bundler를 다운받는다.
```
gem install jekyll bundler
```

* 압축을 풀면 `Gemfile`이 포함되어 있으며, 이를 활용하여 관련 library를 다운받는다.
```
bundle install
``` 

### 3. 해당 폴더를 사이트에 publish를 할 수 있는 local server역할을 하도록 한다.
```
bundle exec jekyll serve
```

이제부터는 0에서 만든 repository에서 사이트에 올릴 내용들을 수정하고 push하면, 3번의 local pc에서 publish가 되기 때문에 update가 가능하다. 즉, 내용을 수정하고 담고 있는 장소와 실제로 내용을 사이트에 올리는 장소가 나누어져 있는 것이다. 


----------------------------------------------------------------------------------------------------------------
