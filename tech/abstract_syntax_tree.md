# Abstract Syntax Tree (AST)

`Abstract Syntax Tree(AST)`는 `Parse Tree`를 간단하게 바꾼 것.

- `Parse Tree`는 `Concrete Syntax Tree(CST)`라고 하기도 함.
- syntactic 정보를 제외. CST에서 non-terminal node들을 제외한다고 생각할 수도 있음.
- redundant한 부분 제외.

    예: `,`, `()`, `;`

- AST보다 CST가 더 정확하지만, AST만으로도 의미의 파악 가능.

    예: source code에서는 `()`가 중요하지만, tree에서는 `()`가 필요없음.

- AST가 일반적인 `intermediate code representation(IR)`.

## 예: 5 + (2 * 12)

아래의 예에서 syntactic 정보 `exp`와 redundant한 `()`가 AST에는 없음.

- Parse Tree

    ``` text
           exp
     ┌──────┼──────┐
    exp     +     exp
     │             │
     5      ┌──────┼──────┐
            (     exp     )
                   │
            ┌──────┼──────┐
           exp     *     exp
            │             │
            2             12
    ```

- AST

    ``` text
          +
     ┌────┴────┐
     5         *
          ┌────┴────┐
          2         12
    ```

## Compile 과정

parser가 AST를 생성. 중간 단계로 CST를 생성할 수도 있음.

- analysis phase / front-end
  - lexical analysis
    - scanner: source text → lexeme
    - lexer: lexemes → tokens
  - syntax analysis
    - parser: tokens → (parse tree) → AST

- 내가 이해하기로는 token이 class이고, lexeme이 instance임.

## 참고

- [Medium: Leveling Up One’s Parsing Game With ASTs](https://medium.com/basecs/leveling-up-ones-parsing-game-with-asts-d7a6fc2400ff)
- [Esprima](https://esprima.org/) : ECMAScript parsing infrastructure for multipurpose analysis
- [stack overflow: What is the difference between a token and a lexeme?
](https://stackoverflow.com/questions/14954721/what-is-the-difference-between-a-token-and-a-lexeme)

### 기타

- [Wikipedia: Box-drawing character](https://en.wikipedia.org/wiki/Box-drawing_character)
- [Wikipedia: Arrow (symbol)](https://en.wikipedia.org/wiki/Arrow_(symbol))
- [Visual Studio Code: Insert Unicode](https://marketplace.visualstudio.com/items?itemName=brunnerh.insert-unicode) - Visual Studio Code에서 Unicode 입력을 지원
