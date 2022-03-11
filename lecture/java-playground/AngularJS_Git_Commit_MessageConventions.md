원글 : [AngularJS Git Commit Message Conventions](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)

# Format of the commit message
```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```
어떤 행이던 100문자를 넘기지 말자. 다양한 깃 툴에서 메시지를 읽기 쉽게 해준다.

## 1. Subject line
subject line은 변경에 대한 간결한 서술
### Allowed \<type>
- feat (feature)
- fix (bug fix)
- docs (documentation)
- style (formatting, missing semi colons, …)
- refactor
- test (when adding missing tests)
- chore (maintain)
>[When to use "chore" as type of commit message?](https://stackoverflow.com/questions/26944762/when-to-use-chore-as-type-of-commit-message)
### Allowed \<scope>
\<scope>는 커밋이 변경된 장소를 구체화하는 무엇이든 될 수 있다.   
$location, $browser, $compile, $rootScope, ngHref, ngClick, ngView, 등...
### \<subject>text
- "change"와 같이 간결한 명령문, "changed"나 "changes"등 사용하지 않기
- 첫 글자 대문자x
- 마지막에 . 붙이지 않기
## 2. Message body
- 명령문 현재형으로 사용
- 지난 행동과 대조하여 변경의 동기를 포함시킨다.
## 3. Message Footer
### Breaking changes
footer에 모든 breaking changes는 언급되어야 한다.  justification and migration notes
```
BREAKING CHANGE: isolate scope bindings definition has changed and the inject option for the directive controller injection was removed.
    
To migrate the code follow the example below:
    
Before:
    
scope: {
    myAttr: 'attribute',
    myBind: 'bind',
    myExpression: 'expression',
    myEval: 'evaluate',
    myAccessor: 'accessor'
}
    
After:

scope: {
    myAttr: '@',
    myBind: '@',
    myExpression: '&',
    // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
    myAccessor: '=' // in directive's template change myAccessor() to myAccessor
}

The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

### Referencing issues
닫힌(해결된?) 버그들은 앞에 "Closes"를 붙여 footer에 나열되어야 한다:
```
Closes #234
```
또는 여러 이슈가 있는 경우:
```
Closes #123, #245, #992
```
[예시들](https://gist.github.com/stephenparish/9941e89d80e2bc58a153#examples)