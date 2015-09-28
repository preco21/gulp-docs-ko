# 자주 묻는 질문

## 왜 ____대신 gulp를 사용해야 하나요?

[gulp introduction slideshow]를 보시면 gulp를 사용해야 하는 이유와 개요를 확인할 수 있습니다.

## "gulp"라고 불러야하나요 "Gulp"라고 불러야하나요?

예외적인 gulp 로고를 제외하곤 gulp는 언제나 소문자입니다.

## gulp 플러그인은 어디서 찾을 수 있나요?

gulp 플러그인은 언제나 `gulpplugin` 키워드를 포함하고 있습니다.
[gulp 플러그인 찾기][search-gulp-plugins] 또는 [모든 플러그인 보기][npm plugin search]를 통해 찾아볼 수 있습니다.
검색에 자신이 있다면 `gulp`를 포함하는 키워드로 직접 찾아볼 수도 있습니다.

## gulp 플러그인을 만들고 싶습니다. 어디서 시작할 수 있나요?

[Writing a gulp plugin] 위키 페이지를 통해 gulp 플러그인을 만드는 가이드와 예시를 참고할 수 있습니다.

## 내 플러그인을 ____를 수행할 수 있는데, 너무 많은 일을 하나요?

아마도, 스스로에게 물어보세요:

1. 내 플러그인이 하는 일을 다른 플러그인이 사용할 필요가 있는가?
  - 만약 그렇다면 플러그인의 기능을 분리해야합니다. [npm에 해당 플러그인이 이미 있는지 확인하세요][npm plugin search]
2. 내 플러그인이 두 가지 일을 한다. 완전히 다른 옵션을 기반으로 하는가?
  - 만약 그렇다면 커뮤니티에서 두 개의 분리된 플러그인으로 더 좋게 제공할 수 있다.
  - 만약 두 작업이 다르지만 가까운 연관성이 있다면 괜찮다.

## 줄 바꿈은 플러그인 출력에서 어떻게 표현해야 하나요?

언제나 `\n`을 사용하여 여러 OS의 문제를 해결합니다.

## gulp에 대한 업데이트 소식은 어디서 얻을 수 있나요?

gulp 업데이트에 관한 소식은 트위터에서 볼 수 있습니다:

- [@wearefractal](https://twitter.com/wearefractal)
- [@eschoff](https://twitter.com/eschoff)
- [@gulpjs](https://twitter.com/gulpjs)

## gulp는 IRC 채널을 가지고 있나요?

네, [Freenode]의 #gulpjs로 오시면 채팅을 나누실 수 있습니다.

[Writing a gulp plugin]: writing-a-plugin/README.md
[gulp introduction slideshow]: http://slid.es/contra/gulp
[Freenode]: http://freenode.net/
[search-gulp-plugins]: http://gulpjs.com/plugins/
[npm plugin search]: https://npmjs.org/browse/keyword/gulpplugin
