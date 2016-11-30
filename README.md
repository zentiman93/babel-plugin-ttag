# babel-polyglot-plugin [WIP]
## project status - POC
Solution for providing gettext like translations into your project. Uses es6 native template syntax.

Plugin functions:
- extracting translations from es6 tagged templates to .pot 
- resolving translations from .po files right into your sources at compile time.

Key features:
- no additional formatting syntax(no sprintf), only es6 tagged templates.
- works with everything that works with babel. (can be used with react and jsx).
- can be integrated with gettext utility (generates .pot files, resolves translations from .po).
- possibility to precompile all translations into the browser bundles (no runtime resolving, all bundles are precompiled).
- support for plurals and contexts.

Here is the demo project (webpack setup). - https://github.com/AlexMost/polyglot-demo

Installation
============

`npm install babel-polyglot-plugin`

Example
=======
Here is how you code will look like while using this plugin:

```javascript
import { t } from 'polyglot';
const name = 'Mike';
console.log(t`Hello ${name}`);
```
So you can see that you can use native es6 template formatting. To make your string translatable, all you need to do is to place 't' tag.

Here is how you can handle plural forms:

```javascript
import { nt } from 'polyglot';
const name = 'Mike';
const n = 5;
console.log(nt(n)`Mike has ${n} bananas`);
```

Usage with react.js library:
```javascript
import React from 'react';
import { t, nt } from './polyglot';

class PluralDemo extends React.Component {
    constructor(props) {
        super(props);
        this.state = { count: 0 };
        this.countInc = this.countInc.bind(this);
    }
    countInc() {
        this.setState({ count: this.state.count + 1 });
    }
    render() {
        const n = this.state.count;
        return (
            <div>
                <h3>{ t`Deadly boring counter demo (but with plurals)` }</h3>
                <div>{ nt(n)`You have clicked ${n} times` }</div>
                <button onClick={this.countInc}>{ t`Click me` }</button>
            </div>
        )
    }
}

export default PluralDemo;
```

Configuration
=============
Here is the configuration object that you can specify in plugin options inside *.babelrc*:

```javascript
{
  // Specifies where to save extracted gettext entries (.pot) file
  // Plugin will be extracting gettext entries if '*extract*' property is present.
  extract: { output: 'dist/translations.pot' }, 
  
  // Specifies Which locale will be resolved currently (must be one of which is stored in 'locales' property)
  // Plugin will be resolving translations from .po file if '*resolve*' property is present.
  resolve: { locale: 'en-us' },
  
  // Map with locales and appropriate .po files with translations.
  locales: {
      'en-us': 'i18n/en.po',
      'ua': 'i18n/ua.po',
  }
}
```

Contribution
============
Feel free to contribute, make sure to cover your contributions with tests.
Test command:
```
make test
```

License
=======

[MIT License](LICENSE).
