---
title: Add React to an Existing Project
---

<Intro>

જો તમે તમારા હાલના પ્રોજેક્ટમાં કેટલીક ક્રિયાપ્રતિક્રિયા (interactivity) ઉમેરવા માંગતા હો, તો તમારે તેને React માં ફરીથી લખવાની જરૂર નથી. તમારા હાલના સ્ટેકમાં React ઉમેરો અને ગમે ત્યાં ઇન્ટરેક્ટિવ React કમ્પોનેન્ટ્સ રેન્ડર કરો.

</Intro>

<Note>

**લોકલી ડેવલપમેન્ટ માટે તમારે [Node.js](https://nodejs.org/en/) ઇન્સ્ટોલ કરવાની જરૂર છે.** જો કે તમે ઓનલાઇન અથવા તો સાધારણ HTML પેજ પર [React નો પ્રયાસ કરી શકો છો](/learn/installation#try-react), વાસ્તવિક રીતે મોટાભાગના જાવાસ્ક્રિપ્ટ સાધનો જેનો તમે ડેવલપમેન્ટ માટે ઉપયોગ કરવા માંગો છો તે Node.js ની જરૂરિયાત ધરાવશે.

</Note>

## તમારી હાલની વેબસાઇટના સંપૂર્ણ સબરૂટ (subroute) માટે React નો ઉપયોગ કરવો {/*using-react-for-an-entire-subroute-of-your-existing-website*/}

ધારો કે તમારી પાસે `example.com` પર એક વેબ એપ્લિકેશન છે જે બીજી સર્વર ટેક્નોલોજીથી (જેમ કે Rails થી), બનેલું છે, અને તમે `example.com/some-app/` થી શરૂ થતા તમામ રૂટ્સને સંપૂર્ણપણે React સાથે અમલમાં મૂકવા માંગો છો.

તેને સેટ કરવા માટે અમે અહીં ભલામણ કરીએ છીએ:
1. **React-આધારિત ફ્રેમવર્કનો ઉપયોગ કરીને તમારી એપ્લિકેશનનો React ભાગ બનાવો**(/learn/start-a-new-react-project).
2. **તમારા ફ્રેમવર્કના કન્ફિગરેશનમાં `/some-app` ને base path તરીકે સ્પષ્ટ કરો** (અહીં કેવી રીતે કરવું તે છે: [Next.js](https://nextjs.org/docs/app/api-reference/config/next-config-js/basePath), [Gatsby](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting/path-prefix/)).
3. **તમારા સર્વર અથવા પ્રોક્સીને એવી રીતે ગોઠવો** કે જેથી કરીને `/some-app/` હેઠળની તમામ વિનંતીઓ (requests) તમારી React એપ્લિકેશન દ્વારા હેન્ડલ કરવામાં આવે.

આ સુનિશ્ચિત કરે છે કે તમારી એપ્લિકેશનનો React ભાગ તે ફ્રેમવર્કમાં રહેલી [શ્રેષ્ઠ પદ્ધતિઓનો લાભ મેળવી શકે](/learn/start-a-new-react-project#can-i-use-react-without-a-framework).

ઘણા React-આધારિત ફ્રેમવર્ક ફુલ-સ્ટેક હોય છે અને તમારી React એપ્લિકેશનને સર્વરનો લાભ લેવા દે છે. જો કે, જો તમે સર્વર પર જાવાસ્ક્રિપ્ટ ચલાવી શકતા નથી અથવા ચલાવવા માંગતા નથી, તો પણ તમે તે જ અભિગમનો ઉપયોગ કરી શકો છો. તે કિસ્સામાં, તેના બદલે `/some-app/` પર HTML/CSS/JS નો export ([`next export` output](https://nextjs.org/docs/advanced-features/static-html-export) for Next.js, default for Gatsby) સર્વ (serve) કરો.

## તમારા હાલના પેજના એક ભાગ માટે React નો ઉપયોગ કરવો {/*using-react-for-a-part-of-your-existing-page*/}

ધારો કે તમારી પાસે બીજી ટેક્નોલોજીથી બનેલું હાલનું પેજ છે (કાં તો સર્વર જેમ કે Rails અથવા ક્લાયન્ટ જેમ કે Backbone) થી બનેલું, અને તમે તે પેજ પર ક્યાંક ઇન્ટરેક્ટિવ React કમ્પોનેન્ટ્સ રેન્ડર કરવા માંગો છો. React ને એકીકૃત (integrate) કરવાની આ એક સામાન્ય રીત છે -- હકીકતમાં, Meta પર ઘણા વર્ષો સુધી React નો મોટાભાગનો ઉપયોગ આ રીતે જ થતો હતો!

તમે આ બે સ્ટેપમાં કરી શકો છો:

1. **એક જાવાસ્ક્રિપ્ટ એન્વાયર્નમેન્ટ સેટ કરો** જે તમને JSX સિન્ટેક્સ વાપરવા દે, તમારા કોડને [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) / [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) સિન્ટેક્સ સાથે મોડ્યુલોમાં વિભાજીત કરવા દે, અને [`npm`](https://www.npmjs.com/) પેકેજ રજિસ્ટ્રીમાંથી પેકેજો (ઉદાહરણ તરીકે, React) નો ઉપયોગ કરવા દે.
2. **તમારા React કમ્પોનેન્ટ્સને રેન્ડર કરો** એ પેજમાં કે જ્યાં તમે તેમને જોવા માંગો છો.

ચોક્કસ અભિગમ તમારા હાલના પેજ સેટઅપ પર આધાર રાખે છે, તો ચાલો અમુક વિગતો જોઈએ.

### સ્ટેપ 1: મોડ્યુલર જાવાસ્ક્રિપ્ટ એન્વાયર્નમેન્ટ સેટઅપ કરો {/*step-1-set-up-a-modular-javascript-environment*/}

મોડ્યુલર જાવાસ્ક્રિપ્ટ એન્વાયર્નમેન્ટ તમને તમારા React કમ્પોનેન્ટ્સને એક જ ફાઇલમાં બધો કોડ લખવાને બદલે વ્યક્તિગત ફાઇલોમાં લખવા દે છે. તે તમને [`npm`](https://www.npmjs.com) રજિસ્ટ્રી પર અન્ય ડેવલપર્સ દ્વારા પ્રકાશિત અદ્ભુત પેકેજોનો ઉપયોગ કરવાની મંજૂરી પણ આપે છે -- જેમાં React નો પણ સમાવેશ થાય છે! તમે આ કેવી રીતે કરશો તે તમારા હાલના સેટઅપ પર આધારિત છે:

* **જો તમારી એપ્લિકેશન `import` સ્ટેટમેન્ટનો ઉપયોગ કરનારી ફાઇલોમાં પહેલેથી જ વિભાજિત છે,** તો તમારી પાસે જે સેટઅપ છે તેનો જ ઉપયોગ કરવાનો પ્રયાસ કરો. તપાસો કે તમારા JS કોડમાં `<div />` લખવાથી સિન્ટેક્સ એરર આવે છે કે નહીં. જો તે સિન્ટેક્સ એરરનું કારણ બને છે, તો તમારે JSX નો ઉપયોગ કરવા માટે [`Babel સાથે તમારા JavaScript કોડને ટ્રાન્સફોર્મ કરવાની`](https://babeljs.io/setup) અને [`Babel React preset`](https://babeljs.io/docs/babel-preset-react) સક્ષમ કરવાની જરૂર પડી શકે છે.

* **જો તમારી એપ્લિકેશન પાસે JavaScript મોડ્યુલો કમ્પાઇલ કરવા માટે હાલનું સેટઅપ નથી, તો તેને [`Vite`](https://github.com/vitejs/awesome-vite#integrations-with-backends) દ્વારા સેટઅપ કરો. Vite સમુદાય Rails, Django, અને Laravel સહિત બેકએન્ડ ફ્રેમવર્ક સાથે [`ઘણા ઇન્ટિગ્રેશન જાળવે છે`](https://github.com/vitejs/awesome-vite#integrations-with-backends). જો તમારું બેકએન્ડ ફ્રેમવર્ક લિસ્ટમાં નથી, તો તમારા બેકએન્ડ સાથે Vite બિલ્ડ્સને મેન્યુઅલી ઇન્ટિગ્રેટ કરવા માટે [`આ માર્ગદર્શિકાને અનુસરો`](https://vite.dev/guide/backend-integration.html).

તમારું સેટઅપ કામ કરે છે કે નહીં તે તપાસવા માટે, તમારા પ્રોજેક્ટ ફોલ્ડરમાં આ આદેશ (command) ચલાવો:

<TerminalBlock>
npm install react react-dom
</TerminalBlock>

પછી તમારી મુખ્ય JavaScript ફાઇલની ટોચ પર કોડની આ લાઈનો ઉમેરો (તેને `index.js` અથવા `main.js` કહી શકાય):

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

જો તમારા પેજની સમગ્ર સામગ્રી "Hello, world!" દ્વારા બદલવામાં આવી હતી, તો બધું કામ કરી ગયું! આગળ વાંચતા રહો.

<Note>
હાલના પ્રોજેક્ટમાં પ્રથમ વખત મોડ્યુલર JavaScript એન્વાયર્નમેન્ટને એકીકૃત કરવું ભયજનક (intimidating) લાગી શકે છે પરંતુ તે યોગ્ય છે! જો તમે અટવાઈ જાઓ, તો અમારા [`સમુદાય સંસાધનો`](/community) અથવા [`Vite Chat`](https://chat.vite.dev/) અજમાવી જુઓ.

</Note>

### Step 2: Render React components anywhere on the page {/*step-2-render-react-components-anywhere-on-the-page*/}

In the previous step, you put this code at the top of your main file:

```js
import { createRoot } from 'react-dom/client';

// Clear the existing HTML content
document.body.innerHTML = '<div id="app"></div>';

// Render your React component instead
const root = createRoot(document.getElementById('app'));
root.render(<h1>Hello, world</h1>);
```

Of course, you don't actually want to clear the existing HTML content!

Delete this code.

Instead, you probably want to render your React components in specific places in your HTML. Open your HTML page (or the server templates that generate it) and add a unique [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id) attribute to any tag, for example:

```html
<!-- ... somewhere in your html ... -->
<nav id="navigation"></nav>
<!-- ... more html ... -->
```

This lets you find that HTML element with [`document.getElementById`](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById) and pass it to [`createRoot`](/reference/react-dom/client/createRoot) so that you can render your own React component inside:

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

Notice how the original HTML content from `index.html` is preserved, but your own `NavigationBar` React component now appears inside the `<nav id="navigation">` from your HTML. Read the [`createRoot` usage documentation](/reference/react-dom/client/createRoot#rendering-a-page-partially-built-with-react) to learn more about rendering React components inside an existing HTML page.

When you adopt React in an existing project, it's common to start with small interactive components (like buttons), and then gradually keep "moving upwards" until eventually your entire page is built with React. If you ever reach that point, we recommend migrating to [a React framework](/learn/start-a-new-react-project) right after to get the most out of React.

## Using React Native in an existing native mobile app {/*using-react-native-in-an-existing-native-mobile-app*/}

[React Native](https://reactnative.dev/) can also be integrated into existing native apps incrementally. If you have an existing native app for Android (Java or Kotlin) or iOS (Objective-C or Swift), [follow this guide](https://reactnative.dev/docs/integration-with-existing-apps) to add a React Native screen to it.
