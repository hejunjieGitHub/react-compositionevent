# Demo

[https://eyesofkids.github.io/](https://eyesofkids.github.io/)

![demo for japanese input method](https://eyesofkids.github.io/demo.gif)

- input = pure onChange events
- inner = onChange + onComposition events corrections

## What is `Composition Events` problem?

[Composition Events](https://w3c.github.io/uievents/#events-compositionevents) will be fired when users type some text from IME. Developers can use these events to help `change` Event to capture the input text correctly. The `change` event occurs when a control loses the input focus and its value has been modified since gaining focus.

The main problem is when users type these words from IME and do something like search the database or filter out from some data, sometimes these functions will be unworkable. For example, if users type "ni" during the composition session, maybe it will be one of "你尼泥腻" in Chinese or one of "にニ尼煮" in Japanese. But in this moment, the `change` event also be fired. If the search or filter functions of the application are designed to be invoked when `change` event occured, there maybe something wrong logically. These functions should be invoked after users finished the composition session.

In React, there are three synthetic events - `onCompositionEnd`, `onCompositionStart` and `onCompositionUpdate`. If the input components(`<input...>` and `<textarea.../>`) are "uncontrolled", we can use them to help `onChange` to capture the text correctly. The only different point is Google Chrome change its events sequence after v53. Check [Cinput.js](https://github.com/eyesofkids/react-compositionevent/blob/master/uncontrolled/Cinput.js) and [Ctextarea.js](https://github.com/eyesofkids/react-compositionevent/blob/master/uncontrolled/Ctextarea.js) files.

But if these input components are "controlled", it will be hard to solve the problem.

Because these the `value` of a controlled component is came from `state`. We can't modify `state` directly and the only way to update state is using `this.setState()` to schedule update. But `this.setState()` may be asynchronous.

After test, i found different OS/browsers could have different results. I have written some code to solve it. But i thought it isn't a good solution. It uses the browser detection and two properties of the `state` object. One is for input, another is for internal functions(search, filter...etc). It can't just use one property of the `state` object because i can't stop any `change` events, state need it to update the value of the input element. If i stop some change events during composition session, i would get nothing after typing these words from IME.

Maybe someone could help to test and give me some idea for the solution. The better solution is to revise the events of controlled components in React.

## Infos

### Online Test

[https://eyesofkids.github.io/](https://eyesofkids.github.io/)

### boilerplate

The boilerplate is [here](https://github.com/eyesofkids/react-compositionevent/tree/master/env).

## Test Results

### IME & Words for test

1. Japanese: Stand Hiragana IME, "kimi no na ha" 君の名は
2. Traditional Chinese: Zhuyin IME, "su3 2k7 au/6 y7" 你的名字
3. Simplified Chinese: Pinyin IME, "ni de ming zi" 你的名字

### React Component

- Controlled/uncontrolled input component(<input...>)

### by OS

> i have resolved these problems listed blow, the code is [here](https://github.com/eyesofkids/react-compositionevent/blob/master/controlled/App.js).

#### macOS

- Chrome v55: composition problem
- Safari: composition problem
- Firefox: composition problem
- Opera v42: composition problem

#### Win7

- IE 11: "NO" problem
- Chrome v55: composition problem
- Firefox v50: composition problem

#### Win10

- IE 11: composition problem
- Edge v38:  composition problem
- Chrome v55: composition problem
- Opera v42: composition problem



