---
title: હાલના પ્રોજેક્ટમાં React ઉમેરો
---

<Intro>

જો તમે તમારા હાલના પ્રોજેક્ટમાં કેટલીક ક્રિયાપ્રતિક્રિયા (interactivity) ઉમેરવા માંગતા હો, તો તમારે તેને React માં ફરીથી લખવાની જરૂર નથી. તમારા હાલના સ્ટેકમાં React ઉમેરો અને ગમે ત્યાં ઇન્ટરેક્ટિવ React કમ્પોનેન્ટ્સ રેન્ડર કરો.

</Intro>

<Note>

**લોકલી ડેવલપમેન્ટ માટે તમારે [Node.js](https://nodejs.org/en/) ઇન્સ્ટોલ કરવાની જરૂર છે.** જો કે તમે ઓનલાઇન અથવા તો સાધારણ HTML પેજ પર [React નો પ્રયાસ કરી શકો છો](/learn/installation#try-react), વાસ્તવિક રીતે મોટાભાગના જાવાસ્ક્રિપ્ટ સાધનો જેનો તમે ડેવલપમેન્ટ માટે ઉપયોગ કરવા માંગો છો તે Node.js ની જરૂરિયાત ધરાવશે.

</Note>

## તમારી હાલની વેબસાઇટના સંપૂર્ણ સબરૂટ (subroute) માટે React નો ઉપયોગ કરવો {/*using-react-for-an-entire-subroute-of-your-existing-website*/}

ધારો કે તમારી પાસે `example.com` પર એક વેબ એપ્લિકેશન છે જે બીજી સર્વર ટેક્નોલોજીથી (જેમ કે Rails થી) બનેલું છે, અને તમે `example.com/some-app/` થી શરૂ થતા તમામ રૂટ્સને સંપૂર્ણપણે React માં અમલ કરવા માંગો છો.

તેને સેટઅપ કરવા માટે અમે અહીં ભલામણ કરીએ છીએ:
1. **React-આધારિત ફ્રેમવર્કનો ઉપયોગ કરીને તમારી એપ્લિકેશનનો React ભાગ બનાવો**(/learn/start-a-new-react-project).
2. **તમારા ફ્રેમવર્કના કન્ફિગરેશનમાં `/some-app` ને base path તરીકે સ્પષ્ટ કરો** (અહીં કેવી રીતે કરવું તે છે: [Next.js](https://nextjs.org/docs/app/api-reference/config/next-config-js/basePath), [Gatsby](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting/path-prefix/)).
3. **તમારા સર્વર અથવા પ્રોક્સીને એવી રીતે ગોઠવો** કે જેથી કરીને `/some-app/` હેઠળની તમામ વિનંતીઓ (requests) તમારી React એપ્લિકેશન દ્વારા હેન્ડલ કરવામાં આવે.

આવી ગોઠવણી સુનિશ્ચિત કરે છે કે તમારી એપ્લિકેશનનો React ભાગ તે ફ્રેમવર્કમાં રહેલી [શ્રેષ્ઠ પદ્ધતિઓનો લાભ મેળવી શકે](/learn/creating-a-react-app#full-stack-frameworks).

ઘણા React-આધારિત ફ્રેમવર્ક ફુલ-સ્ટેક હોય છે અને તમારી React એપ્લિકેશનને સર્વરનો લાભ લેવા દે છે. જો કે, જો તમે સર્વર પર જાવાસ્ક્રિપ્ટ ચલાવી શકતા નથી અથવા ચલાવવા માંગતા નથી, તો પણ તમે તે જ અભિગમનો ઉપયોગ કરી શકો છો. તે કિસ્સામાં, તેના બદલે `/some-app/` પર HTML/CSS/JS નો export સર્વ (serve) કરો (Next.js માટે [`next export` output](https://nextjs.org/docs/advanced-features/static-html-export), Gatsby માટે ડિફોલ્ટ).

## તમારા હાલના પેજના એક ભાગ માટે React નો ઉપયોગ કરવો {/*using-react-for-a-part-of-your-existing-page*/}

ધારો કે તમારી પાસે બીજી ટેક્નોલોજીથી બનેલું હાલનું પેજ છે (કાં તો સર્વર જેમ કે Rails અથવા ક્લાયન્ટ જેમ કે Backbone) થી બનેલું, અને તમે તે પેજ પર ક્યાંક ઇન્ટરેક્ટિવ React કમ્પોનેન્ટ્સ રેન્ડર કરવા માંગો છો. React ને એકીકૃત (integrate) કરવાની આ એક સામાન્ય રીત છે -- હકીકતમાં, Meta પર ઘણા વર્ષો સુધી React નો મોટાભાગનો ઉપયોગ આ રીતે જ થતો હતો!

તમે આ બે સ્ટેપમાં કરી શકો છો:

1. **એક જાવાસ્ક્રિપ્ટ એન્વાયર્નમેન્ટ સેટઅપ કરો**  કે જે તમને [JSX સિન્ટેક્સ](/learn/writing-markup-with-jsx) વાપરવા દે, તમારા કોડને [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) / [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) સિન્ટેક્સ સાથે મોડ્યુલોમાં વિભાજીત કરવા દે, અને [`npm`](https://www.npmjs.com/) પેકેજ રજિસ્ટ્રીમાંથી પેકેજો (ઉદાહરણ તરીકે, React) નો ઉપયોગ કરવા દે.
2. **તમારા React કમ્પોનેન્ટ્સને રેન્ડર કરો** એ પેજમાં કે જ્યાં તમે તેમને જોવા માંગો છો.

ચોક્કસ અભિગમ તમારા હાલના પેજ સેટઅપ પર આધાર રાખે છે, તો ચાલો અમુક વિગતો જોઈએ.

### સ્ટેપ 1: મોડ્યુલર જાવાસ્ક્રિપ્ટ એન્વાયર્નમેન્ટ સેટઅપ કરો {/*step-1-set-up-a-modular-javascript-environment*/}

મોડ્યુલર જાવાસ્ક્રિપ્ટ એન્વાયર્નમેન્ટ તમને તમારા React કમ્પોનેન્ટ્સને એક જ ફાઇલમાં બધો કોડ લખવાને બદલે વ્યક્તિગત ફાઇલોમાં લખવા દે છે. તે તમને [`npm`](https://www.npmjs.com) રજિસ્ટ્રી પર અન્ય ડેવલપર્સ દ્વારા પ્રકાશિત અદ્ભુત પેકેજોનો ઉપયોગ કરવાની મંજૂરી પણ આપે છે -- જેમાં React નો પણ સમાવેશ થાય છે! તમે આ કેવી રીતે કરશો તે તમારા હાલના સેટઅપ પર આધારિત છે:

* **જો તમારી એપ્લિકેશન `import` સ્ટેટમેન્ટનો ઉપયોગ કરનારી ફાઇલોમાં પહેલેથી જ વિભાજિત છે,** તો તમારી પાસે જે સેટઅપ છે તેનો જ ઉપયોગ કરવાનો પ્રયાસ કરો. તપાસો કે તમારા JS કોડમાં `<div />` લખવાથી સિન્ટેક્સ એરર આવે છે કે નહીં. જો તે સિન્ટેક્સ એરરનું કારણ બને છે, તો તમારે JSX નો ઉપયોગ કરવા માટે [`Babel સાથે તમારા જાવાસ્ક્રિપ્ટ કોડને ટ્રાન્સફોર્મ કરવાની`](https://babeljs.io/setup) અને [`Babel React preset`](https://babeljs.io/docs/babel-preset-react) સક્ષમ કરવાની જરૂર પડી શકે છે.

* **જો તમારી એપ્લિકેશન પાસે જાવાસ્ક્રિપ્ટ મોડ્યુલો કમ્પાઇલ કરવા માટે હાલનું સેટઅપ નથી,** તો તેને [`Vite`](https://github.com/vitejs/awesome-vite#integrations-with-backends) દ્વારા સેટઅપ કરો. Vite સમુદાય Rails, Django, અને Laravel સહિત બેકએન્ડ ફ્રેમવર્ક સાથે [`ઘણા ઇન્ટિગ્રેશન જાળવે છે`](https://github.com/vitejs/awesome-vite#integrations-with-backends). જો તમારું બેકએન્ડ ફ્રેમવર્ક લિસ્ટમાં નથી, તો તમારા બેકએન્ડ સાથે Vite બિલ્ડ્સને જાતે ઇન્ટિગ્રેટ કરવા માટે [`આ માર્ગદર્શિકાને અનુસરો`](https://vite.dev/guide/backend-integration.html).

તમારું સેટઅપ કામ કરે છે કે નહીં તે તપાસવા માટે, તમારા પ્રોજેક્ટ ફોલ્ડરમાં આ આદેશ (command) ચલાવો:

<TerminalBlock>
npm install react react-dom
</TerminalBlock>

પછી તમારી મુખ્ય જાવાસ્ક્રિપ્ટ ફાઇલની ટોચ પર કોડની આ લાઈનો ઉમેરો (તેને `index.js` અથવા `main.js` કહી શકાય):

<Sandpack>

```html public/index.html hidden
<!DOCTYPE html>
<html>
  <head><title>My app</title></head>
  <body>
    <!-- Your existing page content (in this example, it gets replaced) -->
    <div id="root"></div>
  </body>
</html>
```

```js src/index.js active
import { createRoot } from 'react-dom/client';

// Clear the existing HTML content
document.body.innerHTML = '<div id="app"></div>';

// Render your React component instead
const root = createRoot(document.getElementById('app'));
root.render(<h1>Hello, world</h1>);
```

</Sandpack>

જો તમારા પેજની સમગ્ર સામગ્રી "Hello, world!" દ્વારા બદલાયેલ , તો બધું કામ કરી ગયું! આગળ વાંચતા રહો.

<Note>
હાલના પ્રોજેક્ટમાં પ્રથમ વખત મોડ્યુલર જાવાસ્ક્રિપ્ટ એન્વાયર્નમેન્ટને એકીકૃત કરવું ભયજનક (intimidating) લાગી શકે છે પરંતુ તે યોગ્ય છે! જો તમે અટવાઈ જાઓ, તો અમારા [`સમુદાય સંસાધનો`](/community) અથવા [`Vite Chat`](https://chat.vite.dev/) અજમાવી જુઓ.

</Note>

### સ્ટેપ 2: પેજ પર ગમે ત્યાં React કમ્પોનેન્ટ્સ રેન્ડર કરો {/*step-2-render-react-components-anywhere-on-the-page*/}

અગાઉના સ્ટેપમાં, તમે તમારી મુખ્ય ફાઇલની ટોચ પર આ કોડ મૂક્યો હતો:

```js
import { createRoot } from 'react-dom/client';

// Clear the existing HTML content
document.body.innerHTML = '<div id="app"></div>';

// Render your React component instead
const root = createRoot(document.getElementById('app'));
root.render(<h1>Hello, world</h1>);
```
અલબત્ત, તમે વાસ્તવમાં હાલની HTML સામગ્રીને સાફ (clear) કરવા માંગતા નથી!

આ કોડ કાઢી નાખો.

તેના બદલે, તમે કદાચ તમારા React કમ્પોનેન્ટ્સને તમારા HTML માં ચોક્કસ જગ્યાએ રેન્ડર કરવા માંગશો. તમારું HTML પેજ ખોલો (અથવા સર્વર ટેમ્પલેટ્સ જે તેને જનરેટ કરે છે) અને કોઈપણ ટેગમાં એક યુનિક [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id) એટ્રિબ્યુટ ઉમેરો, ઉદાહરણ તરીકે:

```html
<!-- ... somewhere in your html ... -->
<nav id="navigation"></nav>
<!-- ... more html ... -->
```

આ તમને [`document.getElementById`](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById) દ્વારા  તે HTML એલીમેન્ટને  શોધવા દે છે અને તેને [`createRoot`](/reference/react-dom/client/createRoot) માં પાસ કરવા દે છે જેથી તમે અંદર તમારો પોતાનો React કમ્પોનેન્ટ રેન્ડર કરી શકો:

<Sandpack>

```html public/index.html
<!DOCTYPE html>
<html>
  <head><title>My app</title></head>
  <body>
    <p>This paragraph is a part of HTML.</p>
    <nav id="navigation"></nav>
    <p>This paragraph is also a part of HTML.</p>
  </body>
</html>
```

```js src/index.js active
import { createRoot } from 'react-dom/client';

function NavigationBar() {
  // TODO: Actually implement a navigation bar
  return <h1>Hello from React!</h1>;
}

const domNode = document.getElementById('navigation');
const root = createRoot(domNode);
root.render(<NavigationBar />);
```

</Sandpack>

નોંધ કરો કે `index.html` માંથી મૂળ HTML સામગ્રી કેવી રીતે સચવાય છે, પરંતુ તમારો પોતાનો `NavigationBar` React કમ્પોનેન્ટ હવે તમારા HTML માં `<nav id="navigation">` ની અંદર દેખાય છે. હાલના HTML પેજ ની અંદર React કમ્પોનેન્ટ્સને રેન્ડર કરવા વિશે વધુ જાણવા માટે [`createRoot ઉપયોગ દસ્તાવેજીકરણ`](/reference/react-dom/client/createRoot#rendering-a-page-partially-built-with-react) વાંચો.

જ્યારે તમે હાલના પ્રોજેક્ટમાં React અપનાવો છો, ત્યારે નાના ઇન્ટરેક્ટિવ કમ્પોનેન્ટ્સ (જેમ કે બટનો) થી પ્રારંભ કરવું સામાન્ય છે, અને પછી ધીમે ધીમે ઊંચે ચડવું (moving upwards) જ્યાં સુધી આખરે તમારું આખું પેજ React થી બનેલું હોય. જો તમે ક્યારેય તે બિંદુએ પહોંચો છો, તો અમે React નો સૌથી વધુ લાભ મેળવવા માટે તરત જ [React ફ્રેમવર્ક](/learn/start-a-new-react-project) પર સ્થાનાંતરિત થવાની ભલામણ કરીએ છીએ.

## હાલની નેટિવ મોબાઇલ એપ્લિકેશનમાં React Native નો ઉપયોગ કરવો {/*using-react-native-in-an-existing-native-mobile-app*/}

[React Native](https://reactnative.dev/) ને હાલની નેટિવ એપ્લિકેશન્સમાં પણ ક્રમશઃ (incrementally) એકીકૃત કરી શકાય છે. જો તમારી પાસે Android (Java અથવા Kotlin) અથવા iOS (Objective-C અથવા Swift) માટે હાલની નેટિવ એપ્લિકેશન છે, તો તેમાં React Native સ્ક્રીન ઉમેરવા માટે [આ માર્ગદર્શિકાને અનુસરો](https://reactnative.dev/docs/integration-with-existing-apps).
