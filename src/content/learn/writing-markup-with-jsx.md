# JSX સાથે માર્કઅપ લખવું {/*writing-markup-with-jsx*/}

*JSX* એ JavaScript માટેનું syntax extension છે જે તમને JavaScript ફાઇલની અંદર HTML-જેવું માર્કઅપ લખવાની મંજૂરી આપે છે. React components લખવાની અન્ય રીતો પણ છે, પરંતુ મોટાભાગના React developers JSX ની સુવિધાને પસંદ કરે છે, અને મોટાભાગના codebases તેનો ઉપયોગ કરે છે.

<Intro>

તમે શીખશો કે:

* React શા માટે markup અને rendering logic ને મિક્સ કરે છે
* JSX HTML થી કેવી રીતે અલગ છે
* JSX સાથે information કેવી રીતે display કરવી

</Intro>

## JSX: JavaScript માં markup મૂકવું {/*jsx-putting-markup-into-javascript*/}

Web લાંબા સમયથી HTML, CSS અને JavaScript પર બનાવવામાં આવ્યું છે. વર્ષો સુધી, web developers content ને HTML માં, design ને CSS માં, અને logic ને JavaScript માં—ઘણીવાર અલગ ફાઇલોમાં રાખતા હતા! Content HTML ની અંદર markup કરવામાં આવતું હતું જ્યારે page નું logic JavaScript માં અલગથી રહેતું હતું:

<DiagramGroup>

<Diagram name="writing_jsx_html" height={237} width={325} alt="HTML markup with purple background and a div with two child tags: p and form. ">

HTML

</Diagram>

<Diagram name="writing_jsx_js" height={237} width={325} alt="Three JavaScript handlers with yellow background: onSubmit, onLogin, and onClick.">

JavaScript

</Diagram>

</DiagramGroup>

પરંતુ જેમ જેમ Web વધુ interactive બન્યું, તેમ તેમ logic વધુને વધુ content ને નિયંત્રિત કરવા લાગ્યું. JavaScript HTML ના હવાલે હતું! આ કારણથી **React માં, rendering logic અને markup એક જ જગ્યાએ રહે છે—components માં.**

<DiagramGroup>

<Diagram name="writing_jsx_sidebar" height={330} width={325} alt="React component with HTML and JavaScript from previous examples mixed. Function name is Sidebar which calls the function isLoggedIn, highlighted in yellow, followed by conditional logic also highlighted in yellow. Nested inside a div highlighted in purple are the tags from before, replaced by three React components: NavLinks, ChatList, ContactList.">

`Sidebar.js` React component

</Diagram>

<Diagram name="writing_jsx_form" height={330} width={325} alt="React component with HTML and JavaScript from previous examples mixed. Function name is Form highlighted in yellow containing two handlers onClick and onSubmit highlighted in yellow. Following the handlers is HTML highlighted in purple. The HTML contains a form element with a nested input element, both highlighted in purple.">

`Form.js` React component

</Diagram>

</DiagramGroup>

Button ના rendering logic અને markup ને એકસાથે રાખવાથી તેઓ દરેક edit પર એકબીજા સાથે sync માં રહે છે. ઉલટાનું, વિગતો જે સંબંધિત નથી, જેમ કે button નું markup અને sidebar નું markup, એકબીજાથી isolated હોય છે, જેનાથી કોઈપણને બદલવું વધુ સુરક્ષિત બને છે.

દરેક React component એક JavaScript function છે જેમાં થોડું markup હોઈ શકે છે જે React browser માં render કરે છે. React components એક syntax extension વાપરે છે જેને JSX કહેવાય છે markup represent કરવા માટે. JSX HTML જેવું દેખાય છે, પરંતુ તે થોડું વધુ strict છે અને dynamic information display કરી શકે છે.

આ સમજવાનો શ્રેષ્ઠ રસ્તો એ છે કે HTML markup ને JSX markup માં convert કરવો:

## HTML ને JSX માં convert કરવું {/*converting-html-to-jsx*/}

ધારો કે તમારી પાસે આ (સંપૂર્ણ valid) HTML છે:

```html
<h1>Hedy Lamarr's Todos</h1>
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  class="photo"
>
<ul>
    <li>Invent new traffic lights
    <li>Rehearse a movie scene
    <li>Improve the spectrum technology
</ul>
```

અને તમે તેને તમારા component માં મૂકવા માંગો છો:

```js
export default function TodoList() {
  return (
    // ???
  )
}
```

જો તમે તેને copy અને paste કરશો, તો તે કામ કરશે નહીં:

<Sandpack>

```js
export default function TodoList() {
  return (
    // This doesn't work!
    <h1>Hedy Lamarr's Todos</h1>
    <img 
      src="https://i.imgur.com/yXOvdOSs.jpg" 
      alt="Hedy Lamarr" 
      class="photo"
    >
    <ul>
      <li>Invent new traffic lights
      <li>Rehearse a movie scene
      <li>Improve the spectrum technology
    </ul>
  );
}
```

</Sandpack>

આ કારણે કે JSX HTML કરતાં વધુ strict છે અને તેના થોડા વધુ નિયમો છે! જો તમે ઉપરના error messages વાંચશો, તો તેઓ તમને markup ને fix કરવા માટે guide કરશે, અથવા તમે નીચેની guide ને follow કરી શકો છો.

<Note>

મોટાભાગે, React ની on-screen error messages તમને સમસ્યા શોધવામાં મદદ કરશે. જો તમે અટવાઈ જાઓ તો તેને વાંચો!

</Note>

## JSX ના નિયમો {/*the-rules-of-jsx*/}

### 1. Single root element return કરો {/*1-return-a-single-root-element*/}

Component માંથી multiple elements return કરવા માટે, **તેમને single parent tag સાથે wrap કરો.**

ઉદાહરણ તરીકે, તમે `<div>` વાપરી શકો છો:

```js {1,11}
<div>
  <h1>Hedy Lamarr's Todos</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</div>
```

જો તમે તમારા markup માં extra `<div>` ઉમેરવા નથી માંગતા, તો તમે `<>` અને `</>` લખી શકો છો:

```js {1,11}
<>
  <h1>Hedy Lamarr's Todos</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</>
```

આ empty tag ને *[Fragment](/reference/react/Fragment)* કહેવાય છે. Fragments તમને DOM tree માં કોઈ trace છોડ્યા વિના વસ્તુઓને group કરવાની મંજૂરી આપે છે.

<DeepDive>

#### JSX ને single tag ની જરૂર શા માટે છે? {/*why-do-multiple-jsx-tags-need-to-be-wrapped*/}

JSX HTML જેવું લાગે છે, પરંતુ તેની અંદર તે plain JavaScript objects માં transform થાય છે. તમે function માંથી બે objects ને array માં wrap કર્યા વિના return કરી શકતા નથી. આ સમજાવે છે કે તમે બે JSX tags ને પણ બીજા tag અથવા Fragment માં wrap કર્યા વિના return કરી શકતા નથી.

</DeepDive>

### 2. બધા tags બંધ કરો {/*2-close-all-the-tags*/}

JSX માં બધા tags explicitly બંધ કરવાની જરૂર છે: self-closing tags જેમ કે `<img>` એ `<img />` બનવા જોઈએ, અને wrapping tags જેમ કે `<li>oranges` એ `<li>oranges</li>` તરીકે લખવા જોઈએ.

આ રીતે Hedy Lamarr નું image અને list items બંધ દેખાય છે:

```js {2-6,8-10}
<>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
   />
  <ul>
    <li>Invent new traffic lights</li>
    <li>Rehearse a movie scene</li>
    <li>Improve the spectrum technology</li>
  </ul>
</>
```

### 3. camelCase લગભગ બધું! {/*3-camelcase-salls-most-of-the-things*/}

JSX JavaScript માં transform થાય છે અને JSX માં લખાયેલા attributes JavaScript objects ની keys બને છે. તમારા પોતાના components માં, તમે ઘણીવાર આ attributes ને variables માં read કરવા માંગશો. પરંતુ JavaScript માં variable names ની limitations છે. ઉદાહરણ તરીકે, તેઓ dashes સમાવી શકતા નથી અથવા `class` જેવા reserved words હોઈ શકતા નથી.

આ કારણથી, React માં ઘણા HTML અને SVG attributes camelCase માં લખાય છે. ઉદાહરણ તરીકે, `stroke-width` ને બદલે તમે `strokeWidth` વાપરો છો. કારણ કે `class` reserved word છે, React માં તમે તેના બદલે `className` લખો છો, [corresponding DOM property](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) પરથી નામ આપવામાં આવ્યું છે:

```js {4}
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  className="photo"
/>
```

તમે [આ બધા attributes DOM component props reference માં શોધી શકો છો](/reference/react-dom/components/common). જો તમે કોઈ ખોટું કરો છો, તો ચિંતા કરશો નહીં—React browser console માં સુધારાનું સુઝાવ આપે છે.

<Pitfall>

Historical reasons માટે, [`aria-*`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA) અને [`data-*`](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes) attributes HTML માં હોય તેવા dashes સાથે લખાય છે.

</Pitfall>

### Pro-tip: JSX Converter વાપરો {/*pro-tip-use-a-jsx-converter*/}

Existing markup માંના આ બધા attributes ને convert કરવું કંટાળાજનક હોઈ શકે છે! અમે existing HTML અને SVG ને JSX માં translate કરવા માટે [converter](https://transform.tools/html-to-jsx) વાપરવાની ભલામણ કરીએ છીએ. Converters practice માં ખૂબ જ ઉપયોગી છે, પરંતુ તમે JSX પર તમારી જાતે comfortably લખી શકો તે માટે શું થઈ રહ્યું છે તે સમજવું હજુ પણ મહત્વપૂર્ણ છે.

આ તમારું અંતિમ પરિણામ છે:

<Sandpack>

```js
export default function TodoList() {
  return (
    <>
      <h1>Hedy Lamarr's Todos</h1>
      <img 
        src="https://i.imgur.com/yXOvdOSs.jpg" 
        alt="Hedy Lamarr" 
        className="photo" 
      />
      <ul>
        <li>Invent new traffic lights</li>
        <li>Rehearse a movie scene</li>
        <li>Improve the spectrum technology</li>
      </ul>
    </>
  );
}
```

```css
img { height: 90px }
```

</Sandpack>

<Recap>

હવે તમે જાણો છો કે JSX શા માટે અસતિત્વ ધરાવે છે અને components માં તેનો ઉપયોગ કેવી રીતે કરવો:

* React components rendering logic ને markup સાથે group કરે છે કારણ કે તેઓ related છે.
* JSX HTML જેવું છે, પરંતુ થોડા differences સાથે. જરૂર હોય તો તમે [converter](https://transform.tools/html-to-jsx) વાપરી શકો છો.
* Error messages ઘણીવાર તમને markup ને fix કરવાની યોગ્ય દિશા આપશે.

</Recap>

<Challenges>

#### HTML ને JSX માં convert કરો {/*convert-some-html-to-jsx*/}

આ HTML એક component માં paste કરવામાં આવ્યું હતું, પરંતુ તે valid JSX નથી. તેને fix કરો:

<Sandpack>

```js
export default function Bio() {
  return (
    <div class="intro">
      <h1>Welcome to my website!</h1>
    </div>
    <p class="summary">
      You can find my thoughts here.
      <br><br>
      <b>And <i>pictures</b></i> of scientists!
    </p>
  );
}
```

</Sandpack>

તમે તેને manually કરવા માંગો છો કે converter વાપરવા માંગો છો તે તમારા પર નિર્ભર છે!

<Solution>

<Sandpack>

```js
export default function Bio() {
  return (
    <div>
      <div className="intro">
        <h1>Welcome to my website!</h1>
      </div>
      <p className="summary">
        You can find my thoughts here.
        <br />
        <br />
        <b>And <i>pictures</i></b> of scientists!
      </p>
    </div>
  );
}
```

</Sandpack>

</Solution>

</Challenges>