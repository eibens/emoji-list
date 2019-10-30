# emoji list

This repository contains a list of all emoji metadata scraped from [this website *(WARNING: downloads 27 MB)*](https://unicode.org/emoji/charts/full-emoji-list.html). I made this because I could not find any readily available list with both names AND unicode codepoints. You can use this list together with your favorite emoji set:

- [emoji.json](emoji.json) (with formatting)
- [emoji.min.json](emoji.min.json) (without formatting)

The list is a JSON array of objects, each with a `category`, `subcategory`, `name`, and array of `codes`. Here are the first two emojis from [emoji.json](emoji.json) as an example:

```json
[
  {
    "category": "Smileys & Emotion",
    "subcategory": "face-smiling",
    "name": "grinning face",
    "codes": [
      "1F600"
    ]
  },
  {
    "category": "Smileys & Emotion",
    "subcategory": "face-smiling",
    "name": "grinning face with big eyes",
    "codes": [
      "1F603"
    ]
  }
]
```

## Do it yourself

You can export the list yourself, if you wish. The steps were tested and work in newest Firefox and Google Chrome:

1. Copy the source code below.
2. Go to [this website *(WARNING: downloads 27 MB)*](https://unicode.org/emoji/charts/full-emoji-list.html)
3. Open the browser console (<kbd>F12</kbd> or right click and `Inspect Element`).
4. Paste the source code into the console and press <kbd>Enter</kbd>.
5. Wait a few seconds until the console outputs `emojis gathered!`.
6. The emoji list is now in your clipboard and you can paste it anywhere.

```JavaScript
format = false // change to `true` if you want formatted output
copy((format => {
  function Xs (node, path) {
    const result = node.ownerDocument
      .evaluate(path, node, null, XPathResult.ORDERED_NODE_ITERATOR_TYPE, null)
    const nodes = []
    let n
    while (n = result.iterateNext()) nodes.push(n)
    return nodes
  }
  function XText (node, path) {
    return Xs(node, path)[0].textContent
  }
  const list = Xs(document.body, '//tr[td]').map(tr => ({ 
    category: XText(tr, '(preceding::tr/th[@class="bighead"])[last()]'),
    subcategory: XText(tr, '(preceding::tr/th[@class="mediumhead"])[last()]'),
    // must replace '⊛' in name of recently added emojis 
    name: XText(tr, 'td[last()]').replace(/⊛/, '').trim(),
    // multiple codes in single anchor tag
    codes: XText(tr, 'td[2]/a').split(' ').map(code => code.substr(2))
  }))
  return JSON.stringify(list, null, format ? 2 : null)
})(format))
console.log('emojis gathered!')
```

## Contact

:pencil: Lukas Eibensteiner<br>
:e-mail: [l.eibensteiner@gmail.com](mailto:l.eibensteiner@gmail.com)<br>
:package: [eibens/emoji-list](https://github.com/eibens/emoji-list)
