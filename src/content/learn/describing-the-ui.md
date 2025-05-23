---
title: UI નું વર્ણન
---

<Intro>

React એ જાવાસ્ક્રિપ્ટ લાઈબ્રેરી છે જે યૂઝર ઈન્ટરફેસ (UI) રેન્ડર કરવા માટે ઉપયોગમાં લેવામાં આવે છે. UI સામાન્ય રીતે નાના ઘટકોમાંથી બનેલું હોય છે જેમ કે બટન, ટેક્સ્ટ અને છબીઓ. React તમને આવા ઘટકોને ફરીથી વાપરી શકાય તેવા અને અંદર અંદર ગોઠવી શકાય એવા *કમ્પોનેન્ટસ* તરીકે બનાવી, જોડવાની સુવિધા આપે છે. વેબસાઇટથી લઈને મોબાઇલ એપ સુધી, સ્ક્રીન પર દેખાતું લગભગ બધું કમ્પોનેન્ટસના રૂપમાં વહેચી શકાય છે. આ અધ્યાયમાં, તમે React કમ્પોનેન્ટસ કેવી રીતે બનાવવા, તેમને કસ્ટમાઇઝ કરવા અને અને જરૂર મુજબ શરતી રીતે કેવી રીતે દર્શાવવા તે શીખશો.

</Intro>

<YouWillLearn isChapter={true}>

* [તમારું પહેલું React કમ્પોનેન્ટ કેવી રીતે લખશો?](/learn/your-first-component)
* [મલ્ટી-કમ્પોનેન્ટ ફાઇલો ક્યારે અને કેવી રીતે બનાવવી?](/learn/importing-and-exporting-components)
* [JSX સાથે જાવાસ્ક્રિપ્ટમાં માર્કઅપ કેવી રીતે જોડવું?](/learn/writing-markup-with-jsx)
* [તમારા કમ્પોનેન્ટ્સમાંથી જાવાસ્ક્રિપ્ટ ફંક્શનાલિટી ઍક્સેસ કરવા માટે JSX સાથે કર્લી બ્રેસિસ કેવી રીતે વાપરશો?](/learn/javascript-in-jsx-with-curly-braces)
* [કમ્પોનેન્ટ્સને props ના માધ્યમથી કઈ રીતે કન્ફિગર કરશો?](/learn/passing-props-to-a-component)
* [કમ્પોનેન્ટ્સને શરતો પ્રમાણે કેવી રીતે રેન્ડર કરશો?](/learn/conditional-rendering)
* [એક સમયે અનેક કમ્પોનેન્ટ્સને કેવી રીતે રેન્ડર કરશો?](/learn/rendering-lists)
* [કમ્પોનેન્ટ્સને શુદ્ધ રાખવાથી ગૂંચવણજનક બગ્સથી કેવી રીતે બચવું?](/learn/keeping-components-pure)
* [તમારા UI ને વૃક્ષ રચના તરીકે સમજવું કેમ ઉપયોગી છે?](/learn/understanding-your-ui-as-a-tree)

</YouWillLearn>

## તમારું પહેલું કમ્પોનેન્ટ {/*your-first-component*/}

React એપ્લિકેશન્સ અલગ-અલગ UI ઘટકોના સમૂહથી બનેલી હોય છે, જેને *કમ્પોનેન્ટસ* કહે છે. React કમ્પોનેન્ટ એ જાવાસ્ક્રિપ્ટ ફંક્શન હોય છે જેમાં તમે Markup ઉમેરી શકો છો. કમ્પોનેન્ટ્સ નાનાં બટન જેટલાં સાદા હોઈ શકે છે અથવા આખા પેજ જેટલાં મોટા પણ હોઈ શકે છે. નીચે આપેલું ઉદાહરણ એક `Gallery` કમ્પોનેન્ટ બતાવે છે, જે ત્રણ `Profile` કમ્પોનેન્ટસ રેન્ડર કરે છે:

<Sandpack>

```js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

<LearnMore path="/learn/your-first-component">

React કમ્પોનેન્ટ્સ કેવી રીતે ડિક્લેર અને ઉપયોગ કરવાં તે શીખવા માટે **[તમારું પહેલું કમ્પોનેન્ટ](/learn/your-first-component)** વાંચો.

</LearnMore>

## કમ્પોનેન્ટ્સને ઇમ્પોર્ટ અને એક્સપોર્ટ કરવા {/*importing-and-exporting-components*/}

તમે એક જ ફાઇલમાં ઘણાં કમ્પોનેન્ટસ ડિક્લેર કરી શકો છો, પરંતુ મોટી ફાઇલોને મેનેજ કરવી મુશ્કેલ બની શકે છે. આ સમસ્યાનો ઉકેલ લાવવા માટે, તમે દરેક કમ્પોનેન્ટને તેની અલગ ફાઇલમાં *એક્સપોર્ટ* કરી શકો છો અને પછી બીજા ફાઇલમાંથી તે કમ્પોનેન્ટને *ઇમ્પોર્ટ* કરી શકો છો:


<Sandpack>

```js src/App.js hidden
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```js src/Gallery.js active
import Profile from './Profile.js';

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```js src/Profile.js
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```

```css
img { margin: 0 10px 10px 0; }
```

</Sandpack>

<LearnMore path="/learn/importing-and-exporting-components">

કમ્પોનેન્ટ્સને પોતાની ફાઇલમાં અલગ કેવી રીતે કરવા તે શીખવા માટે **[કમ્પોનેન્ટ્સને ઇમ્પોર્ટ અને એક્સપોર્ટ કરવા](/learn/importing-and-exporting-components)** વાંચો.

</LearnMore>

## JSX સાથે માર્કઅપ લખવું {/*writing-markup-with-jsx*/}

દરેક React કમ્પોનેન્ટ એ જાવાસ્ક્રિપ્ટ ફંક્શન છે જેમાં કેટલાક માર્કઅપ હોઈ શકે છે, જેને React બ્રાઉઝરમાં રેન્ડર કરે છે. આ માર્કઅપ રજૂ કરવા માટે React કમ્પોનેન્ટ JSX નામના સિન્ટેક્સ એક્સ્ટેંશનનો ઉપયોગ કરે છે. JSX દેખાવમાં HTML જેવું લાગે છે, પરંતુ તે થોડું વધુ કડક હોય છે અને ડાયનામિક માહિતી પણ દર્શાવી શકે છે.

જો આપણે હાલના HTML માર્કઅપને સીધા React કમ્પોનેન્ટમાં પેસ્ટ કરીએ છીએ, તો તે હંમેશાં કામ કરશે નહીં:

<Sandpack>

```js
export default function TodoList() {
  return (
    // This doesn't quite work!
    <h1>Hedy Lamarr's Todos</h1>
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      class="photo"
    >
    <ul>
      <li>Invent new traffic lights
      <li>Rehearse a movie scene
      <li>Improve spectrum technology
    </ul>
  );
}
```

```css
img { height: 90px; }
```

</Sandpack>

જો તમારા પાસે આવું પહેલેથી જ રહેલું HTML છે, તો તમે તેને [કન્વર્ટર](https://transform.tools/html-to-jsx) નો ઉપયોગ કરીને સુધારી શકો છો.:

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
        <li>Improve spectrum technology</li>
      </ul>
    </>
  );
}
```

```css
img { height: 90px; }
```

</Sandpack>

<LearnMore path="/learn/writing-markup-with-jsx">

માન્ય JSX કેવી રીતે લખવું તે શીખવા માટે **[JSX સાથે માર્કઅપ લખવું](/learn/writing-markup-with-jsx)** વાંચો.

</LearnMore>

## Curly Braces સાથે JSX માં જાવાસ્ક્રિપ્ટ નો ઉપયોગ {/*javascript-in-jsx-with-curly-braces*/}

JSX તમને જાવાસ્ક્રિપ્ટ ફાઇલની અંદર HTML જેવા માર્કઅપ લખવાની છૂટ આપે છે, જેથી રેન્ડરિંગ લોજિક અને કન્ટેન્ટ એકસાથે રહે. ઘણીવાર એવું થાય કે તમે માર્કઅપમાં થોડુ જાવાસ્ક્રિપ્ટ લોજિક ઉમેરવા અથવા કોઈ ડાયનામિક પ્રોપર્ટીનો રેફરન્સ આપવા માંગો છો. આવી સ્થિતિમાં, તમે JSX માં curly braces `{}` નો ઉપયોગ કરીને જાવાસ્ક્રિપ્ટ માટે એક "નવો માર્ગ ખોલી" શકો છો:

<Sandpack>

```js
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

<LearnMore path="/learn/javascript-in-jsx-with-curly-braces">

JSX માં જાવાસ્ક્રિપ્ટના ડેટાનો ઉપયોગ કરવાનું શીખવા માટે **[Curly Braces સાથે JSX માં જાવાસ્ક્રિપ્ટ નો ઉપયોગ](/learn/javascript-in-jsx-with-curly-braces)** વાંચો.

</LearnMore>

## કમ્પોનેન્ટમાં props પસાર કરવા {/*passing-props-to-a-component*/}

React માં, કમ્પોનેન્ટસ એકબીજાને માહિતી આપવા માટે props નો ઉપયોગ કરે છે. એક પેરેન્ટ કમ્પોનેન્ટ પોતાનાં child કમ્પોનેન્ટને કંઈક માહિતી આપવા માંગે, ત્યારે તે આ માહિતી props તરીકે મોકલે છે. Props થોડાક HTML એટ્રિબ્યુટ્સ જેવા લાગે છે, પણ તેમાં તમે જાવાસ્ક્રિપ્ટના કોઈપણ વેલ્યુ મોકલી શકો છો—જેમાં ઓબ્જેક્ટસ, arrays, ફંકશનસ, અને JSX નો પણ સમાવેશ થાય છે!

<Sandpack>

```js
import { getImageUrl } from './utils.js'

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.card {
  width: fit-content;
  margin: 5px;
  padding: 5px;
  font-size: 20px;
  text-align: center;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.avatar {
  margin: 20px;
  border-radius: 50%;
}
```

</Sandpack>

<LearnMore path="/learn/passing-props-to-a-component">

Props કેવી રીતે મોકલવા અને કેવી રીતે ઉપયોગ કરવા તે શીખવા માટે **[કમ્પોનેન્ટમાં props પસાર કરવા](/learn/passing-props-to-a-component)** વાંચો.

</LearnMore>

## શરતના આધાર પર JSX બતાવવુ {/*conditional-rendering*/}

ક્યારેક તમારી એપ્લિકેશનમાં એવી જરૂર પડે છે કે તમારા કમ્પોનેન્ટ્સ અલગ-અલગ સ્થિતિઓ પ્રમાણે અલગ-અલગ વસ્તુઓ બતાવે. React માં તમે જાવાસ્ક્રિપ્ટના લોજિક, જેમ કે `if` સ્ટેટમેન્ટ, `&&`, અને `? :` ઓપરેટર્સનો ઉપયોગ કરીને JSXને શરત મુજબ બતાવી શકો છો.

ઉદાહરણ તરીકે, નીચેના કોડમાં `&&` ઓપરેટરનો ઉપયોગ કરીને ચેકમાર્ક માત્ર શરત સાચી હોય ત્યારે જ બતાવવામાં આવે છે:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          isPacked={true}
          name="Space suit"
        />
        <Item
          isPacked={true}
          name="Helmet with a golden leaf"
        />
        <Item
          isPacked={false}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<LearnMore path="/learn/conditional-rendering">

કન્ટેન્ટને શરત મુજબ કેવી રીતે રેન્ડર કરવું તે શીખવા માટે **[શરતના આધાર પર JSX બતાવવુ](/learn/conditional-rendering)** વાંચો.

</LearnMore>

## સૂચિ રેન્ડર કરવી {/*rendering-lists*/}

ઘણીવાર એવું થાય છે કે તમે ડેટાની સૂચિમાંથી બહુ સરખા એવા ઘણા કમ્પોનેન્ટ્સ સ્ક્રીન પર બતાવવા હોય છે. આવી સ્થિતિમાં તમે જાવાસ્ક્રિપ્ટનાં `filter()` અને `map()` ફંક્શન્સનો ઉપયોગ કરીને ડેટાના array ને કમ્પોનેન્ટસના array માં બદલી શકો છો.

જ્યારે તમે array માંથી દરેક આઇટમ રેન્ડર કરો છો, ત્યારે તમારે દરેક array આઇટમ માટે એક `key` આપવી જરૂરી હોય છે. સામાન્ય રીતે,  ડેટાબેઝમાંથી મળતી એક અનન્ય ID ને key તરીકે ઉપયોગ કરવો ઉત્તમ હોય છે. Key દ્વારા React એ યાદ રાખી શકે છે કે દરેક આઇટમ લિસ્ટમાં કઈ જગ્યાએ છે જો લિસ્ટ બદલાય ત્યારે પણ.

<Sandpack>

```js src/App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  return (
    <article>
      <h1>Scientists</h1>
      <ul>{listItems}</ul>
    </article>
  );
}
```

```js src/data.js
export const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
h1 { font-size: 22px; }
h2 { font-size: 20px; }
```

</Sandpack>

<LearnMore path="/learn/rendering-lists">

કમ્પોનેન્ટ્સની સૂચિ કેવી રીતે રેન્ડર કરવી અને યોગ્ય key કેવી રીતે પસંદ કરવી તે શીખવા માટે **[સૂચિ રેન્ડર કરવી](/learn/rendering-lists)** વાંચો.

</LearnMore>

## કમ્પોનેન્ટ્સને શુદ્ધ રાખવા {/*keeping-components-pure*/}

કેટલાક જાવાસ્ક્રિપ્ટ ફંક્શન્સ શુદ્ધ (pure) હોય છે. એક શુદ્ધ ફંક્શનની ખાસિયતો હોય છે:

* **ફક્ત પોતાનું કામ કરે છે.** તે પોતાની બહારના કોઈપણ ઓબ્જેક્ટ કે વેરિએબલમાં કોઈ ફેરફાર કરતું નથી, જે તેના કોલિંગ પહેલા હાજર હતા.
* **એજ ઇનપુટ, એજ આઉટપુટ.** એક સમાન ઇનપુટ માટે, શુદ્ધ ફંક્શન હંમેશાં સમાન પરિણામ આપે છે.

તમારા બધા React કમ્પોનેન્ટ્સને શુદ્ધ ફંક્શન્સ તરીકે લખવાથી, તમે મોટી હોય તેવી અણપેક્ષિત ભૂલો અને અજંપાજનક વર્તનથી બચી શકો છો—ખાસ કરીને જ્યારે પ્રોજેક્ટ સમય સાથે મોટો બનતો જાય છે. નીચે એક ઉદાહરણ છે જેમાં કમ્પોનેન્ટ શુદ્ધ નથી:

<Sandpack>

```js
let guest = 0;

function Cup() {
  // Bad: changing a preexisting variable!
  guest = guest + 1;
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

</Sandpack>

આ કમ્પોનેન્ટને શુદ્ધ બનાવા માટે, તમે પૂર્વમાં હાજર વેરિએબલને બદલીને પ્રોપ પસાર કરી શકો છો:

<Sandpack>

```js
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </>
  );
}
```

</Sandpack>

<LearnMore path="/learn/keeping-components-pure">

કમ્પોનેન્ટ્સને શુદ્ધ અને પુનરાવૃત્તીશીલ ફંક્શન તરીકે લખવા વિશે શીખવા માટે **[કમ્પોનેન્ટ્સને શુદ્ધ રાખવા](/learn/keeping-components-pure)** વાંચો.

</LearnMore>

## તમારા UI ને વૃક્ષ તરીકે સમજવું {/*your-ui-as-a-tree*/}

React કમ્પોનેન્ટ્સ અને મોડ્યૂલ્સ વચ્ચેના સંબંધોને દર્શાવવા માટે વૃક્ષોનો ઉપયોગ કરે છે. 

એક React રેન્ડર વૃક્ષ એ કમ્પોનેન્ટ્સના પેરેન્ટ અને ચાઈલ્ડ સંબંધોને પ્રસ્તુત કરે છે. 

<Diagram name="generic_render_tree" height={250} width={500} alt="A tree graph with five nodes, with each node representing a component. The root node is located at the top the tree graph and is labelled 'Root Component'. It has two arrows extending down to two nodes labelled 'Component A' and 'Component C'. Each of the arrows is labelled with 'renders'. 'Component A' has a single 'renders' arrow to a node labelled 'Component B'. 'Component C' has a single 'renders' arrow to a node labelled 'Component D'.">

એક ઉદાહરણ React રેન્ડર વૃક્ષ.

</Diagram>

વૃક્ષના ટોચ પર, રુટ કમ્પોનેન્ટના નજીક આવેલી કમ્પોનેન્ટ્સને ટોપ-લેવલ કમ્પોનેન્ટ્સ માનવામાં આવે છે. અને જે કમ્પોનેન્ટ્સના ચાઈલ્ડ કમ્પોનેન્ટ્સ નથી, તે લીફ કમ્પોનેન્ટ્સ તરીકે ઓળખાય છે. કમ્પોનેન્ટ્સના આ વિભાજનને ડેટા પ્રવાહ અને રેન્ડરિંગ પ્રદર્શન સમજવામાં ઉપયોગી માનવામાં આવે છે.

જાવાસ્ક્રિપ્ટ મૉડ્યુલ્સ વચ્ચેના સંબંધને મૉડ્યુલ ડિપેન્ડન્સી વૃક્ષ (Module Dependency Tree) તરીકે મૉડેલ કરવું, એ પણ તમારા એપ્લિકેશનને સમજવા માટે એક ઉપયોગી રીત છે.

<Diagram name="generic_dependency_tree" height={250} width={500} alt="પાંચ ગાંઠો સાથેનો એક વૃક્ષ ગ્રાફ. દરેક નોડ જાવાસ્ક્રિપ્ટ મોડ્યુલનું પ્રતિનિધિત્વ કરે છે. ટોપ-મોસ્ટ નોડ 'RootModule.js' લેબલ થયેલ છે. તેમાં ગાંઠો સુધી વિસ્તરેલા ત્રણ તીર છે: 'ModuleA.js', 'ModuleB.js', અને 'ModuleC.js'. દરેક તીરને 'imports' તરીકે લેબલ કરવામાં આવે છે. 'ModuleC.js' નોડમાં એક જ 'imports' તીર હોય છે જે 'ModuleD.js' લેબલવાળા નોડ તરફ નિર્દેશ કરે છે.">

મૉડ્યુલ ડિપેન્ડન્સી વૃક્ષનું ઉદાહરણ.

</Diagram>

ડિપેન્ડન્સી વૃક્ષ (Dependency Tree) એ મોટાભાગના બિલ્ડ ટૂલ્સ દ્વારા ઉપયોગમાં લેવાય છે, જે ક્લાયન્ટ માટે ડાઉનલોડ અને રેન્ડર કરવા માટે તમામ સંબંધિત JavaScript કોડને બંડલ કરે છે. મોટા બંડલ કદને કારણે React એપ્લિકેશન્સમાં યુઝર અનુભવ પર નકારાત્મક અસર પડે છે. મૉડ્યુલ ડિપેન્ડન્સી વૃક્ષને સમજવાથી આવા problemasને ડિબગ કરવામાં મદદ મળે છે.

<LearnMore path="/learn/understanding-your-ui-as-a-tree">

**[તમારા UI ને વૃક્ષ તરીકે સમજવું](/learn/understanding-your-ui-as-a-tree)** વાંચો, જેથી તમે React એપ્લિકેશન માટે રેન્ડર અને મૉડ્યુલ ડિપેન્ડન્સી વૃક્ષો કેવી રીતે બનાવી શકો અને તે કેવી રીતે યુઝર અનુભવ અને પ્રદર્શન સુધારવા માટે લાભદાયક માનસિક મોડલ તરીકે કામ કરે છે તે શીખી શકો.

</LearnMore>


## આગળ શું? {/*whats-next*/}

પ્રથમ, [તમારું પહેલું કમ્પોનેન્ટ](/learn/your-first-component) પર જાઓ અને આ અધ્યાયને એક બાદ એક પાનું વાંચો!

અથવા, જો તમે પહેલાથી આ વિષયોથી પરિચિત છો, તો [ઇન્ટરએક્ટિવિટી ઉમેરવી](/learn/adding-interactivity) વિશે વાંચો.
