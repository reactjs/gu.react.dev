---
title: Passing Props to a Component
---

<Intro>

React components એકબીજા સાથે વાતચીત (communicate) કરવા માટે *props* નો ઉપયોગ કરે છે. દરેક parent component તેના child components ને props આપીને કેટલીક માહિતી પાસ કરી શકે છે. Props તમને કદાચ HTML attributes ની યાદ અપાવી શકે છે, પરંતુ તમે તેના દ્વારા કોઈપણ JavaScript value પાસ કરી શકો છો, જેમાં objects, arrays અને functions નો પણ સમાવેશ થાય છે.

</Intro>

<YouWillLearn>

* Component ને props કેવી રીતે પાસ કરવા
* Component માંથી props કેવી રીતે રીડ (read) કરવા
* Props માટે default values કેવી રીતે નક્કી કરવી
* Component ને થોડું JSX કેવી રીતે પાસ કરવું
* સમય જતાં props કેવી રીતે બદલાય છે

</YouWillLearn>

## જાણીતા props {/*familiar-props*/}

Props એ એવી માહિતી છે જેને તમે JSX ટેગ (tag) માં પાસ કરો છો. ઉદાહરણ તરીકે, `className`, `src`, `alt`, `width`, અને `height` એ કેટલાક એવા props છે જેને તમે એક `<img>` ટેગમાં પાસ કરી શકો છો:

<Sandpack>

```js
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="લિન લેનિંગ (Lin Lanying)"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return (
    <Avatar />
  );
}
```

```css
body { min-height: 120px; }
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

તમે `<img>` ટેગમાં જે props પાસ કરી શકો છો તે પહેલેથી જ નક્કી કરેલા (predefined) હોય છે (ReactDOM એ [HTML standard](https://www.w3.org/TR/html52/semantics-embedded-content.html#the-img-element) ને અનુસરે છે). પરંતુ તમે તમારા *પોતાના* components (જેમ કે `<Avatar>`) ને કસ્ટમાઇઝ કરવા માટે કોઈપણ props પાસ કરી શકો છો. અહીં જુઓ કેવી રીતે!

## props ને component મા પાસ કરવા {/*passing-props-to-a-component*/}

આ કોડમાં, `Profile` component તેના child component, `Avatar` ને કોઈ props પાસ કરી રહ્યું નથી:

```js
export default function Profile() {
  return (
    <Avatar />
  );
}
```

તમે `Avatar` ને બે સ્ટેપ્સ (steps) માં કેટલાક props આપી શકો છો.

### Step 1: props ને child component મા પાસ કરવા {/*step-1-pass-props-to-the-child-component*/}

સોપ્રથમ, `Avatar`મા થોડા props ઉમેરો. ઉદાહરણ તરીકે, ચાલો બે props પાસ કરીએ: `person` (એક object), અને `size` (એક સંખ્યા/number):

```js
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

<Note>

જો `person=` પછીના ડબલ કરલી બ્રેસેસ (double curly braces) તમને મૂંઝવણમાં મૂકતા હોય, તો યાદ રાખો કે તે JSX curlies ની અંદર [માત્ર એક object છે]( /learn/javascript-in-jsx-with-curly-braces#using-double-curlies-css-and-other-objects-in-jsx).

</Note>

હવે તમે આ props ને `Avatar` component ની અંદર રીડ (read) કરી શકો છો.

### Step 2: Child component ની અંદર props રીડ કરવા {/*step-2-read-props-inside-the-child-component*/}

તમે આ props ને `function Avatar` ની બરાબર પછી `({` અને `})` ની અંદર અલ્પવિરામ (commas) દ્વારા અલગ પાડેલા `person, size` નામોની સૂચિ બનાવીને રીડ (read) કરી શકો છો. આ તમને `Avatar` કોડની અંદર ઉપયોગ કરવાની મંજૂરી આપે છે, જેમ તમે એક સામાન્ય variable નો ઉપયોગ કરો છો.

```js
function Avatar({ person, size }) {
  // person અને size અહીં ઉપલબ્ધ છે
}
```

રેન્ડરિંગ (rendering) માટે `person` અને `size` props નો ઉપયોગ કરે તેવું થોડું લોજિક (logic) `Avatar` માં ઉમેરો, અને બસ તમારું કામ થઈ ગયું.

હવે તમે અલગ-અલગ props સાથે ઘણી વિવિધ રીતે રેન્ડર કરવા માટે `Avatar` ને કન્ફિગર (configure) કરી શકો છો.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

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

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi', 
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Aklilu Lemma', 
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{ 
          name: 'Lin Lanying',
          imageId: '1bX5QH6'
        }}
      />
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
body { min-height: 120px; }
.avatar { margin: 10px; border-radius: 50%; }
```

</Sandpack>

Props તમને parent અને child components વિશે સ્વતંત્ર રીતે (independently) વિચારવાની મંજૂરી આપે છે. ઉદાહરણ તરીકે, તમે `Avatar` તેને કેવી રીતે ઉપયોગ કરે છે તેના વિશે વિચાર્યા વિના `Profile` ની અંદર `person` અથવા `size` props બદલી શકો છો. તેવી જ રીતે, તમે `Profile` ને જોયા વિના, `Avatar` આ props નો ઉપયોગ કેવી રીતે કરે છે તે બદલી શકો છો.

તમે props ને એવા "knobs" તરીકે વિચારી શકો છો જેને તમે એડજસ્ટ (adjust) કરી શકો છો. તેઓ એ જ ભૂમિકા ભજવે છે જે ભૂમિકા functions માટે arguments ભજવે છે—વાસ્તવમાં, props એ તમારા component માટેના *એકમાત્ર* argument છે! React component functions એક સિંગલ argument સ્વીકારે છે, જે એક `props` object હોય છે:

```js
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

સામાન્ય રીતે તમને આખા `props` ઓબ્જેક્ટ (object) ની જાતે જરૂર હોતી નથી, તેથી તમે તેને અલગ-અલગ props માં ડિસ્ટ્રક્ચર (destructure) કરો છો.

<Pitfall>

**જ્યારે તમે props ડિક્લેર (declare) કરો ત્યારે `(` અને `)` ની અંદર `{` અને `}` કરલી બ્રેસેસની જોડી (pair) ને ચૂકશો નહીં:**

```js
function Avatar({ person, size }) {
  // ...
}
```

આ સિન્ટેક્સને ["destructuring"](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_a_function_parameter) કહેવામાં આવે છે અને તે ફંક્શન પેરામીટર (function parameter) માંથી પ્રોપર્ટીઝ રીડ કરવા બરાબર જ છે:

```js
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

</Pitfall>

## Prop માટે default value નક્કી કરવી {/*specifying-a-default-value-for-a-prop*/}

જો તમે કોઈ prop માટે એક default value આપવા માંગો છો, જેથી જ્યારે કોઈ value પાસ ન કરવામાં આવી હોય ત્યારે તે બેકઅપ (fall back) તરીકે ઉપયોગમાં આવી શકે, તો તમે destructuring ફંક્શન પેરામીટરની બરાબર પછી `=` અને તેની default value મૂકીને આ કરી શકો છો:

```js
function Avatar({ person, size = 100 }) {
  // ...
}
```

હવે, જો `<Avatar person={...} />` ને કોઈ પણ `size` prop વગર રેન્ડર (render) કરવામાં આવે, તો તેની `size` આપમેળે `100` સેટ થઈ જશે.

Default value નો ઉપયોગ માત્ર ત્યારે જ થાય છે જ્યારે `size` prop ગેરહાજર (missing) હોય અથવા જો તમે `size={undefined}` પાસ કરો છો. પરંતુ જો તમે `size={null}` કે `size={0}` પાસ કરશો, તો default value નો ઉપયોગ કરવામાં **નહીં આવે**.

## JSX spread syntax નો ઉપયોગ કરીને props ફોરવર્ડ કરવા {/*forwarding-props-with-the-jsx-spread-syntax*/}

કેટલીકવાર, props પાસ કરવાનું કામ ખૂબ જ પુનરાવર્તિત (repetitive) થઈ જાય છે:

```js
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

પુનરાવર્તિત (repetitive) કોડ લખવામાં કંઈ ખોટું નથી—તે વાંચવામાં વધુ સરળ (legible) હોઈ શકે છે. પરંતુ કેટલીકવાર તમે કોડ ટૂંકો અને સ્પષ્ટ (conciseness) રાખવા માંગો છો. કેટલાક જુદા-જુદા components તેમના તમામ props તેમના children ને ફોરવર્ડ કરે છે, જેમ કે અહીં `Profile` component `Avatar` સાથે કરે છે. કારણ કે તેઓ તેમના કોઈપણ props નો સીધો ઉપયોગ કરતા નથી, તેથી વધુ ટૂંકી અને સ્પષ્ટ "spread" syntax નો ઉપયોગ કરવો વધુ યોગ્ય રહેશે:

```js
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

આ રીત દરેક prop નું નામ અલગથી લિસ્ટ કર્યા વિના `Profile` ના તમામ props ને સીધા જ `Avatar` પર ફોરવર્ડ કરે છે.

**Spread syntax નો ઉપયોગ સમજી-વિચારીને મર્યાદિત પ્રમાણમાં કરો.** જો તમે આનો ઉપયોગ તમારા દરેક અન્ય component માં કરી રહ્યા છો, તો તેનો અર્થ એ કે કંઈક ભૂલ થઈ રહી છે. મોટેભાગે, તે દર્શાવે છે કે તમારે તમારા components ને અલગ ભાગોમાં વિભાજીત (split) કરવા જોઈએ અને children ને JSX તરીકે પાસ કરવા જોઈએ. તેના વિશે વધુ આગળ જુઓ!

## JSX ને children તરીકે પાસ કરવું {/*passing-jsx-as-children*/}

બિલ્ટ-ઇન બ્રાઉઝર ટેગ્સ (built-in browser tags) ને એકબીજાની અંદર નેસ્ટ (nest) કરવા તે સામાન્ય છે:

```js
<div>
  <img />
</div>
```

કેટલીકવાર તમે તમારા પોતાના બનાવેલા custom components ને પણ આ જ રીતે નેસ્ટ (nest) કરવા માંગો છો:

```js
<Card>
  <Avatar />
</Card>
```

જ્યારે તમે કોઈ JSX ટેગની અંદર કન્ટેન્ટને નેસ્ટ (nest) કરો છો, ત્યારે parent component ને તે કન્ટેન્ટ `children` નામના એક ખાસ prop માં મળે છે. ઉદાહરણ તરીકે, નીચે આપેલું `Card` component એક `children` prop સ્વીકારશે જેની value `<Avatar />` સેટ કરેલી હશે, અને તે તેને એક wrapper div ની અંદર રેન્ડર (render) કરશે:

<Sandpack>

```js src/App.js
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

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
```

```js src/Avatar.js
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
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

Card component તેની અંદરના nested content ને કેવી રીતે wrap કરી શકે છે તે જોવા માટે, Card ની અંદર રહેલા <Avatar> ને કોઈ text સાથે replace કરીને જુઓ. તેની અંદર શું render થઈ રહ્યું છે તે આ component એ જાણવાની કોઈ જરૂર નથી. તમને આ flexible pattern ઘણી જગ્યાએ જોવા મળશે.

તમે children prop ધરાવતા component ને એક એવા "hole" તરીકે વિચારી શકો છો જેને તેના parent components દ્વારા arbitrary JSX વડે "fill" કરી શકાય છે. તમે visual wrappers જેવા કે: panels, grids વગેરે માટે અવારનવાર children prop નો ઉપયોગ કરશો.

<Illustration src="/images/docs/illustrations/i_children-prop.png" alt='A puzzle-like Card tile with a slot for "children" pieces like text and Avatar' />

## સમય સાથે props કેવી રીતે change થાય છે {/*how-props-change-over-time*/}

નીચે આપેલું Clock component તેના parent component માંથી બે props મેળવે છે: color અને time. (parent component નો code અહીં omit કરેલ છે કારણ કે તે state નો ઉપયોગ કરે છે, જેના વિશે આપણે હમણાં ઊંડાણમાં ચર્ચા નહીં કરીએ.)

નીચે આપેલા select box માં color change કરીને જુઓ:

<Sandpack>

```js src/Clock.js active
export default function Clock({ color, time }) {
  return (
    <h1 style={{ color: color }}>
      {time}
    </h1>
  );
}
```

```js src/App.js hidden
import { useState, useEffect } from 'react';
import Clock from './Clock.js';

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}

export default function App() {
  const time = useTime();
  const [color, setColor] = useState('lightcoral');
  return (
    <div>
      <p>
        Pick a color:{' '}
        <select value={color} onChange={e => setColor(e.target.value)}>
          <option value="lightcoral">lightcoral</option>
          <option value="midnightblue">midnightblue</option>
          <option value="rebeccapurple">rebeccapurple</option>
        </select>
      </p>
      <Clock color={color} time={time.toLocaleTimeString()} />
    </div>
  );
}
```

</Sandpack>

આ ઉદાહરણ દર્શાવે છે કે કોઈપણ component સમય જતાં અલગ-અલગ props મેળવી શકે છે. Props હંમેશા static હોતા નથી! અહીં, time prop દરેક સેકન્ડે change થાય છે, અને જ્યારે તમે બીજો કોઈ color select કરો છો ત્યારે color prop change થાય છે. Props માત્ર શરૂઆતના સમયના બદલે, કોઈપણ સમયે component ના ડેટાને reflect કરે છે.

જો કે, props એ immutable હોય છે—જે computer science નો એક શબ્દ છે જેનો અર્થ "unchangeable" (બદલી ન શકાય તેવું) થાય છે. જ્યારે કોઈ component ને તેના પોતાના props change કરવાની જરૂર પડે (ઉદાહરણ તરીકે, કોઈ user interaction અથવા નવા ડેટાના response માં), ત્યારે તેણે તેના parent component ને different props—એટલે કે એક નવો object pass કરવા માટે "ask" કરવું (કહેવું) પડશે! તેના જૂના props ત્યારપછી સાઇડમાં મુકાઈ જશે, અને અંતે JavaScript engine તેમના દ્વારા રોકાયેલી memory ને reclaim (પાછી મેળવી) લેશે.

ક્યારેય "props change" કરવાનો પ્રયત્ન ન કરો. જ્યારે તમારે કોઈ user input નો response આપવો હોય (જેમ કે select કરેલો color change કરવો), ત્યારે તમારે "state set" કરવાની જરૂર પડશે, જેના વિશે તમે શીખી શકો છો...[State: A Component's Memory.](/learn/state-a-components-memory)

<Recap>

* props pass કરવા માટે, તેમને JSX માં ઉમેરો, જેમ તમે HTML attributes સાથે કરો છો.
* props ને read કરવા માટે, function Avatar({ person, size }) destructuring syntax નો ઉપયોગ કરો.
* તમે size = 100 જેવી default value specify કરી શકો છો, જે missing અને undefined props માટે ઉપયોગમાં લેવાય છે.
* તમે <Avatar {...props} /> JSX spread syntax ની મદદથી બધા જ props ને forward કરી શકો છો, પણ તેનો અતિશય ઉપયોગ ન કરો!
* <Card><Avatar /></Card> જેવું nested JSX એ Card component ના children prop તરીકે દેખાશે.
* Props એ સમયના read-only snapshots છે: દરેક render ને props નું એક નવું version મળે છે.
* તમે props ને change કરી શકતા નથી. જ્યારે તમારે interactivity ની જરૂર હોય, ત્યારે તમારે state set કરવાની જરૂર પડશે.

</Recap>



<Challenges>

#### Extract a component {/*extract-a-component*/}

આ Gallery component બે profiles માટે ખૂબ જ મળતું આવતું (similar) markup ધરાવે છે. Duplication ને ઓછું કરવા માટે આમાંથી એક Profile component ને extract કરો. તેને કયા props pass કરવા તે તમારે નક્કી કરવું પડશે.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

export default function Gallery() {
  return (
    <div>
      <h1>Notable Scientists</h1>
      <section className="profile">
        <h2>Maria Skłodowska-Curie</h2>
        <img
          className="avatar"
          src={getImageUrl('szV5sdG')}
          alt="Maria Skłodowska-Curie"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Profession: </b> 
            physicist and chemist
          </li>
          <li>
            <b>Awards: 4 </b> 
            (Nobel Prize in Physics, Nobel Prize in Chemistry, Davy Medal, Matteucci Medal)
          </li>
          <li>
            <b>Discovered: </b>
            polonium (chemical element)
          </li>
        </ul>
      </section>
      <section className="profile">
        <h2>Katsuko Saruhashi</h2>
        <img
          className="avatar"
          src={getImageUrl('YfeOqp2')}
          alt="Katsuko Saruhashi"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Profession: </b> 
            geochemist
          </li>
          <li>
            <b>Awards: 2 </b> 
            (Miyake Prize for geochemistry, Tanaka Prize)
          </li>
          <li>
            <b>Discovered: </b>
            a method for measuring carbon dioxide in seawater
          </li>
        </ul>
      </section>
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(imageId, size = 's') {
  return (
    'https://i.imgur.com/' +
    imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

<Hint>

કોઈ એક scientist માટેના markup ને extract કરવાથી શરૂઆત કરો. ત્યારપછી, એવા pieces (ટુકડાઓ) શોધો જે બીજા ઉદાહરણ સાથે match નથી થતા, અને તેમને props દ્વારા configurable બનાવો.

</Hint>

<Solution>

આ solution માં, Profile component એકસાથે multiple props accept કરે છે: imageId (એક string), name (એક string), profession (એક string), awards (strings નો એક array), discovery (એક string), અને imageSize (એક number).

નોંધ કરો કે imageSize prop એક default value ધરાવે છે, જેના કારણે જ આપણે તેને component માં pass કરતા નથી.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Profile({
  imageId,
  name,
  profession,
  awards,
  discovery,
  imageSize = 70
}) {
  return (
    <section className="profile">
      <h2>{name}</h2>
      <img
        className="avatar"
        src={getImageUrl(imageId)}
        alt={name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li><b>Profession:</b> {profession}</li>
        <li>
          <b>Awards: {awards.length} </b>
          ({awards.join(', ')})
        </li>
        <li>
          <b>Discovered: </b>
          {discovery}
        </li>
      </ul>
    </section>
  );
}

export default function Gallery() {
  return (
    <div>
      <h1>Notable Scientists</h1>
      <Profile
        imageId="szV5sdG"
        name="Maria Skłodowska-Curie"
        profession="physicist and chemist"
        discovery="polonium (chemical element)"
        awards={[
          'Nobel Prize in Physics',
          'Nobel Prize in Chemistry',
          'Davy Medal',
          'Matteucci Medal'
        ]}
      />
      <Profile
        imageId='YfeOqp2'
        name='Katsuko Saruhashi'
        profession='geochemist'
        discovery="a method for measuring carbon dioxide in seawater"
        awards={[
          'Miyake Prize for geochemistry',
          'Tanaka Prize'
        ]}
      />
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(imageId, size = 's') {
  return (
    'https://i.imgur.com/' +
    imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

નોંધ કરો કે જો awards એક array હોય, તો તમારે એક અલગ awardCount prop ની કોઈ જરૂર નથી. ત્યારપછી તમે awards ની સંખ્યા ગણવા માટે awards.length નો ઉપયોગ કરી શકો છો. યાદ રાખો કે props કોઈ પણ values સ્વીકારી શકે છે, અને તેમાં arrays નો પણ સમાવેશ થાય છે!

બીજું એક solution, જે આ પેજ પરના અગાઉના ઉદાહરણોને વધુ મળતું આવે છે, તે એ છે કે કોઈ વ્યક્તિ વિશેની બધી જ માહિતીને એક સિંગલ object માં group કરવી, અને તે object ને એક સિંગલ prop તરીકે pass કરવો:

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Profile({ person, imageSize = 70 }) {
  const imageSrc = getImageUrl(person)

  return (
    <section className="profile">
      <h2>{person.name}</h2>
      <img
        className="avatar"
        src={imageSrc}
        alt={person.name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li>
          <b>Profession:</b> {person.profession}
        </li>
        <li>
          <b>Awards: {person.awards.length} </b>
          ({person.awards.join(', ')})
        </li>
        <li>
          <b>Discovered: </b>
          {person.discovery}
        </li>
      </ul>
    </section>
  )
}

export default function Gallery() {
  return (
    <div>
      <h1>Notable Scientists</h1>
      <Profile person={{
        imageId: 'szV5sdG',
        name: 'Maria Skłodowska-Curie',
        profession: 'physicist and chemist',
        discovery: 'polonium (chemical element)',
        awards: [
          'Nobel Prize in Physics',
          'Nobel Prize in Chemistry',
          'Davy Medal',
          'Matteucci Medal'
        ],
      }} />
      <Profile person={{
        imageId: 'YfeOqp2',
        name: 'Katsuko Saruhashi',
        profession: 'geochemist',
        discovery: 'a method for measuring carbon dioxide in seawater',
        awards: [
          'Miyake Prize for geochemistry',
          'Tanaka Prize'
        ],
      }} />
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
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

જો કે તેનો syntax થોડો અલગ દેખાય છે કારણ કે તમે JSX attributes ના collection ના બદલે એક JavaScript object ની properties ને describe કરી રહ્યા છો, છતાં આ ઉદાહરણો મોટે ભાગે સમાન (equivalent) જ છે, અને તમે બંનેમાંથી કોઈપણ approach પસંદ કરી શકો છો.

</Solution>

#### Props ના આધારે image નું size adjust કરો {/*adjust-the-image-size-based-on-a-prop*/}

આ ઉદાહરણમાં, Avatar એક numeric size prop મેળવે છે જે <img> ની width અને height નક્કી કરે છે. આ ઉદાહરણમાં size prop ને 40 પર set કરવામાં આવ્યો છે. જો કે, જો તમે આ image ને કોઈ નવી tab માં open કરશો, તો તમે નોંધ કરશો કે image પોતે ઘણી મોટી (160 pixels) છે. Real image size એ તેના પરથી નક્કી થાય છે કે તમે કઈ thumbnail size ની request કરી રહ્યા છો.

size prop ના આધારે સૌથી નજીકનું image size request કરવા માટે Avatar component માં change કરો. ખાસ કરીને, જો size એ 90 કરતાં ઓછું હોય, તો getImageUrl function માં 'b' ("big") ના બદલે 's' ("small") pass કરો. size prop ની અલગ-અલગ values સાથે avatars ને render કરીને અને images ને નવી tab માં open કરીને ખાતરી કરો કે તમારા ફેરફારો બરાબર કામ કરે છે કે નહીં.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person, 'b')}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <Avatar
      size={40}
      person={{ 
        name: 'Gregorio Y. Zara', 
        imageId: '7vQD0fP'
      }}
    />
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

<Solution>

તમે આ રીતે આગળ વધી શકો છો:

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  let thumbnailSize = 's';
  if (size > 90) {
    thumbnailSize = 'b';
  }
  return (
    <img
      className="avatar"
      src={getImageUrl(person, thumbnailSize)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <>
      <Avatar
        size={40}
        person={{ 
          name: 'Gregorio Y. Zara', 
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={120}
        person={{ 
          name: 'Gregorio Y. Zara', 
          imageId: '7vQD0fP'
        }}
      />
    </>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

તમે એકાઉન્ટ મા [window.devicePixelRatio] ને ધ્યાનમાં લઈને high DPI screens માટે વધુ sharp image પણ દર્શાવી શકો છો(https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio):

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

const ratio = window.devicePixelRatio;

function Avatar({ person, size }) {
  let thumbnailSize = 's';
  if (size * ratio > 90) {
    thumbnailSize = 'b';
  }
  return (
    <img
      className="avatar"
      src={getImageUrl(person, thumbnailSize)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <>
      <Avatar
        size={40}
        person={{ 
          name: 'Gregorio Y. Zara', 
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={70}
        person={{ 
          name: 'Gregorio Y. Zara', 
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={120}
        person={{ 
          name: 'Gregorio Y. Zara', 
          imageId: '7vQD0fP'
        }}
      />
    </>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

Props તમને આ પ્રકારની logic ને Avatar component ની અંદર encapsulate કરવાની (અને જો પછીથી જરૂર પડે તો તેને change કરવાની) સુવિધા આપે છે, જેથી કરીને દરેક વ્યક્તિ images કેવી રીતે request થાય છે અને resize થાય છે તેના વિશે વિચાર્યા વગર જ <Avatar> component નો ઉપયોગ કરી શકે.

</Solution>

#### `children` prop માં JSX pass કરવું {/*passing-jsx-in-a-children-prop*/}

નીચે આપેલા markup માંથી એક Card component ને extract કરો, અને તેને અલગ-અલગ JSX pass કરવા માટે children prop નો ઉપયોગ કરો:

<Sandpack>

```js
export default function Profile() {
  return (
    <div>
      <div className="card">
        <div className="card-content">
          <h1>Photo</h1>
          <img
            className="avatar"
            src="https://i.imgur.com/OKS67lhm.jpg"
            alt="Aklilu Lemma"
            width={70}
            height={70}
          />
        </div>
      </div>
      <div className="card">
        <div className="card-content">
          <h1>About</h1>
          <p>Aklilu Lemma was a distinguished Ethiopian scientist who discovered a natural treatment to schistosomiasis.</p>
        </div>
      </div>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

<Hint>

તમે component ના tag ની અંદર જે કોઈપણ JSX મૂકો છો, તે તે component ને children prop તરીકે pass કરવામાં આવશે.

</Hint>

<Solution>

તમે બંને જગ્યાએ Card component નો ઉપયોગ આ રીતે કરી શકો છો:

<Sandpack>

```js
function Card({ children }) {
  return (
    <div className="card">
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

export default function Profile() {
  return (
    <div>
      <Card>
        <h1>Photo</h1>
        <img
          className="avatar"
          src="https://i.imgur.com/OKS67lhm.jpg"
          alt="Aklilu Lemma"
          width={100}
          height={100}
        />
      </Card>
      <Card>
        <h1>About</h1>
        <p>Aklilu Lemma was a distinguished Ethiopian scientist who discovered a natural treatment to schistosomiasis.</p>
      </Card>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

જો તમે ઇચ્છો કે દરેક Card પાસે હંમેશા એક title હોય, તો તમે title ને એક અલગ prop પણ બનાવી શકો છો:

<Sandpack>

```js
function Card({ children, title }) {
  return (
    <div className="card">
      <div className="card-content">
        <h1>{title}</h1>
        {children}
      </div>
    </div>
  );
}

export default function Profile() {
  return (
    <div>
      <Card title="Photo">
        <img
          className="avatar"
          src="https://i.imgur.com/OKS67lhm.jpg"
          alt="Aklilu Lemma"
          width={100}
          height={100}
        />
      </Card>
      <Card title="About">
        <p>Aklilu Lemma was a distinguished Ethiopian scientist who discovered a natural treatment to schistosomiasis.</p>
      </Card>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

</Solution>

</Challenges>
