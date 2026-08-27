---
title: react-dom/test-utils નિવૃત્તિ ચેતવણીઓ
---

## ReactDOMTestUtils.act() ચેતવણી {/*reactdomtestutilsact-warning*/}

`react-dom/test-utils` માંથી `act` ને `react` માંથી `act` ની તરફેણમાં નિવૃત્ત કરવામાં આવ્યું છે.

પહેલાં:

```js
import {act} from 'react-dom/test-utils';
```

પછી:

```js
import {act} from 'react';
```

## બાકીના ReactDOMTestUtils APIs {/*rest-of-reactdomtestutils-apis*/}

`act` સિવાયના તમામ APIs દૂર કરવામાં આવ્યા છે.

React ટીમ આધુનિક અને સારી રીતે સપોર્ટેડ ટેસ્ટિંગ અનુભવ માટે તમારા ટેસ્ટ્સને [@testing-library/react](https://testing-library.com/docs/react-testing-library/intro/) પર સ્થળાંતર કરવાની ભલામણ કરે છે.

### ReactDOMTestUtils.renderIntoDocument {/*reactdomtestutilsrenderintodocument*/}

`renderIntoDocument` ને `@testing-library/react` ના `render` વડે બદલી શકાય છે.

પહેલાં:

```js
import {renderIntoDocument} from 'react-dom/test-utils';

renderIntoDocument(<Component />);
```

પછી:

```js
import {render} from '@testing-library/react';

render(<Component />);
```

### ReactDOMTestUtils.Simulate {/*reactdomtestutilssimulate*/}

`Simulate` ને `@testing-library/react` ના `fireEvent` વડે બદલી શકાય છે.

પહેલાં:

```js
import {Simulate} from 'react-dom/test-utils';

const element = document.querySelector('button');
Simulate.click(element);
```

પછી:

```js
import {fireEvent} from '@testing-library/react';

const element = document.querySelector('button');
fireEvent.click(element);
```

ધ્યાનમાં રાખો કે `fireEvent` એલિમેન્ટ પર એક વાસ્તવિક ઇવેન્ટ ડિસ્પેચ કરે છે અને માત્ર સિન્થેટિકલી ઇવેન્ટ હેન્ડલરને કૉલ કરતું નથી.

### દૂર કરવામાં આવેલા તમામ APIs ની યાદી {/*list-of-all-removed-apis-list-of-all-removed-apis*/}

- `mockComponent()`
- `isElement()`
- `isElementOfType()`
- `isDOMComponent()`
- `isCompositeComponent()`
- `isCompositeComponentWithType()`
- `findAllInRenderedTree()`
- `scryRenderedDOMComponentsWithClass()`
- `findRenderedDOMComponentWithClass()`
- `scryRenderedDOMComponentsWithTag()`
- `findRenderedDOMComponentWithTag()`
- `scryRenderedComponentsWithType()`
- `findRenderedComponentWithType()`
- `renderIntoDocument`
- `Simulate`