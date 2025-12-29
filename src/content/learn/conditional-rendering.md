---
title: શરતી રેન્ડરિંગ
---

<Intro>

તમારા કમ્પોનેન્ટસને ઘણીવાર વિવિધ શરતો અનુસાર અલગ-અલગ વસ્તુઓ દર્શાવવાની જરૂર પડશે. React માં, તમે જાવાસ્ક્રિપ્ટ ની `if` સ્ટેટમેન્ટ્સ, `&&`, અને `? :` ઓપરેટર્સનો ઉપયોગ કરીને શરતી રીતે JSX રેન્ડર કરી શકો છો.

</Intro>

<YouWillLearn>

* શરત પર આધાર રાખી અલગ JSX કેવી રીતે રીટર્ન કરવી.
* JSX ના ભાગને શરતી રીતે કેવી રીતે ઉમેરવો અથવા કાઢી નાખવો.
* React કોડબેઝમાં તમને મળતી સામાન્ય શરત આધારિત સિન્ટેક્સ શોર્ટકટ્સ.

</YouWillLearn>

## શરતી રીતે JSX રીટર્ન કરવું {/*conditionally-returning-jsx*/}

માનો કે તમારી પાસે `PackingList` નામનું કમ્પોનેન્ટ છે, જે અમુક `Item`s રેન્ડર કરે છે, જેમને packed તરીકે અથવા not packed તરીકે ચિહ્નિત કરી શકાય છે:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
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

ધ્યાન આપો કે અમુક `Item` કમ્પોનેન્ટસમાં `isPacked` prop `true` તરીકે સેટ છે, જ્યારે અમુકમાં `false` છે. જો `isPacked={true}` હોય, તો packed કરેલા items માટે તમે ચેકમાર્ક (✅) કરી શકો છો.

તમે આને [`if`/`else` સ્ટેટમેન્ટ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) તરીકે આ રીતે લખી શકો છો:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

જો `isPacked` prop `true` છે, તો આ કોડ **અલગ JSX tree રીટર્ન કરશે.** આ બદલાવના કારણે, અમુક items ના અંતે ચેકમાર્ક(✅) જોવા મળશે.

<Sandpack>

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✅</li>;
  }
  return <li className="item">{name}</li>;
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

પ્રત્યેક કિસ્સામાં શું રીટર્ન થાય છે તે એડિટ કરવાનો પ્રયત્ન કરો, અને પરિણામ કેવી રીતે બદલાય છે તે જુઓ!

નોધો કે તમે જાવાસ્ક્રિપ્ટ ના `if` અને `return` સ્ટેટમેન્ટ્સ સાથે ભેગું લોજિક બનાવો છો. React માં, કંટ્રોલ ફ્લો (જેમ કે શરતો) જાવાસ્ક્રિપ્ટ દ્વારા નિયંત્રિત થાય છે.

### શરતી રીતે `null` સાથે કશું જ રીટર્ન કરશે નહીં. {/*conditionally-returning-nothing-with-null*/}

અમુક પરિસ્થિતિઓમાં, તમે કશું પણ રેન્ડર કરવા માંગતા નથી. ઉદાહરણ તરીકે, માનો કે તમે packed કરેલ items દર્શાવવા માંગતા નથી. એક કમ્પોનેન્ટને કંઈક રીટર્ન કરવું જરૂરી છે. આ કિસ્સામાં, તમે `null` રીટર્ન કરી શકો છો:

```js
if (isPacked) {
  return null;
}
return <li className="item">{name}</li>;
```

જો `isPacked` true છે, તો કમ્પોનેન્ટ કશું પણ રીટર્ન નહીં કરે, એટલે કે `null`. અન્યથા, તે JSX રેન્ડર કરવા માટે રીટર્ન કરશે.

<Sandpack>

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
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

વ્યવહારમાં, એક કમ્પોનેન્ટમાંથી `null` રીટર્ન કરવું સામાન્ય નથી કારણ કે તે તેને રેન્ડર કરવાનો પ્રયાસ કરનાર ડેવલપરને આશ્ચર્યમાં મૂકશે. વધારે વાર, તમે પેરેન્ટ કમ્પોનેન્ટના JSX માં કમ્પોનેન્ટને શરતસર શામેલ કરવા કે બહાર કરવા પસંદ કરશો. તે કેવી રીતે કરવું તે અહીં છે!

## શરતી રીતે JSX સમાવિષ્ટ કરવું {/*conditionally-including-jsx*/}

પહેલાના ઉદાહરણમાં, તમે નિયંત્રિત કર્યું હતું કે કયું (અથવા કંઈ નહીં!) JSX tree કમ્પોનેન્ટ દ્વારા રીટર્ન થશે. તમે કદાચ રેન્ડર આઉટપુટમાં થોડી પુનરાવર્તન જોઈ હશે.

```js
<li className="item">{name} ✅</li>
```

આ ખૂબ સમાન છે

```js
<li className="item">{name}</li>
```

બન્ને શરતી શાખાઓ `<li className="item">...</li>` રીટર્ન કરે છે.

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

જોકે આ પુનરાવૃત્તિ નુકસાનકારક નથી, તે તમારા કોડને જાળવવા માટે મુશ્કેલ બનાવી શકે છે. જો તમારે `className` બદલવું પડે, તો તમારે તમારા કોડની બે અલગ અલગ જગ્યાએ આ ફેરફાર કરવો પડશે! આવી પરિસ્થિતિમાં, તમે તમારા કોડને વધુ [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) બનાવવા માટે શરતી રીતે થોડું JSX સામેલ કરી શકો છો.

### શરતી (ટર્નરી) ઓપરેટર (`? :`) {/*conditional-ternary-operator--*/}

જાવાસ્ક્રિપ્ટ માં શરતી એક્સપ્રેશન લખવા માટે સંક્ષિપ્ત સિનટેક્સ છે - [`શરતી ઓપરેટર`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) અથવા "ટર્નરી ઓપરેટર".

આના બદલે:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

તમે આ લખી શકો છો:

```js
return (
  <li className="item">
    {isPacked ? name + ' ✅' : name}
  </li>
);
```

તમે આ રીતે વાંચી શકો છો: *"જો `isPacked` true છે, તો (`?`) `name + ' ✅'` રેન્ડર કરો, અન્યથા (`:`) `name` રેન્ડર કરો"*.

<DeepDive>

#### શું આ બે ઉદાહરણ સંપૂર્ણ રીતે સમાન છે? {/*are-these-two-examples-fully-equivalent*/}

જો તમે ઓબ્જેક્ટ-ઓરિએન્ટેડ પ્રોગ્રામિંગ પૃષ્ઠભૂમિમાંથી આવી રહ્યા છો, તો તમે માની શકો છો કે ઉપરના બંને ઉદાહરણો થોડા અલગ છે , કારણ કે એક `<li>` ના બે અલગ-અલગ "instances" બનાવે છે. પણ JSX એલીમેન્ટ્સ "instances" નથી, કારણ કે તે કોઈ આંતરિક state ધરાવતા નથી અને એ સાચા DOM નોડ્સ નથી. તે લાઇટવેઇટ વર્ણનાઓ છે, જેમ કે બ્લૂપ્રિન્ટ. તેથી આ બે ઉદાહરણો વાસ્તવમાં સંપૂર્ણ રીતે સમાન છે. [સ્ટેટ જાળવવું અને ફરીથી સેટ કરવું](/learn/preserving-and-resetting-state) એ કેવી રીતે કામ કરે છે તેની વિગતવાર માહિતી આપવામાં આવી છે.

</DeepDive>

હવે માનો કે તમે પૂર્ણ થયેલા item ના ટેક્સ્ટને બીજાં HTML ટૅગ, જેમ કે `<del>`,માં વાપરવા માંગો છો જેથી તે સ્ટ્રાઇક થતું દેખાય. દરેક કિસ્સામાં વધુ JSX ને સારી રીતે nested રીતે ઉમેરવા માટે તમે newline અને parentheses ઉમેરી શકો છો.

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✅'}
        </del>
      ) : (
        name
      )}
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
આ શૈલી સરળ શરતો માટે સારી રીતે કામ કરે છે, પરંતુ તેને મર્યાદિત પ્રમાણમાં જ ઉપયોગ કરો. જો તમારા કમ્પોનેન્ટમાં ખૂબ Nested શરતી માર્કઅપને કારણે ગૂંચવણ થાય, તો વસ્તુઓને સ્પષ્ટ કરવા માટે ચાઈલ્ડ કમ્પોનેન્ટને અલગ કાઢવાનું વિચારવું જોઈએ. React માં, માર્કઅપ તમારા કોડનો જ ભાગ હોય છે, તેથી તમે કૉમ્પ્લેક્સ એક્સપ્રેશન્‍સ ને સરળ બનાવવા માટે વેરીએબલ્સ અને ફંક્શન્સ જેવા ટૂલ્સનો ઉપયોગ કરી શકો છો.

### લોજિકલ AND ઑપરેટર (`&&`) {/*logical-and-operator-*/}

React કમ્પોનેન્ટસમાં તમે સામાન્ય રીતે [જાવાસ્ક્રિપ્ટ લોજિકલ AND (`&&`) ઓપરેટર](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#:~:text=The%20logical%20AND%20(%20%26%26%20)%20operator,it%20returns%20a%20Boolean%20value.) નો ઉપયોગ કરશો.આનો ઉપયોગ ત્યારે થાય છે જ્યારે તમારી શરત સાચી હોય ત્યારે અમુક JSX રેન્ડર કરવા માંગતા હો, **અથવા અન્યથા કશું જ રેન્ડર ન કરો.** `&&` ની મદદથી, તમે `isPacked` `true` હોય ત્યારે જ ચેકમાર્ક શરતી રીતે રેન્ડર કરી શકો:

```js
return (
  <li className="item">
    {name} {isPacked && '✅'}
  </li>
);
```

તમે આને આ રીતે વાંચી શકો છો *"જો `isPacked` હોય, તો (`&&`) ચેકમાર્ક રેન્ડર કરો, અન્યથા કશું જ રેન્ડર ન કરો"*.

આ છે તેનું કાર્યરત ઉદાહરણ:

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
 
એક [જાવાસ્ક્રિપ્ટ && એક્સપ્રેશન્‍સ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND) એ તેના જમણા ભાગની value પરત કરે છે (આપના કેસમાં, ચેકમાર્ક), જો ડાબો ભાગ (આપની શરત) `true` હોય. પરંતુ જો શરત `false` હોય, તો આખી expression `false` બની જાય છે. React `false` ને JSX tree માં "ખાલી જગ્યા" તરીકે માને છે, જેમ કે `null` અથવા `undefined`, અને તેની જગ્યાએ કશું જ રેન્ડર કરતું નથી.

<Pitfall>

**નંબરને `&&` ની ડાબી બાજુ પર ન મૂકો.**

શરતને તપાસવા માટે, જાવાસ્ક્રિપ્ટ ડાબા ભાગને આપમેળે બૂલિયનમાં પરિવર્તિત કરે છે. જો ડાબો ભાગ `0` હોય, તો આખી એક્સપ્રેશન્‍સ તેની value (`0`) મેળવશે, અને React ખુશીથી `0` રેન્ડર કરશે, કશું નહીં રેન્ડર કરવાનો બદલે.

ઉદાહરણ તરીકે, સામાન્ય ભૂલ એ છે કે તમે આ પ્રકારે કોડ લખો: `messageCount && <p>New messages</p>`. સાહજિક રીતે એવું માનવું સરળ છે કે જ્યારે `messageCount` `0` હોય ત્યારે તે કશું જ રેન્ડર નહીં કરે, પરંતુ વાસ્તવમાં તે `0` જ રેન્ડર કરે છે!

તેને ઠીક કરવા માટે, ડાબો ભાગ બુલિયન બનાવો: `messageCount > 0 && <p>New messages</p>`.

</Pitfall>

### JSX ને શરતી રીતે વેરીએબલને ફાળવવું {/*conditionally-assigning-jsx-to-a-variable*/}

જ્યારે શોર્ટકટ્સ સરળ કોડ લખવામાં અવરોધ રૂપ બને ત્યારે, `if` સ્ટેટમેન્ટ અને વેરીએબલનો ઉપયોગ કરવાનો પ્રયત્ન કરો. તમે [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) વડે વ્યાખ્યાયિત વેરીએબલ્સને ફરી ફાળવી શકો છો, એટલે કે, તમે દર્શાવવા માંગતા ડિફોલ્ટ કન્ટેન્ટથી પ્રારંભ કરો, જેમ કે name:

```js
let itemContent = name;
```

`if` સ્ટેટમેન્ટનો ઉપયોગ કરીને JSX એક્સપ્રેશનને `itemContent`માં ફરી ફાળવો જો `isPacked` `true` હોય:

```js
if (isPacked) {
  itemContent = name + " ✅";
}
```

[કૌંસો (curly braces) "જાવાસ્ક્રિપ્ટમાં વિન્ડો" ખોલે છે.](/learn/javascript-in-jsx-with-curly-braces#using-curly-braces-a-window-into-the-javascript-world) રીટર્ન કરેલા JSX tree માં કૌંસ સાથે વેરિએબલ એમ્બેડ કરો,અને અગાઉ ગણવામાં આવેલ એક્સપ્રેશન JSX ની અંદર nest કરો:

```js
<li className="item">
  {itemContent}
</li>
```

આ શૈલી સૌથી વધારે વર્બોઝ (Verbose) છે, પરંતુ તે સૌથી વધુ લવચીક પણ છે. અહીં તે પ્રયોગમાં છે:

<Sandpack>

```js
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✅";
  }
  return (
    <li className="item">
      {itemContent}
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
  
અગાઉની જેમ, આ માત્ર ટેક્સ્ટ માટે જ નહીં પરંતુ મનચાહું JSX માટે પણ કાર્ય કરે છે:

<Sandpack>

```js
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = (
      <del>
        {name + " ✅"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
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

જો તમે જાવાસ્ક્રિપ્ટથી પરિચિત નથી, તો આ શૈલીઓની વિવિધતા પ્રથમ દ્રષ્ટિએ બહુ જટિલ લાગી શકે છે. તેમ છતાં, તેને શીખવાથી તમને કોઇ પણ જાવાસ્ક્રિપ્ટ કોડ -- અને ફક્ત React કમ્પોનેન્ટસ જ નહિ -- વાંચવા અને લખવામાં મદદ મળશે! શરૂઆતમાં જે શૈલી તમને યોગ્ય લાગતી હોય, તે પસંદ કરો, અને પછી જો બીજાની કાર્યપદ્ધતિ ભૂલી જાવ તો આ સંદર્ભ ફરીથી તપાસો.

<Recap>
 
* React માં, તમે branching logic ને જાવાસ્ક્રિપ્ટની મદદથી નિયંત્રિત કરી શકો છો.
* તમે `if` statement નો ઉપયોગ કરીને JSX શરતી રીતે રીટર્ન કરી શકો છો.
* તમે શરતો મુજબ JSX ને વેરીએબલમાં સાચવી શકો છો અને પછી તેને બીજા JSX માં curly braces નો ઉપયોગ કરીને સમાવિષ્ટ કરી શકો છો.
* JSX માં, `{cond ? <A /> : <B />}`નો અર્થ છે *"જો `cond` true હોય, તો `<A />` દર્શાવો, નહીં તો `<B />`"*.
* JSX માં, `{cond && <A />}`નો અર્થ છે *"જો `cond` true હોય, તો `<A />` દર્શાવો, નહીં તો કંઈ પણ ન દર્શાવો"*.
* આ શોર્ટકટ્સ સામાન્ય છે, પરંતુ તમે plain `if` નો ઉપયોગ કરવાની ઇચ્છા કરો છો તો તે તમારી ઇચ્છા છે.

</Recap>

<Challenges>

#### અપૂર્ણ items માટે આઇકન બતાવવા `? :` નો ઉપયોગ કરો. {/*show-an-icon-for-incomplete-items-with--*/}

`cond ? a : b` શરતનો ઉપયોગ કરીને, જયારે `isPacked` false હોય ત્યારે ❌ દર્શાવો.

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

<Solution>

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked ? '✅' : '❌'}
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

</Solution>

#### આઇટમનું મહત્વ દર્શાવવા માટે `&&` નો ઉપયોગ કરો. {/*show-the-item-importance-with-*/}

આ ઉદાહરણમાં, દરેક `Item` ને સંખ્યાત્મક `importance` prop પ્રાપ્ત થાય છે. `&&` ઓપરેટરનો ઉપયોગ "_(Importance: X)_" ઇટાલિકમાં દર્શાવવા માટે કરો, પરંતુ ફક્ત એવા items માટે જ, જેઓનું importance શૂન્ય કરતાં વધુ છે. તમારી item ની સૂચિ આ રીતે દેખાશે:

* Space suit _(Importance: 9)_
* Helmet with a golden leaf
* Photo of Tam _(Importance: 6)_

બે લેબલ વચ્ચે ખાલી જગ્યા ઉમેરવાનું ભૂલશો નહીં!

<Sandpack>

```js
function Item({ name, importance }) {
  return (
    <li className="item">
      {name}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          importance={9}
          name="Space suit"
        />
        <Item
          importance={0}
          name="Helmet with a golden leaf"
        />
        <Item
          importance={6}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<Solution>

આ યુક્તિ કામ કરી જવી જોઈએ:

<Sandpack>

```js
function Item({ name, importance }) {
  return (
    <li className="item">
      {name}
      {importance > 0 && ' '}
      {importance > 0 &&
        <i>(Importance: {importance})</i>
      }
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          importance={9}
          name="Space suit"
        />
        <Item
          importance={0}
          name="Helmet with a golden leaf"
        />
        <Item
          importance={6}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

ધ્યાનમાં રાખો કે તમારે `importance && ...` ના બદલે, `importance > 0 && ...` લખવું પડશે. જેથી જો `importance` 0 હોય, તો 0 પરિણામ તરીકે રેન્ડર ન થાય!

આ ઉકેલમાં, નામ અને મહત્વના લેબલ વચ્ચે ખાલી જગ્યા દાખલ કરવા માટે બે અલગ-અલગ શરતોનો ઉપયોગ કરવામાં આવે છે.વૈકલ્પિક રીતે, તમે આગળની જગ્યા સાથે ફ્રેગમેન્ટનો ઉપયોગ કરી શકો છો: `importance > 0 && <> <i>...</i></>` અથવા `<i>`ની અંદર તરત જ ખાલી જગ્યા ઉમેરો: `importance > 0 && <i> ...</i>`.

</Solution>

#### `? :` ની શ્રેણી ને `if` અને વેરીએબલમાં પુનર્ગઠિત કરો. {/*refactor-a-series-of---to-if-and-variables*/}

આ `Drink` કમ્પોનેન્ટ `? :` શરતોનો ઉપયોગ કરે છે જેથી તે `name` prop `"tea"` અથવા `"coffee"` પર આધાર રાખીને વિવિધ માહિતી દર્શાવે છે. સમસ્યા એ છે કે દરેક પીણાં વિશેની માહિતી વિભિન્ન શરતોમાં ફેલાઈ ગઈ છે. આ કોડને ત્રણ `? :` શરતોના બદલે એકમાત્ર `if` સ્ટેટમેન્ટનો ઉપયોગ કરીને પુનર્ગઠિત કરો.

<Sandpack>

```js
function Drink({ name }) {
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Part of plant</dt>
        <dd>{name === 'tea' ? 'leaf' : 'bean'}</dd>
        <dt>Caffeine content</dt>
        <dd>{name === 'tea' ? '15–70 mg/cup' : '80–185 mg/cup'}</dd>
        <dt>Age</dt>
        <dd>{name === 'tea' ? '4,000+ years' : '1,000+ years'}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

જ્યારે તમે કોડને `if` સ્ટેટમેન્ટ સાથે પુનર્ગઠિત કર્યું, ત્યારે શું તમને તેને વધુ સરળ બનાવવા માટે કોઈ અન્ય વિચારો આવે છે?

<Solution>

આ માટે ઘણા રસ્તા હોઈ શકે છે, પરંતુ અહીં એક શરૂઆતનો માર્ગ છે:

<Sandpack>

```js
function Drink({ name }) {
  let part, caffeine, age;
  if (name === 'tea') {
    part = 'leaf';
    caffeine = '15–70 mg/cup';
    age = '4,000+ years';
  } else if (name === 'coffee') {
    part = 'bean';
    caffeine = '80–185 mg/cup';
    age = '1,000+ years';
  }
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Part of plant</dt>
        <dd>{part}</dd>
        <dt>Caffeine content</dt>
        <dd>{caffeine}</dd>
        <dt>Age</dt>
        <dd>{age}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

અહીં દરેક પીણાં વિશેની માહિતી અલગ-અલગ શરતોમાં વિભાજિત કરવામાં આવવાને બદલે એકઠી કરી છે. આથી ભવિષ્યમાં વધુ પીણાં ઉમેરવાનું સરળ બની જાય છે.

બીજો ઉકેલ એ હોઈ શકે છે કે શરતને સંપૂર્ણપણે દૂર કરી ને માહિતી ઓબ્જેક્ટસમાં મૂકી શકાય:
<Sandpack>

```js
const drinks = {
  tea: {
    part: 'leaf',
    caffeine: '15–70 mg/cup',
    age: '4,000+ years'
  },
  coffee: {
    part: 'bean',
    caffeine: '80–185 mg/cup',
    age: '1,000+ years'
  }
};

function Drink({ name }) {
  const info = drinks[name];
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Part of plant</dt>
        <dd>{info.part}</dd>
        <dt>Caffeine content</dt>
        <dd>{info.caffeine}</dd>
        <dt>Age</dt>
        <dd>{info.age}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

</Solution>

</Challenges>
